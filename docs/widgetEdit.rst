Customizing the Widget
**********************

* In the code customization mode, the user can edit the widget wrapper element (widget.html), the widget’s item templates (items.html), and their corresponding styles (widget.css)
* Some of the elements and attributes are mandatory and should not be changed, others are optional
* Data is injected to the html via the Mustache templating engine; for example, {{data.content.image}} will inject the image’s URL.

Editing the Widget Wrapper (widget.html)
========================================
In the widget.html file, you can edit the HTML base of the widget, which contains the frame and the basic HTML code.
The following is the structure of the widget wrapper.::

    <div class="my6_wrap" ><span class="my6_title">You may also like</span>
        <div class="my6_list" data-noscroll="true"></div>
        <img class="my6_logo_img" src="//d2ymkpxi1rgldj.cloudfront.net/web-clients/images/empty_logo.png"/>
    </div>

API
----

Mandatory
++++++++++

#. ``class = my6_list``, allows the widget to identify this element as a **list** (in other words, the element that contains the widget’s items). It is a container for the injected items.
#. ``data-noscroll = true``

Optional
++++++++++

#. ``<img  class="my6_logo_img" src=”default image that will be replaced”>``- this is an element with a network logo; src will be replaced during runtime.
#. Title element.

Editing Item Templates (items.html)
====================================

The UI templates in items.html are used to display each single content recommendation received from the server.
Each item is a repeatable template; each widget must have two different item templates defined, as follows:

#. **“default”** (data-template=”default”), which will be used to display “organic” items and
#. **“external”** (data-template=”external”) for paid items

.. image:: ../assets/itemTemplates.png

The following is the structure of the item template::

    <a class="my6_item my6_item_default"
    href="{{data.action.originalUrl}}"
    onmousedown="href='javascript:void(0);'"
    onmouseover="href='{{data.action.originalUrl}}'"
    data-template="default"
    data-href="{{data.action.url}}"
    data-item_id="{{id}}"
    data-method="{{#data.action}}{{#open_externally}}external{{/open_externally}}{{^open_externally}}default{{/open_externally}}{{/data.action}}">
    <div class="my6_item_image_wrap">
       <div class="my6_item_img_bg"></div>
       <div class="my6_placeholder my6_scale></div>
        <div class="my6_item_image my6_scale" style="background-image: url('{{data.content.image}}');"></div>
    </div>
    <div class="my6_item_text">
       <div  class="my6_item_title">{{data.content.title}}></div>
       {{#data.content.alias}}
      <span class="my6_alias"
           {{data.content.alias}}
       </span>
       {{/data.content.alias}}
    </div>
    </a>

Mandatory Constraints
++++++++++++++++++++++
Do **NOT** set an ID for Item elements using the HTML “id” attribute.

API
----

Mandatory
++++++++++
#. ``class := my6_item``, allows the widget to identify this element as an Item.
#. ``data-template``- one or more names for this item template; the name must be unique relative to other templates. Multiple names can be set using a comma separated string. items.html must contain an element with “external” and “default” data templates (the same element can be used for both templates).
#. ``data-method``– allows the core to identify the navigation method on item click. **Do not change its value.**
#. ``class = my6_placeholder``, allows the widget to identify this element for item fallback image (in case the item image is not valid). The background image will be replaced in the widget’s flow.
#. Mustache meta-data strings boilerplate - see the following table

Mandatory Mustache Meta-Data Attributes
+++++++++++++++++++++++++++++++++++++++

+-------------------------------------------------------+------------------------+
| Attribute Name                                        | Description            |
+=======================================================+========================+
| | data-href=”{{data.action.url}}”                     | Item URL               |
+-------------------------------------------------------+------------------------+
| | data-item_id=”{{id}}"                               | Item Internal ID       |
+-------------------------------------------------------+------------------------+
| | data-method=”{{#data.action}}{{#open_externally}}   |   Redirect method      |
| | external{{/open_externally}}{{^open_externally}}    |   specification        |
| | default{{/open_externally}}{{/data.action}}”        |                        |
+-------------------------------------------------------+------------------------+


Optional
++++++++

#. To show the item’s URL in the status bar when hovering over the item, use the item element as an <a> element, and also add the following attributes:

- href="{{data.action.originalUrl}}"
- onmousedown="href='javascript:void(0);'"
- onmouseover="href='{{data.action.originalUrl}}'"

#. The following are the Mustache data strings (if any data should be rendered).

+-------------------------------------------+------------------------------------------------------+
| Attribute Name                            | Description                                          |
+===========================================+======================================================+
| {{data.content.title}}                    | The Item’s Title                                     |
+-------------------------------------------+------------------------------------------------------+
| {{data.content.image}}                    | Item Internal ID                                     |
+-------------------------------------------+------------------------------------------------------+
| {{data.content.data.content.alias}}       | The shortened site name to display for the item      |
+-------------------------------------------+------------------------------------------------------+

Editing the Widget’s Style (widget.css)
=======================================
The widget’s style (both items.html and widget.html) can be customized in the widget.css.

Mandatory Constraints
----------------------
Placeholder images should be placed behind the real images (using the z-index property).

Reserved HTML Class Names
=========================

Some UIs can be configured via the management application.
This configuration capability expects the usage of certain class names for certain elements,
and might override developer set properties and values, as defined in the following table.

+-----------------+---------------------------------------------+------------+--------------+
| Class Name      | Element Description                         |  Mandatory | File Name    |
|                 |                                             |            |              |
+=================+=============================================+============+==============+
| my6_title       | | Entire widget title                       |  No        | widget.html  |
+-----------------+---------------------------------------------+------------+--------------+
| my6_list        | | List Element, Mandatory class for List    |  Yes       | widget.html  |
+-----------------+---------------------------------------------+------------+--------------+
| my6_logo_img    | | Network logo image. When applying the     |  No        | widget.html  |
|                 | | class to an <img> element, the widget     |            |              |
|                 | | will override the image src with the      |            |              |
|                 | | network logo.                             |            |              |
+-----------------+---------------------------------------------+------------+--------------+
| my6_item_title  | | Item Element                              |  No        | items.html   |
+-----------------+---------------------------------------------+------------+--------------+
| my6_placeholder | | Will render the backup image in this div, |  Yes       | items.html   |
|                 | | should be placed **behind** an Item’s     |            |              |
|                 | | 'real' image element.                     |            |              |
|                 | |                                           |            |              |
|                 | | Mandatory class for usage inside an       |            |              |
|                 | | Item                                      |            |              |
+-----------------+---------------------------------------------+------------+--------------+
| my6_item        | | Item Element. Mandatory class for Item    |  Yes       | items.html   |
+-----------------+---------------------------------------------+------------+--------------+