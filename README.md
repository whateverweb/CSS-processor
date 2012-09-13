# Serving CSS from wew.io

### Intro
The Extended Media Query engine will server-side evaluate media queries and adapt CSS to the end-user's current context. It supports both WURFL and custom capabilities. 

### Getting started
1. Sign up for a free account at [whateverweb.com](http://whateverweb.com/)
2. Register an application to get an application key and service URL.

### Example

#### Including the filter in your HTML

    <link rel="stylesheet" type="text/css" href="http://wew.io/css/http://mpulp.mobi/labs/wew/css/style.css" />

The URL pattern is: 'http://wew.io/css/cssURL'

#### Using the extended syntax in your CSS

All WURFL capabilities are available using the prefix '-mt-' as the example shown below

    @media all and (-mt-model_name: iPhone) {
        /* CSS styles just shown to the iPhone */
    }

### Details

#### Available features
- Extended Media Queries with support for WURFL capabilities and custom media features
- CSS minification using the [Yahoo YUI Compressor](http://developer.yahoo.com/yui/compressor/)
- GZip support

#### Available parameters
None.