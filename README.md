# PostHTML Minifier
[![Build Status](https://travis-ci.org/maltsev/posthtml-minifier.svg?branch=master)](https://travis-ci.org/maltsev/posthtml-minifier)

Modular HTML minifier.


## Usage
Just add `posthtml-minifier` as the last plugin:
```js
var posthtml = require('posthtml');

posthtml([
    /* other PostHTML plugins */

    require('posthtml-minifier')({
        removeComments: false // Disable the module "removeComments"
    })
]).process(html).then(function (result) {
    // result.html is minified
});
```


## Modules
By default all modules are enabled. You can disable some of them by passing `false`
in the plugin's arguments (like in the usage example above).

### removeComments
Removes HTML comments.
The module does not remove [`<!--noindex--><!--/noindex-->`](https://yandex.com/support/webmaster/controlling-robot/html.xml) comments,
since they can be used to prohibit indexing of some text fragments.

Source:
```html
<div><!-- test --></div>
```

Minified:
```html
<div></div>
```


### removeEmptyAttributes
Removes empty [safe-to-remove](https://github.com/maltsev/posthtml-minifier/blob/master/lib/modules/removeEmptyAttributes.es6) attributes.

Source:
```html
<img src="foo.jpg" alt="" style="">
```

Minified:
```html
<img src="foo.jpg" alt="">
```


### custom
It's possible to pass custom modules in the minifier.

As a function:
```js
require('posthtml-minifier')({
    custom: function (tree, options) {
        // Some minification
        return tree;
    }
})
```

As a list of functions:
```js
require('posthtml-minifier')({
    custom: [
        function (tree, options) {
            // Some minification
            return tree;
        },

        function (tree, options) {
            // Some other minification
            return tree;
        }
    ]
})
```

`options` is an object with all options that were passed to the plugin.




## Contribute

Since the minifier is modular, it's very easy to add new modules:

1. Create a file inside `lib/minifiers/` with a function that does some minification.
2. Create a file inside `test/minifiers/` with some unit-tests.
3. Describe your module in the section "[Modules](https://github.com/maltsev/posthtml-minifier/blob/master/README.md#modules)".
4. Send me a pull request.