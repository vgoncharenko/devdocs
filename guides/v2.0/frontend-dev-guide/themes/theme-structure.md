---
layout: default
group: fedg
subgroup: A_Themes
title: Magento theme structure
menu_title: Magento theme structure
menu_order: 3
version: 2.0
github_link: frontend-dev-guide/themes/theme-structure.md
redirect_from: /guides/v1.0/frontend-dev-guide/themes/theme-structure.html
---

<h2 id="theme-structure-intro">What's in this topic</h2>
A <a href="{{page.baseurl}}frontend-dev-guide/themes/theme-general.html#theme-gen-overview" target="_blank">design theme</a> is an important part of the Magento application. This topic describes the file structure of a Magento {% glossarytooltip d2093e4a-2b71-48a3-99b7-b32af7158019 %}theme{% endglossarytooltip %}.

<h2 id="theme-structure-loc">Magento theme location</h2>
{% glossarytooltip 1a70d3ac-6bd9-475a-8937-5f80ca785c14 %}Storefront{% endglossarytooltip %} themes are conventionally located under `app/design/frontend/<Vendor>/`. Though technically they can reside in other directories. For example Magento built-in themes can be located under `vendor/magento/theme-frontend-<theme_code>` when a Magento instance is deployed from the {% glossarytooltip d85e2d0a-221f-4d03-aa43-0cda9f50809e %}Composer{% endglossarytooltip %} repository.

Each theme must be stored in a separate directory:
<pre>
app/design/frontend/&lt;Vendor&gt;/
├──&nbsp;&lt;theme1&gt;
├──&nbsp;&lt;theme2&gt;/
├──&nbsp;&lt;theme3&gt;
├--...
</pre>

<h2 id="theme-structure-comp">Theme components</h2>
The structure of a Magento theme directory typically would be like following:
<pre>
&lt;theme_dir&gt;/
├──&nbsp;&lt;Vendor&gt;_&lt;Module&gt;/&nbsp;
│	├──&nbsp;web/
│	│	├──&nbsp;css/
│	│	│	├──&nbsp;source/
│	├──&nbsp;layout/
│	│	├──&nbsp;override/
│	├──&nbsp;templates/
├──&nbsp;etc/
├──&nbsp;i18n/&nbsp;
├──&nbsp;media/
├──&nbsp;web/
│	├──&nbsp;css/
│	│	├──&nbsp;source/&nbsp;
│	├──&nbsp;fonts/
│	├──&nbsp;images/
│	├──&nbsp;js/
├──&nbsp;composer.json&nbsp;
├──&nbsp;registration.php&nbsp;
├──&nbsp;theme.xml&nbsp;
</pre>
Let's have a closer look at each particular sub-directory.

<div class="bs-callout bs-callout-info" id="info">
  <p>The directories and files structure described below is the most extended one. It may not coincide with the structure of your store.</p>
