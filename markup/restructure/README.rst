Cloudinary
==========

Cloudinary is a cloud service that offers a solution to a web application's entire image management pipeline.

Easily upload images to the cloud. Automatically perform smart image resizing, cropping and conversion without installing any complex software. Integrate Facebook or Twitter profile image extraction in a snap, in any dimension and style to match your website’s graphics requirements. Images are seamlessly delivered through a fast CDN, and much much more.

Cloudinary offers comprehensive APIs and administration capabilities and is easy to integrate with any web application, existing or new.

Cloudinary provides URL and HTTP based APIs that can be easily integrated with any Web development framework.

For Javascript, Cloudinary provides a jQuery plugin for simplifying the integration even further.

The direct image upload feature of the plugin is based on https://github.com/blueimp/jQuery-File-Upload

Getting started guide
---------------------

|image0| **Take a look at our `Getting started guide for jQuery <http://cloudinary.com/documentation/jquery_integration#getting_started_guide>`_**.

:construction: New API!
-----------------------

The Javascript library has undergone an extensive redesign. Previously, the library was deeply coupled with jQuery and the Blueimp upload plugin. In order to cater for developer who do not wish to use jQuery, the library has been split into three libraries:

-  A core Javascript library that does not depend on jQuery or Blueimp,
-  A jQuery plugin library.
-  A library that includes both the jQuery and the Blueimp functionality (which is fully backward compatible with previous versions).

Installation
------------

bower
~~~~~

1. Install the files using ``bower install cloudinary_js#^1.1``. Use the optional ``--save`` parameter if you wish to save the dependency in your bower.json file.
2. Include the javascript file in your HTML. For Example:

   ``<script src="../bower_components/cloudinary_js/js/cloudinary.js"></script>`` ### NPM
3. Install the files using ``npm  install cloudinary_js@”^1.1”``.
4. Include the javascript file in your HTML. For Example:

   ::

       <script src="../node_modules/cloudinary_js/js/cloudinary.js"></script>

Setup
-----

In order to properly use this library you have to provide it with a few configuration parameters:

Required:

-  ``cloud_name`` - The cloudinary cloud name associated with your Cloudinary account.

Optional:

-  ``private_cdn``, ``secure_distribution``, ``cname``, ``cdn_subdomain`` - Please refer to `Cloudinary Documentation <http://cloudinary.com/documentation/rails_additional_topics#configuration_options>`_ for information on these parameters.

To set these configuration parameters use the ``Cloudinary::config`` function (see below).

Note:

When loading the jQuery Cloudinary library directly (using a ``script`` tag), the library automatically converts the relevant fileupload tags to utilize the upload functionality. If jquery.cloudinary is loaded as an `AMD <https://github.com/amdjs/amdjs-api/blob/master/AMD.md>`_ however, you need to initialize the Cloudinary fileupload fields e.g., by calling ``$("input.cloudinary-fileupload[type=file]").cloudinary_fileupload();``

Usage
-----

The following blog post details the process of setting up a jQuery based file upload. http://cloudinary.com/blog/direct\_image\_uploads\_from\_the\_browser\_to\_the\_cloud\_with\_jquery

The Cloudinary Documentation can be found at: http://cloudinary.com/documentation

Core JavaScript library
~~~~~~~~~~~~~~~~~~~~~~~

The Core Cloudinary JavaScript library provides several classes, defined under the "``cloudinary``" domain.

Configuration
^^^^^^^^^^^^^

Start by instantiating a new Cloudinary class:

