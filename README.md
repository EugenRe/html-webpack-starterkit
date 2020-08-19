## Features

* Separated development and production webpack settings
* Sass
* ES6
* Asset loading
* CSS Vendor prefixing
* Development server
* Sourcemaps
* Favicons generation
* Production optimizations
* Mobile browser header color


## Requirements

* [Node](https://nodejs.org) > 7.6


## Usage

To start the development server

```sh
npm start
```

To build for production

```sh
npm run build
```


### How to load fonts

If you don't support Opera Mini, browsers support the .woff format. Its newer version .woff2, is widely supported by modern browsers and can be a good alternative.

If you decide to use only this format you can load the fonts in a similar manner to images.

In your `webpack.dev.js` and `webpack.prod.js` add the following

```js
module.exports = {
    // ..
    module: {
        rules: [
            // ..
            {
                test: /\.woff$/,
                loader: 'url-loader',
                options: {
                    // Limit at 50k. Above that it emits separate files
                    limit: 50000,
                    // url-loader sets mimetype if it's passed.
                    // Without this it derives it from the file extension
                    mimetype: 'application/font-woff',
                    // Output below fonts directory
                    name: './fonts/[name].[ext]',
                },
            }
            // ..
        ]
    }
    // ..
};
```

And let's say your font is in the folder `assets` with the name `pixel.woff`

You can add it and use it in `index.scss` as
```scss
@font-face {
    font-family: "Pixel";
    src: url('./../assets/pixel.woff') format('woff');
}

.body{
    font-family: 'Pixel', sans-serif;
}
```

If you would like to support all kinds of font types, remove the woff rule we previously added to `webpack.dev.js` and `webpack.prod.js` and add the following

```js
module.exports = {
    // ..
    module: {
        rules: [
            // ..
            {
                test: /\.(ttf|eot|woff|woff2)$/,
                loader: 'file-loader',
                options: {
                    name: 'fonts/[name].[ext]',
                },
            }
            // ..
        ]
    }
    // ..
};
```

And assuming you have your fonts in the directory `fonts` with names `pixel.woff`, `pixel.ttf`, `pixel.eot` , etc.

You can add it and use it in `index.scss` as
```scss
@font-face {
    font-family: 'Pixel';
    src: url('./../fonts/pixel.woff2') format('woff2'),
    url('./../fonts/pixel.woff') format('woff'),
    url('./../fonts/pixel.eot') format('embedded-opentype'),
    url('./../fonts/pixel.ttf') format('truetype');
    /* Add other formats as you see fit */
}
```

### How to load images

If you would like to include an image on your `index.html` file, place the path of the image in a webpack require statement`<%= require(imagePath) %>`.

```html
  <img class="splash-title__img"
                     src="<%= require('./src/assets/logo-on-dark-bg.png') %>"
                     alt="webpack logo"></a>
```
