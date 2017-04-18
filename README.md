# videojs-errors

[![Build Status](https://travis-ci.org/brightcove/videojs-errors.svg?branch=master)](https://travis-ci.org/brightcove/videojs-errors)

A plugin that displays user-friendly messages when Video.js encounters an error.

### Table of Contents

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->


- [Getting Started](#getting-started)
  - [Localization](#localization)
  - [Supported Errors](#supported-errors)
  - [Custom Errors](#custom-errors)
- [Known Issues](#known-issues)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Getting Started

The plugin automatically registers itself when you include videojs.errors.js in your page:

```html
<script src="videojs.errors.js"></script>
```

You probably want to include the default stylesheet, too. It displays error messages as a semi-transparent overlay on top of the video element itself. It's designed to match up fairly well with the default Video.js styles:

```html
<link href="videojs.errors.css" rel="stylesheet">
```

If you're not a fan of the default styling, you can drop in your own stylesheet. The only new element to worry about is `vjs-errors-dialog` which is the container for the error messages.

### Localization
The plugin supports multiple languages when using Video.JS v4.7.3 or greater. In order to add additional language support, add the language file after your plugin as follows:

```html
<script src="videojs.errors.js"></script>
<script src="lang/es.js"></script>
```

**Note:** A formatted example is available for Spanish under 'lang/es.js'.

### Supported Errors

Once you've initialized Video.js, you can activate the errors plugin. The plugin has a set of default error messages for the standard HTML5 video errors keyed off their runtime values:

- MEDIA_ERR_ABORTED (numeric value `1`)
- MEDIA_ERR_NETWORK (numeric value `2`)
- MEDIA_ERR_DECODE (numeric value `3`)
- MEDIA_ERR_SRC_NOT_SUPPORTED (numeric value `4`)
- MEDIA_ERR_ENCRYPTED (numeric value `5`)

### Custom Errors

Additionally, some custom errors have been added as reference for future extension.

- PLAYER_ERR_NO_SRC (numeric value `-1`)
- PLAYER_ERR_TIMEOUT (numeric value `-2`)
- PLAYER_ERR_DOMAIN_RESTRICTED (numeric value `-3`)
- PLAYER_ERR_IP_RESTRICTED (numeric value `-4`)
- PLAYER_ERR_GEO_RESTRICTED (numeric value `-5`)

**Note:**

- Custom errors should reference a code value of a negative integer.
- Custom errors should reference a type beginning with `PLAYER_ERR` versus the standardized `MEDIA_ERR` to avoid confusion.
- Custom errors can be chosen to be dismissible (boolean value `true`)

If the video element emits any of those errors, the corresponding error message will be displayed. You can override and add custom error codes by supplying options to the plugin:

```js
player.errors({
  errors: {
    3: {
      headline: 'This is an override for the generic MEDIA_ERR_DECODE',
      message: 'This is a custom error message'
    }
  }
});
```

Or by calling `player.errors.extend` _after_ initializing the plugin:

```js
player.errors();

player.errors.extend({
  3: {
    headline: 'This is an override for the generic MEDIA_ERR_DECODE',
    message: 'This is a custom error message'
  },
  foo: {
    headline: 'My custom "foo" error',
    message: 'A custom "foo" error message.'
  }
});
```

If you define custom error messages, you'll need to let Video.js know when to emit them yourself:

```js
player.error({code: 'custom', dismiss: true});
```

If an error is emitted that doesn't have an associated key, a generic, catch-all message is displayed. You can override that text by supplying a message for the key `unknown`.

## Known Issues

On iPhones, default errors are not dismissible. The video element intercepts all user interaction so error message dialogs miss the tap events. If your video is busted anyways, you may not be that upset about this.