Explicitly
''''''''''

::

    var cl = cloudinary.Cloudinary.new( { cloud_name: "demo"});

Using the config function
'''''''''''''''''''''''''

::


    // Using the config function
    var cl = cloudinary.Cloudinary.new();
    cl.config( "cloud_name", "demo");

From meta tags in the current HTML document
'''''''''''''''''''''''''''''''''''''''''''

When using the library in a browser environment, you can use meta tags to define the configuration options.

:information\_source: The ``init()`` function is a convenience function that invokes both ``fromDocument()`` and ``fromEnvironment()``.

::

    // Add the following to the header tag:
    // <meta name="cloudinary_cloud_name" content="demo">
    var cl = cloudinary.Cloudinary.new();
    cl.fromDocument();
    // or
    cl.init();

From environment variables
''''''''''''''''''''''''''

When using the library in a backend environment, such as NodeJS, you can use an environment variable to define the configuration options.

::


    // CLOUDINARY_URL=cloudinary://demo
    var cl = cloudinary.Cloudinary.new();
    cl.fromEnvironment();
    // or
    cl.init();

URL generation
^^^^^^^^^^^^^^

::

    cl.url("sample")
    // "http://res.cloudinary.com/demo/image/upload/sample"
    cl.url( "sample", { width: 100, crop: "fit"})
    // "http://res.cloudinary.com/demo/image/upload/c_fit,w_100/sample"

HTML tag generation
^^^^^^^^^^^^^^^^^^^

You can generate HTML tags in several ways:

Cloudinary::image() generates a DOM tag, and prepares it for responsive functionality. This is the same functionality as ``$.cloudinary.image()``. (When using the jQuery plugin, the ``src-cache`` data attribute is stored using jQuery's ``data()`` method and so is not visible.)

::

    cl.image("sample")
    //<img src=​"http:​/​/​res.cloudinary.com/​demo/​image/​upload/​sample" data-src-cache=​"http:​/​/​res.cloudinary.com/​demo/​image/​upload/​sample">​

You can generate an image Tag using the ``imageTag`` function:

::

    var tag = cl.imageTag("sample");
    tag.toHtml();
    // <img src="http://res.cloudinary.com/demo/image/upload/sample">
    tag.transformation().crop("fit").width(100).toHtml();
    // <img src="http://res.cloudinary.com/demo/image/upload/c_fit,w_100/sample">

You can also use ``ImageTag`` independently:

::

    var tag = cloudinary.ImageTag.new( "sample", { cloud_name: "some_other_cloud" });
    tag.toHtml();
    // <img src="http://res.cloudinary.com/some_other_cloud/image/upload/sample">

Transformation
^^^^^^^^^^^^^^

In addition to using a plain object to define transformations or using the builder methods (both described above), you can define transformations by using the Transformation class:

::

    var tr = cloudinary.Transformation.new();
    tr.crop("fit").width(100);
    tr.serialize()
    // "c_fit,w_100"

You can also chain transformations together:

::

    var tr = cloudinary.Transformation.new();
    tr.width(10).crop('fit').chain().angle(15).serialize()
    // "c_fit,w_10/a_15"

jQuery plugin
~~~~~~~~~~~~~

The Cloudinary jQuery plugin is fully backward compatible with the previous cloudinary\_js version. Under the hood, the new JavaScript library instantiates a CloudinaryJQuery object and attaches it to jQuery.

-  ``$.cloudinary.config(parameter_name, parameter_value)`` - Sets parameter\_name's value to parameter\_value.
-  ``$.cloudinary.url(public_id, options)`` - Returns a cloudinary URL based on your configuration and the given options.
-  ``$.cloudinary.image(public_id, options)`` - Returns an HTML image tag for the photo specified by public\_id
-  ``$.cloudinary.facebook_profile_image``, ``$.cloudinary.twitter_profile_image``, ``$.cloudinary.twitter_name_profile_image``, ``$.cloudinary.gravatar_image`` , ``$.cloudinary.fetch_image`` - Same as ``image`` but returns a specific type of image.

-  ``$(jquery_selector).cloudinary(options)`` - Goes over the elements specified by the jQuery selector and changes all the images to be fetched using Cloudinary's CDN. Using options, you can also specify transformations to the images. The ``options`` parameters are similar across all cloudinary frameworks. Please refer to `jQuery image manipulation <http://cloudinary.com/documentation/jquery_image_manipulation>`_ for a more complete reference regarding the options.

|image1| **See `our documentation <http://cloudinary.com/documentation/jquery_image_manipulation>`_ for more information about displaying and transforming images using jQuery**.

Direct file upload with backend support
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The javascript library implements helpers to be used in conjunction with the backend cloudinary frameworks (Rails, PHP, django). These frameworks can be used to embed a file upload field in the HTML (``cl_image_upload_tag``). When used, the script finds these fields and extends them as follows:

Upon a successful image upload, the script will trigger a jQuery event (``cloudinarydone``) on the input field and provide a fileupload data object (along with the ``result`` key containing received data from the cloudinary upload API) as the only argument.

If a ``cloudinary-field-name`` has been provided with the upload field, the script will find an input field in the form with the given name and update it post-upload with the image metadata: ``<image-path>#<public-id>``. If no such field exists a new hidden field will be created.

|image2| **See `our documentation <http://cloudinary.com/documentation/jquery_image_upload>`_ with plenty more options for uploading to the cloud directly from the browser**.

Client side image resizing before upload
----------------------------------------

See the File Processing Options in https://github.com/blueimp/jQuery-File-Upload/wiki/Options. Add the following javascript includes after the standard fileupload includes:

::

    js/load-image.min.js
    js/canvas-to-blob.min.js
    js/jquery.fileupload-process.js
    js/jquery.fileupload-image.js
    js/jquery.fileupload-validate.js

Also, add the following javascript:

::

    $(document).ready(function() {
      $('.cloudinary-fileupload').fileupload({
        disableImageResize: false,
        imageMaxWidth: 800,
        imageMaxHeight: 600,
        acceptFileTypes: /(\.|\/)(gif|jpe?g|png|bmp|ico)$/i,
        maxFileSize: 20000000, // 20MB
        loadImageMaxFileSize: 20000000 // default limit is 10MB
      });
    });

Angular Directives
------------------

Joshua Chaitin-Pollak contributed AngularJS directives for Cloudinary: Display and manipulate images and perform uploads from your Angular application.

The Angular module is currently maintained as a separate GitHub project:

https://github.com/cloudinary/cloudinary\_angular

Additional resources
--------------------

Additional resources are available at:

-  `Website <http://cloudinary.com>`_
-  `Documentation <http://cloudinary.com/documentation>`_
-  `Knowledge Base <http://support.cloudinary.com/forums>`_
-  `Documentation for jQuery integration <http://cloudinary.com/documentation/jquery_integration>`_
-  `jQuery image upload documentation <http://cloudinary.com/documentation/jquery_image_upload>`_
-  `jQuery image manipulation documentation <http://cloudinary.com/documentation/jquery_image_manipulation>`_
-  `Image transformations documentation <http://cloudinary.com/documentation/image_transformations>`_

Support
-------

You can `open an issue through GitHub <https://github.com/cloudinary/cloudinary_js/issues>`_.

Contact us at `http://cloudinary.com/contact <http://cloudinary.com/contact>`_.

Stay tuned for updates, tips and tutorials: `Blog <http://cloudinary.com/blog>`_, `Twitter <https://twitter.com/cloudinary>`_, `Facebook <http://www.facebook.com/Cloudinary>`_.

License
-------

Released under the MIT license.

.. |image0| image:: http://res.cloudinary.com/cloudinary/image/upload/see_more_bullet.png
.. |image1| image:: http://res.cloudinary.com/cloudinary/image/upload/see_more_bullet.png
.. |image2| image:: http://res.cloudinary.com/cloudinary/image/upload/see_more_bullet.png