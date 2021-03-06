
django-shrink
=============
A js compiler & css minifier with sass compatibility


Requirements
------------
* Django 1.3+
* 2.6 <= Python < 3
* Java for compiling and compressing
* ``django.contrib.staticfiles`` in ``INSTALLED_APPS``


Installation
------------
::

    pip install django-shrink


Configuration
-------------
::

    INSTALLED_APPS = (
        ...
        'django.contrib.staticfiles',
        ...
        'shrink',
        ...
    )

Optionally if you want to use `Sass`_ you need to add the compilation view to
your urls::

    # urls.py
    urlpatterns = patterns('',
        ...
        (r'', include('shrink.urls')),
        ...
    )

.. note::
    The compilation view is only available when ``DEBUG = True``


Usage
-----
Define your javascripts and css files in your template as in this example::

    {% load shrink %}
    {% styles css/myproject-min.css %}
        css/reset.css
        css/forms.css
        css/myproject.scss
    {% endstyles %}
    {% scripts js/myproject-min.js %}
        js/jquery.js
        js/plugin.js
        js/myproject.js
    {% endscripts %}

When ``DEBUG = True`` this will end up as::

    <link rel="stylesheet" href="{{ STATIC_URL }}css/reset.css">
    <link rel="stylesheet" href="{{ STATIC_URL }}css/forms.css">
    <link rel="stylesheet" href="{{ STATIC_URL }}css/myproject.scss">
    <script src="{{ STATIC_URL }}js/jquery.js"></script>
    <script src="{{ STATIC_URL }}js/plugin.js"></script>
    <script src="{{ STATIC_URL }}js/myproject.js"></script>

When ``DEBUG = False`` this will end up as::

    <link rel="stylesheet" href="{{ STATIC_URL }}css/myproject-min.css?timestamp">
    <script src="{{ STATIC_URL }}js/myproject-min.js?timestamp"></script>

When deploying you want to compile your javascripts, compile your scss (`Sass`_)
and compress the css files. django-shrink overrides the ``collectstatic``
management command and after collecting the static files it does the compiling
and compressing. Thus you need to execute the management command
``collectstatic`` in your deployment environment.

.. note:: As of yet you need all the source css files and the destination
   minified css file to be in the same path if they ahev relative references to
   media.

Settings
--------

SHRINK_TIMESTAMP
^^^^^^^^^^^^^^^^
Controls if you want to timestamp the compressed/compiled assets.

* Default: ``True``

SHRINK_STORAGE
^^^^^^^^^^^^^^
Storage for the compressed/compiled assets.

* Default: ``settings.STATICFILES_STORAGE``

SHRINK_CLOSURE_COMPILER
^^^^^^^^^^^^^^^^^^^^^^^
Path to Google Closure Compiler jar.

* Default: Google Closure Compiler jar provided.

SHRINK_CLOSURE_COMPILER_COMPILATION_LEVEL
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Google Closure Compiler optimization level.

* Default: ``'SIMPLE_OPTIMIZATIONS'``

SHRINK_YUI_COMPRESSOR
^^^^^^^^^^^^^^^^^^^^^
Path to YUI Compressor

* Default: YUI compressor jar provided.


.. _Sass: http://sass-lang.com/

