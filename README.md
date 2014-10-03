jquery-xblock
=============

jQuery plugin to load XBlocks remotely

### Setup

This plugin will allow you to load an XBlock remotely, from another website, using jQuery.

You will need to:

* Host both the application using this plugin, and the OpenEdX LMS on a common base URL. For example, you could have your website on http://example.com and the LMS on http://lms.example.com. This is needed to allow to share the authentication cookies
* Configure the LMS to allow CORS requests from the other domain

#### Configuring CORS on the LMS

Assuming the website on which you will use the current plugin is a http://example.com, you will need to set the following in the LMS configuration:

```python
FEATURES['ENABLE_CORS_HEADERS'] = True
CORS_ORIGIN_WHITELIST = ('example.com',)
```

### Instantiating the XBlock

On your website, load the plugin and its dependencies (jQuery & jQuery cookie) in the header:

```html
<html>
  <head>
    <script src="//code.jquery.com/jquery-1.11.0.min.js"></script>
    <script src="//cdnjs.cloudflare.com/ajax/libs/jquery-cookie/1.4.0/jquery.cookie.min.js"></script>
    <script src="/static/js/vendor/jquery.xblock.js"></script>
  </head>
  <body>
    <div>
      <h1>My amazing XBlock</h1>
      <div class="courseware-content"></div>
    </div>
  </body>
</html>
```

Then, to load an XBlock in the `div.content` element for example:

```js
$('.courseware-content').xblock({
    courseId: 'TestX/TST-BRGT/2015-12',
    usageId: 'i4x:;_;_TestX;_TST-BRGT;_vertical;_0c4f0ca3c3f54a1b8ad5d9830c1d16b0',
    sessionId: '89e5dd96180debc33b582969b88ec9ce',
    baseDomain: 'example.com',
    lmsSubDomain: 'lms',
});
```

Note that you need to provide the usage id of the XBlock you want to load, and a valid user session id from the LMS.

If you are using jquery.xblock in the same environment/host than the LMS, you can use the useCurrentHost
option, which simplify the usage. Example:

```js
$('.courseware-content').xblock({
    courseId: 'TestX/TST-BRGT/2015-12',
    usageId: 'i4x:;_;_TestX;_TST-BRGT;_vertical;_0c4f0ca3c3f54a1b8ad5d9830c1d16b0',
    useCurrentHost: true,
});
```

### Global Options

The global options is used to prioritize options with multiple xblock loading. A XBlock itself can
use jquery.xblock to load various componments, but they are not aware of the main configuration and
simply use the useCurrentHost option. In an external application, if we load a xblock with jquery
xblock with a configuration X, then it's children should use that config too. The above behavior
will override the following jquery.xblock options:

- sessionId
- baseDomain
- lmsSubDomain
- useCurrentHost

Note that you can also disable that behavior by using the **disableGlobalOptions** setting.

### Handling events

When a user clicks on a `/jump_to_id` URL, used in courseware content to create links between courseware sections,
jquery-xblock catches the click and emits an `xblock_jump` event, which you can monitor:

```js
$('.courseware-content').on('xblock_jump', function(event, course_id, block_type, block_id) {
  // Here, load a new XBlock
  // Likely the vertical containing the block referenced in the jump_to_id
});
```

### Tests

#### Setup

The test environment uses Karma + Chrome.

1. Make sure you have nodeJS and npm installed on your system.
2. Install tests dependencies.

   ```sh
   npm install
   ```

#### Run

1. Run tests

   ```sh
   make test
   ```

2. The coverage report is in the *coverage* folder. ie: coverage/Chrome\ 33.0.1750\ \(Linux\)/index.html

#### TODO

...