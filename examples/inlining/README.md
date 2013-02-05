## Inlining CSS

With the WEW CSS-processor you can combine multiple CSS files into one using the @import-statement.

Sometimes it is useful and logic to split your layout into multiple CSS files. However, using the standard CSS import statement can easily impact page performance because browsers need to make more requests - one per imported resource.

The WEW CSS-processor tries to address this issue by rendering the imports on the server, combining the different resources into a single response. To enable the WEW CSS processor in your markup, simply wrap it around the 'main' CSS file:

	<link rel="stylesheet" href="http://css.demo.wew.io/http://demo.wew.io/main.css">

In this utterly simple example we combine three different stylesheets into one;

	- main.css
		+ left.css
		+ right.css

The 'left.css' and 'right.css' are imported at their position in the 'main.css'. (Please note that if you want to adhere strictly to the CSS specifications, you should put your @import-statements in the start of 'main.css')


In the next example we only want to include the 'right.css' for devices with a minimum display width of 480 px;

	- main-480.css
		+ left.css
		+ right.css

[Live Example](http://demo.wew.io/CSS-processor/examples/inlining/)
