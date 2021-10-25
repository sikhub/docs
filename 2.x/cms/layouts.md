# Layouts

- [Introduction](#introduction)
- [Placeholders](#placeholders)
- [Dynamic Layouts](#dynamic-layouts)
    - [Layout Execution Life Cycle](#layout-life-cycle)

<a name="introduction"></a>
## Introduction

Layouts define a common scaffold for your pages, which usually means everything that repeats on every page, such as a header and a footer. They will almost always contain HTML code such as HEAD, TITLE and BODY tags.

Layout template files reside in the **layouts** directory in your theme. Layout template files should have the **htm** extension. Inside the layout file you should use the `{% page %}` tag to output the page content. Next is an example of the simplest possible layout.

    <html>
        <body>
            {% page %}
        </body>
    </html>

To use a layout for a [page](pages) the page should refer the layout file name (without extension) in the [Configuration](themes#configuration-section) section. Remember that if you refer a layout from a [subdirectory](themes#subdirectories) you should specify the subdirectory name. Example page template using the default.htm layout:

    url = "/"
    layout = "default"
    ==
    <p>Hello, world!</p>

When this page is requested its content is merged with the layout, or more precisely - the layout's `{% page %}` tag is replaced with the page content. The previous examples would generate the following markup:

    <html>
        <body>
            <p>Hello, world!</p>
        </body>
    </html>

Note that you can render [partials](partials) in layouts. This lets you to share the common markup elements between different layouts. For example, you can have a partial that outputs the website CSS and JavaScript links. This approach simplifies the resource management - if you want to add a JavaScript reference you should modify a single partial instead of editing all the layouts.

The [Configuration](themes#configuration-section) section is optional for layouts. The supported configuration parameters are **name** and **description**. The parameters are optional and used in the back-end user interface. Example layout template with a description:

    description = "Basic layout example"
    ==
    <html>
        <body>
            {% page %}
        </body>
    </html>

<a name="placeholders"></a>
## Placeholders

Placeholders allow pages to inject content to the layout. Placeholders are defined in the layout templates with the `{% placeholder %}` tag. The next example shows a layout template with a placeholder **head** defined in the HTML HEAD section.

    <html>
        <head>
            {% placeholder head %}
        </head>
        ...

Pages can inject content to placeholders with the `{% put %}` and `{% endput %}` tags. The following example demonstrates a simple page template which injects a CSS link to the placeholder **head** defined in the previous example.

    url = "/my-page"
    layout = "default"
    ==
    {% put head %}
        <link href="/themes/demo/assets/css/page.css" rel="stylesheet">
    {% endput %}

    <p>The page content goes here.</p>

More information on placeholders can be found [in the Markup guide](../markup/tag-placeholder).

<a name="dynamic-layouts"></a>
## Dynamic Layouts

Layouts, like pages, can use any Twig features. Please refer to the [Dynamic pages](pages#dynamic-pages) documentation for details.

<a name="layout-life-cycle"></a>
### Layout Execution Life Cycle

Inside the layout's [PHP section](themes#php-section) you can define the following functions for handling the page execution life cycle: `onInit`, `onStart`, `onBeforePageStart` and `onEnd`.

The `onInit` function is executed when all components are initialized and before AJAX requests are handled. The `onStart` function is executed in the beginning of the page processing. The `onBeforePageStart` function is executed after the layout [components](components) ran, but before the page's `onStart` function is executed. The `onEnd` function is executed after the page is rendered. The sequence the handlers are executed is following:

1. Layout `onInit()` function.
1. Page `onInit()` function.
1. Layout `onStart()` function.
1. Layout components `onRun()` method.
1. Layout `onBeforePageStart()` function.
1. Page `onStart()` function.
1. Page components `onRun()` method.
1. Page `onEnd()` function.
1. Layout `onEnd()` function.