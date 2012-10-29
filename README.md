# Serving CSS from wew.io

### Intro
The Extended Media Query engine is a server-side Media Query parser which allows to check for a broader set of “media features” from the device database. It supports both standard WURFL capabilities and custom capabilities.

The CSS3 standard includes Media Queries so that we may use different styling based on certain conditions. Unfortunately, the browser might not interpret these queries like you expect it to. The available list of media features is also very limited compared to our device library.

Our extensive set of “media features” let you bring back the control to the designers. By letting you query all features of our device library you may successfully differentiate styling properties within the CSS. All this is processed server-side so that it also works on devices that don’t natively support Media Queries.



### Getting started
1. Sign up for a free account at [whateverweb.com](http://whateverweb.com/)
2. Register an application to get an application key and service URL.

### Example
See the official [CSS3 Media Queries specification](http://www.w3.org/TR/css3-mediaqueries/) for official information and examples

#### Including the filter in your HTML

    <link rel="stylesheet" type="text/css" href="http://wew.io/css/http://mpulp.mobi/labs/wew/css/style.css" />

The URL pattern is: 'http://wew.io/css/cssURL'

#### Using the extended syntax in your CSS

All WURFL capabilities are available using the prefix '-mt-' as the example shown below

    @media all and (-mt-model_name: iPhone) {
        /* CSS styles just shown to the iPhone */
    }

### Details
The MT extensions are annotated with the -mt- prefix like this:

	(-mt-pointing_method: touchscreen)

**Important information**

*Use the same unit as the Device Library when checking for features. E.g. if the Device Library specifies max_image_width as being 600 for a device one cannot write 600px in the media query.
The CSS MQ parser will try to evaluate numbers as float in case of a max/min match. E.g. 1.2 > 1.1.*

####Example usage

Matches touchscreen devices with a minimum of 600px max_image_width.

	@media (-mt-pointing_method: touchscreen) AND (-mt-min-max_image_width: 600) {
		font-size: 18px;
	}
Matches touchscreen devices or devices a minimum of 600px max_image_width.

	@media (-mt-pointing_method: touchscreen), (-mt-min-max_image_width: 600) {
		font-size: 18px;
	}
Negative matching. Negates the result of the full media query.

	@media NOT (-mt-pointing_method: touchscreen) {
		/* CSS for non-touchscreen devices here */
	}
The ONLY keyword is supported. In case of a syntax error our parser will output the media query in the CSS and ONLY should hide the style for older user agents.

	@media ONLY (-mt-pointing_method: touchscreen) {
		/* CSS for non-touchscreen devices here */
	}

####Mixing standard Media Queries with MT extensions

You should never mix standard CSS3 media types or media features with the extended media features within the same media query. You may however mix an extended media query with a CSS3 media query in a media query list like below.

	@media all and (min-width: 50), (-mt-pointing_method: touchscreen)
The following code is also valid and useful for mixing expressions.

	@media all and (min-width: 50) {
		.testClass {
			font-weight: bold;
			@media(-mt-min-xhtml_support_level: 2) {
				font-size: 50;
			}
		}
	}
If the expression `xhtml_support_level>2` returns true the output from Frame will look like this

	@media all and (min-width: 50) {
		.testClass {
			font-weight: bold;
			font-size: 50;
		}
	}

An example that won’t work is this. If the user-agent runs on a touchscreen device it will not be evaluated. However, as explained in Illegal mixed media queries that still may be positively evaluated it may evaluate as a positive FALSE.	

	@media handheld and (-mt-pointing_method: touchscreen) {
		.testClass {
			font-weight: bold;
		}
	}

####Media types

The  Media Query parser does only support the media type supported “ALL”. The reason for this is that the query “@media (-mt-pointing_method: touchscreen)” is the same as “@media all and (-mt-pointing_method: touchscreen)” according to the CSS3 Media Queries specification.

#####Media Features – some Device Library properties

- pointing_method
- is_tablet
- brand_name
- device_os
- device_os_version
- max_image_width
- max_image_height
- dual_orientation
- css_gradient
- colors

####Known pitfalls

Our parser is greedy.

This means that if you forget to write a closing bracket after your media query block it will end the block after the next end-bracket (“}”) it finds in your CSS file. The result being either that one more class is filtered out, or that the your CSS will contain a malformed class block.

Illegal mixed media queries that still may be positively evaluated

Consider the following media query when browsing from a regular web-browser on your computer

	@media handheld and (min-device-width: 1px) and (-mt-pointing_method: touchscreen) {
		.testClass {
			font-weight: bold;
		}
	}
	
Logically this will produce the following answers within our parser:

1. Undecided – handheld?
2. Undecided – min-device-width: 1px?
3. FALSE – -mt-pointing_method: touchscreen?

Considering that the Media Query standard states that all parts must be evaluated to true for .testClass to be output we have enough info to positively evaluate the expression as FALSE. Your browser is considered to have no touchscreen and the CSS block is stripped from the output.

Negating this result with the NOT-prefix will also work and result in the positively evaluated answer TRUE.

#### Available features
- Custom defined media features defined on whateverweb.com
- CSS minification using the [Yahoo YUI Compressor](http://developer.yahoo.com/yui/compressor/)
- GZip support