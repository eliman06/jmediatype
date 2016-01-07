# Overview #

jMediaType is a jQuery plugin that allows javascript to determine the current media type(s) in use by the user agent thus allowing you to change javascript behaviour to suit the needs of the media type(s).

Example scenario: You're viewing a normal web page and choose to go in to full screen mode after hooking up a projector. Using the "Fullerscreen" Firefox plugin the `projection` media type is enabled and having detected this you run some jQueries to convert the web page in to a slide show presentation.

# About #

I got the idea for this jQuery plugin from the following blog post:

http://www.ibloomstudios.com/articles/css_media_targeted_javascript/

I noticed that the method used had some issues, most notably the fact that it's possible for a browser to report more than one media type at the same time - we can't simply use `z-index` and a `switch` statement.

Currently this project is in alpha release state - I've got a rough draft of some working code, but it needs turning in to a proper plugin, events adding, etc. I'd also like to know if it's possible (from JS) to programatically change the media type. Any help greatly appreciated!

Google talk screen name: guy . fraser1 at gmail . com

# Code #

I've not got round to handing this in to SVN yet. In fact, I've not even turned it in to a proper plugin yet! But before I do something stupid and lose the code I'm just going to dump it in here first...

**CSS**

```
body div#atb-mediatype {
 border-width: 0;
 padding: 0;
 margin: 0;
}

@media print      {#atb-mediatype {margin-top:1px;}}
@media embossed   {#atb-mediatype {margin-right:1px;}}
@media braille    {#atb-mediatype {margin-bottom:1px;}}
@media aural      {#atb-mediatype {margin-left:1px;}}
@media tty        {#atb-mediatype {border-top:1px;}}
@media screen     {#atb-mediatype {padding-top:1px;}}
@media projection {#atb-mediatype {padding-right:1px;}}
@media handheld   {#atb-mediatype {padding-bottom:1px;}}
@media tv         {#atb-mediatype {padding-left:1px;}}
```

**HTML**

```
<div id="atb-mediatype">Detecting media type...</div>
```

**JavaScript**

```
jQuery(function() {

 var foo=function() {
  var test = jQuery("#atb-mediatype");
  var media = {};
  media.print      = test.css("border-width");
  media.embossed   = test.css("margin-top");
  media.braille    = test.css("margin-right");
  media.aural      = test.css("margin-bottom");
  media.tty        = test.css("border-top");
  media.screen     = test.css("padding-top");
  media.projection = test.css("padding-right");
  media.handheld   = test.css("padding-bottom");
  media.tv         = test.css("padding-left");
  test = "";
  var i;
  for (i in media) if (media[i]=(parseInt(media[i])==1)) test += i+",";
  jQuery("#atb-mediatype").text(test);
 }

 setTimeout(foo,2000);
});
```

As you can see, I'm using border, margin and padding properties as simple binary registers (each capable of holding 4 bits) with each media type using one of the bits. There are several unused bits which should give some room for future extension.

I'm currently trying to work out how to more elegantly detect changes in media type - eg. I might start in screen mode then move to projection mode, especially when using something like the "Fullerscreen" Firefox extension (which also results in both `screen` and `projection` media types being used at the same time). Ideally an event should also be triggered when the media type changes, but I've not used jQuery events yet.

# License #

This plugin uses the same dual-licensing model as jQuery: MIT and GPL

Just choose the one that suits you best :)