</div>
<table>
  <tbody>
    <tr>
      <th>Directory</th>
      <th colspan="1">Required</th>
      <th>Description</th>
    </tr>
    <tr>
      <td colspan="1">
        <code>
          /&lt;Vendor&gt;_&lt;Module&gt;
        </code>
      </td>
      <td colspan="1">
        optional
      </td>
      <td colspan="1">
          Module-specific styles, layouts, and templates.
      </td>
    </tr>
    <tr>
      <td colspan="1">
        <code>/&lt;Vendor&gt;_&lt;Module&gt;/web/css/source</code>
      </td>
      <td colspan="1">
        optional
      </td>
      <td colspan="1">
          Module-specific styles (<code>.css</code> and/or <code>.less</code> files). General styles for the module are in the <code>_module.less</code> file, and styles for widgets are in <code>_widgets.less</code>.
      </td>
    </tr>
    <tr>
      <td colspan="1">
        <code>/&lt;Vendor&gt;_&lt;Module&gt;/layout</code>
      </td>
      <td colspan="1">
        optional
      </td>
      <td colspan="1">
        Layout files which extend the default module or parent theme layouts. <!--ADDLINK-->
      </td>
    </tr>
    <tr>
      <td colspan="1">
        <code>/&lt;Vendor&gt;_&lt;Module&gt;/layout/override/base</code>
      </td>
      <td colspan="1">
        optional
      </td>
      <td colspan="1">
        Layouts that override the default module layouts.
      </td>
    </tr>
    <tr>
      <td colspan="1">
        <code>/&lt;Vendor&gt;_&lt;Module&gt;/layout/override/&lt;parent_theme&gt;</code>
      </td>
      <td colspan="1">optional</td>
      <td colspan="1">
        Layouts that override the parent theme layouts for the module.
      </td>
    </tr>
    <tr>
      <td colspan="1">
        <code>/&lt;Vendor&gt;_&lt;Module&gt;/templates</code>
      </td>
      <td colspan="1">
        optional
      </td>
      <td colspan="1">
        This directory contains theme templates which override the default module templates or parent theme templates for this module. Custom templates are also stored in this directory.
      </td>
    </tr>
    <tr>
      <td colspan="1">
        <code>
          /etc/view.xml
        </code>
      </td>
      <td colspan="1">required for a theme, but optional if exists in the parent theme</td>
      <td colspan="1">
        This file contains images configuration for all storefront product images and thumbnails.
      </td>
    </tr>
    <tr>
      <td colspan="1">
        <code>/i18n</code>
      </td>
      <td colspan="1">optional</td>
      <td colspan="1">.csv files with translations.</td>
    </tr>
    <tr>
      <td colspan="1">
        <code>/media</code>
      </td>
      <td colspan="1">required</td>
      <td colspan="1">
        This directory contains a theme preview (a screenshot of your theme).
      </td>
    </tr>
    <tr>
      <td colspan="1">
        <code>/web</code>
      </td>
      <td colspan="1">
        optional
      </td>
      <td colspan="1">Static files that can be loaded directly from the frontend.</td>
    </tr>
    <tr>
      <td colspan="1">
        <code>/web/css/source</code>
      </td>
      <td colspan="1">
        optional
      </td>
      <td colspan="1">This directory contains theme
        <code>less</code>
         configuration files that invoke mixins for global elements from the Magento UI library, and
        <code>theme.less</code>
         file which overrides the default variables values. <!--ADDLINK-->
      </td>
    </tr>
    <tr>
      <td colspan="1">
        <code>/web/css/source/lib</code>
      </td>
      <td colspan="1">
        optional
      </td>
      <td colspan="1">
        View files that override the UI library files stored in <code>lib/web/css/source/lib</code>
      </td>
    </tr>
    <tr>
      <td colspan="1">
        <code>/web/fonts</code>
      </td>
      <td colspan="1">
        optional
      </td>
      <td colspan="1">
        Theme fonts.
      </td>
    </tr>
    <tr>
      <td colspan="1">
        <code>/web/images</code>
      </td>
      <td colspan="1">
        optional
      </td>
      <td colspan="1">
        Images that are used in this theme.
      </td>
    </tr>
    <tr>
      <td colspan="1">
        <code>/web/js</code>
      </td>
      <td colspan="1">
        optional
      </td>
      <td colspan="1">
        Theme JavaScript files.
      </td>
    </tr>
    <tr>
      <td colspan="1">
        <code>
          /composer.json
        </code>
      </td>
      <td colspan="1">optional</td>
      <td colspan="1">
        Describes the theme dependencies and some meta-information. Will be here if your theme is a Composer package.
      </td>
    </tr>
    <tr>
      <td colspan="1">
        <code>/registration.php</code>
      </td>
      <td colspan="1">required</td>
      <td colspan="1">
        Required to register your theme in the system.
      </td>
    </tr>
    <tr>
      <td colspan="1">
        <code>/theme.xml</code>
      </td>
      <td colspan="1">required</td>
      <td colspan="1">
        The file is mandatory as it declares a theme as a system component. It contains the basic meta-information, like the theme name and the parent theme name, if the theme is inherited from an existing theme. The file is used by the Magento system to recognize the theme.
      </td>
    </tr>
  </tbody>
</table>

<h2 id="theme-structure-files">Theme files</h2>

Apart from the configuration file and theme {% glossarytooltip 3f0f2ef1-ad38-41c6-bd1e-390daaa71d76 %}metadata{% endglossarytooltip %} file, all theme files fall into the following two categories:

* Static view files
* Dynamic view files

<h3 id="theme-structure-pub">Static view files</h3>
A set of theme files that are returned by the server to a browser as is, without any processing, are called the *static files* of a theme.

{% glossarytooltip 363662cb-73f1-4347-a15e-2d2adabeb0c2 %}Static files{% endglossarytooltip %} can be located in a theme directory as follows:
<pre>
&lt;theme_dir&gt;/
├──&nbsp;media/
├──&nbsp;web
│	├──&nbsp;css/&nbsp;(except&nbsp;the&nbsp;&quot;source&quot;&nbsp;sub-directory)
│	├──&nbsp;fonts/
│	├──&nbsp;images/
│	├──&nbsp;js/
</pre>
The key difference between static files and other theme files is that static files appear on a web page as references to the files, while other theme files take part in the page generation, but are not explicitly referenced on a web page as files.

Static view files that can be accessed by a direct link from the store front, are distinguished as public theme files.

<div class="bs-callout bs-callout-info" id="info">
  <p>To be actually accessible for browsers public static files are <a href="{{page.baseurl}}architecture/view/static-process.html#publish-static-view-files" target="_blank">published</a> to the <code>/pub/static/frontend/&lt;Vendor&gt;/&lt;theme&gt;/&lt;language&gt;/css/</code> directory.</p>
</div>

<h3>Dynamic view files</h3>
View files that are processed or executed by the server in order to provide result to the client. These are: `.less` files, templates, and layouts.

Dynamic view files are located in a theme directory as follows:
<pre>
&lt;theme_dir&gt;/
├──&nbsp;Magento_&lt;module&gt;/&nbsp;
│	├──&nbsp;web/
│	│	├──&nbsp;css/
│	│	│	├──&nbsp;source/
│	├──&nbsp;layout/
│	│	├──&nbsp;override/
│	├──&nbsp;templates/
├──&nbsp;web/
│	├──&nbsp;css/
│	│	├──&nbsp;source/

</pre>
