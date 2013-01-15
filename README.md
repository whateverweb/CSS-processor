# Serving CSS from wew.io

## Intro
The CSS Processor is a server-side component used to optimize and process CSS media queries. It supports processing of both standard and 'extended' CSS media queries on the server, allowing CSS to be optimized and adjusted to the device/browser requesting it. The service allows you to optimize the CSS delivered with small, or no changes to your existing and standard CSS(3).

The import/inline feature enables you to break up your stylesheets into more logical fragments, and then reassemble them in a single request.

### Features
- minify CSS transferred by deleting sections not relevant, based on media queries
- conditional CSS based on capabilities resolved on the server using the WEW device repository
- strip irrelevant vendor prefixes for requesting browser/device
- inline/combine CSS fragments using 'extended' @import statement
- cache friendly, both browser cache and edge/proxy caches. Automatic Etag and headers
- automatic minification (YUI compressor) - except if application is in debug mode
- automatic compression (Gzip)


## Getting started
1. Sign up for a free account at [whateverweb.com](http://whateverweb.com/)
2. Register an application to get an application key and service URL.

### Including the filter in your HTML

    <link rel="stylesheet" type="text/css" href="http://demo.wew.io/css/http://mpulp.mobi/labs/wew/css/style.css" />

In the URL, replace 'demo' with your application's name, and point it to the CSS URL you want to process.

## Media query evaluation

The CSS Processor will consider the device requesting the CSS and attempt to strip away irrelevant stuff and optimise it as best it can.

The evaluation process is based on media queries. The processor will evaluate some of the standard W3C media queries and all of the extended media expressions offered. If, for some reason, the processor is unsure about the evaluation (typically it has one or more condition that the server cannot be sure about) the entire CSS block and media query is left as is.

#### Extended media expressions
The processor handles media expressions starting with '-wew-' by checking them against the capabilities in the [WEW Device Detection Service](https://github.com/whateverweb/device-detection). You can use any capability as a media expression, even custom ones you have defined using the WEW control panel.

	@media (-wew-pointing-method: touchscreen) and (-wew-myCustomCapability: myValue)

Negation can be achieved by prefixing the value with '!'. To match all non-touchscreen devices you could use:

	@media (-wew-pointing-method: !touchscreen)

For evaluation of numerical ranges, add the 'min' and 'max' prefixes, as you would for the standard media expressions. Please note this might look weird when the capability is named 'max-*', like in the example below. The example will match all devices with a 'max-image-width' over 300px:

	@media (-wew-min-max-image-width: 300px)

The extended media expressions can of course be mixed with standard ones:

	@media (-wew-min-max-image-width: 480px) and (min-device-pixel-ratio: 2)

#### Standard media expressions
The processor will attempt to evaluate standard media expressions against the WEW Device Detection Service if possible. These include:
* *min-width*, *max-width*
* *min-device-width*, *max-device-width*

After evaluation, the extended media expressions are stripped from the result, as they would make no sense to the browser.


#### Caveats
* Only media types 'all' and 'screen' are evaluated by the processor, and they are always evaluated to TRUE. For historical reasons, we do not even attempt to evaluate 'handheld' or other media types
* At the moment, media expressions dealing with resolutions will only work with pixels, and this is the default unit
* If the CSS to be evaluated has incorrect syntax (missing closing brace etc), it will most probably be served without being evaluated *at all*
* You may replace underscore character(s) with hyphens in capability names (for instance use '-wew-model-name' instead of '-wew-model_name')


## Imports

All '@import'-statements will be inlined, if possible. The CSS Processor supports using importing fragments throughout the documents and will insert the fragments where the '@import' is placed -- not just at the top of the documents.

	body {
		@import url('http://demo.wew.io/styles/resetStyles.css');
	}

Import statements can also contain WEW extended media queries:

	@import url('http://demo.wew.io/styles/iPadStyles.css') all and (-wew-model-name: iPad);


## Vendor prefix stripping

If browser is detected for the requesting device, the processor will strip away vendor prefixes for other browsers.
Recognized prefixes are: *-moz-*, *-webkit-*, *-o-*, *-ms-*

## Examples

Match touchscreen devices with a minimum device width of 320 pixels:

    @media screen and (min-device-width: 320px) and (-wew-pointing-method: touchscreen) {
    	/* ... */
    }


Match iPhones only:

    @media all and (-wew-model-name: iPhone) {
        /* CSS styles just shown to the iPhone */
    }

Matches touchscreen devices **or** devices a minimum of 600px max-image-width.

	@media (-wew-max-image-width: 600px) , (-wew-pointing-method: touchscreen) {
		font-size: 18px;
	}

The (uncompressed) output will be:

		font-size: 18px;


Negative matching of media expression

	@media (-wew-pointing_method: !touchscreen) {
		/* CSS for non-touchscreen devices here */
	}


##### Media Features – some Device Library properties

- is_tablet
- is_app
- is_desktop
- pointing_method
- brand_name
- device_os
- device_os_version
- dual_orientation
- css_gradient
- colors

Please see the [WEW Device Detection Service](https://github.com/whateverweb/device-detection) for more information.

