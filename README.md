A Webpack plugin to merge your files together, unedited, into a single file.

# Getting Started

```bash
npm install webpack-raw-bundler
```

# Usage

When you need to include a bunch of libraries, but don't have time to make the ordinary require statements, use this to bundle them all together so that their globals are actually global.
In other words, From the old:
``` html
	<script src="js/jquery.js"></script>
	<script src="js/silly.js"></script>
	<script>
		$(silly()); // Some function in silly.js's global scope
	</script>
```
To the new:
``` html
	<script src="js/bundle.js"></script>
	<script>
		$(silly()); 
	</script>
```


# Installing to the config
``` javascript

  var RawBundlerPlugin = require('webpack-raw-bundler');
  
  module.exports = {
    plugins: [
       new RawBundlerPlugin({
             excludedFilenames: [/angulartics/],
             readEncoding: "utf-8",
             includeFilePathComments: false,
			 allowDuplicatesInBundle: false,
             bundles: [ "vendor.js", "styles.css", "vendor.lib.js" ],
             "vendor.js": [
				'js/*.js'
             ],
			 "vendor.lib.js": [{
                 path: "../build/*.js",
                 match: /vendor/
             }],
			 "styles.css": [
			 	'css/bootstrap.css',
				'css/edits.css'
			 ]
       })
    ]

```
## Example Output

This generates two files with merged js and css content.

``` html
  <script src="./vendor.js"></script>
  <link rel="stylesheet" href="./styles.css">
```

# Options

## bundles 
(Required)

The output files to be made. These will be outputted to the same directory as set in `module.exports.output.path` of your webpack.config

## [Files] 
(Required)
 
Must have the same name as a member of bundles. The sub-array are the files to are be included in the bundle. The sub-array has the following format options:
``` javascript
"bundle_name.extension" : [
	"pathtype1.ext",
	"*.ext",
	{
		path: "pathtype2.ext"
		match: /good_file_standard/ /* Overpowerd by the exlusion variable */
	},
	{ path: "*.ext2" }
]
```
If a path is given a wildcard, it will add all files, including all subdirectories, with the appropiate file extension from that folder on.

## excludedFilenames
default: []

An array of regexp expressions that if a dynamically found filename is matched, will not be included.

## readEncoding
default: utf-8

The encoding nodejs reads in. Look up the documentation for more information.

## includeFilePathComments
(Boolean) - default: false 

Puts the file path of the added file in a `/**/` comment style before the bundled file contents.
Note: If reading from an invlaid encoding type, ie: settings are utf-8 but file is encoded in utf-8-BOM, then you may seem some non-rendering character generated after this.

## allowDuplicatesInBundle
(Boolean) - default: false

Per individual file bundle, are identical file names allowed to be parsed?

### TODO
```
	Add support for `useAsLoader`
	Add per bundle support for exlcusions
	Add custom comment styles 
```