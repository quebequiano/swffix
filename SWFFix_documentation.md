## Why should you use SWFFix? ##

The A List Apart article Flash Embedding Cage Match [http://www.alistapart.com/articles/flashembedcagematch/ ](.md) describes the rationale behind SWFFix and why it should be better than any other available Flash embed method.

## How to use SWFFix? ##

SWFFix offers two ways to embed Flash content:
  1. Embed Flash content using standards compliant markup, and use SWFFix to resolve the issues that markup alone cannot solve
  1. Use SWFFix to dynamically embed Flash content (similar like SWFObject and UFO)

The advantage of the first method over the second is that the actual authoring of standards compliant markup is promoted and that the mechanism of inserting Flash content doesn't rely on JavaScript anymore, so it degrades properly:
  * If you have the Flash plug-in installed, but have JavaScript disabled or a use a browser that doesn't support JavaScript, you will still be able to see your Flash content
  * Flash will now also run on a device like Sony PSP, which has very poor JavaScript support
  * Automated tools like RSS readers are able to pick up Flash content

The advantage of the second method over the first is that it is easier to author, because it is less verbose and it contains no redundant code.

## How to embed Flash content using standards compliant markup and use SWFFix to solve its issues? (option 1) ##

### STEP 1: Embed both Flash content and alternative content using standards compliant markup ###

SWFFix' base markup uses the nested-objects method (with proprietary Internet Explorer conditional comments) [http://www.alistapart.com/articles/flashembedcagematch/ ](.md) to ensure the most optimal cross-browser support by means of markup only, while being standards compliant and supporting alternative content [http://www.swffix.org/testsuite/ ](.md):

```
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
<html xmlns="http://www.w3.org/1999/xhtml" lang="en" xml:lang="en">
  <head>
    <title>SWFFix - step 1</title>
    <meta http-equiv="Content-Type" content="text/html; charset=iso-8859-1" />
  </head>
  <body>
    <div>

      <object classid="clsid:D27CDB6E-AE6D-11cf-96B8-444553540000" width="780" height="420">
        <param name="movie" value="myContent.swf" />
        <!--[if !IE]>-->
        <object type="application/x-shockwave-flash" data="myContent.swf" width="780" height="420">
        <!--<![endif]-->
          <p>Alternative content</p>
        <!--[if !IE]>-->
        </object>
        <!--<![endif]-->
      </object>

    </div>
  </body>
</html>
```

Note 1: The nested-objects method requires a double `object` definition (the outer `object` targeting Internet Explorer and the inner `object` targeting all other browsers), so you need to define your `object` attributes and nested `param` elements twice.

Note 2: The `id` and `classid` attributes, and the `movie` `param` element should be used for the outer `object` only. The `type` and `data` attributes should be used by the inner `object` only.

Note 3: We advise not to use the `codebase` attribute to point to the URL of the Flash plugin installer on Adobe's servers, because this is illegal according to the specifications which restrict its access to the domain of the current document only. We recommend the use of alternative content with a subtle message that a user can have a richer experience by downloading the Flash plugin instead.

#### How can you use HTML to configure your Flash content? ####

You can add the following often-used optional attributes [http://www.w3schools.com/tags/tag\_object.asp ](.md) to the `object` element:
  * `id`
  * `name`
  * `class`
  * `align`

You can use the following optional Flash specific `param` elements [http://www.adobe.com/cfusion/knowledgebase/index.cfm?id=tn\_12701 ](.md):
  * `play`
  * `loop`
  * `menu`
  * `quality`
  * `scale`
  * `salign`
  * `wmode`
  * `bgcolor`
  * `base`
  * `swliveconnect`
  * `flashvars`
  * `devicefont` [http://www.adobe.com/cfusion/knowledgebase/index.cfm?id=tn\_13331 ](.md)
  * `allowscriptaccess` [http://www.adobe.com/cfusion/knowledgebase/index.cfm?id=tn\_16494 ](.md)
  * `seamlesstabbing` [http://www.adobe.com/support/documentation/en/flashplayer/7/releasenotes.html ](.md)
  * `allowfullscreen` [http://www.adobe.com/devnet/flashplayer/articles/full\_screen\_mode.html ](.md)
  * `allownetworking` [http://livedocs.adobe.com/flash/9.0/main/00001079.html ](.md)

#### Why should you use alternative content? ####

The `object` element allows you to nest alternative content inside of it, which will be displayed if Flash is not installed or supported. This content will also be picked up by search engines, making it a great tool for creating search-engine-friendly content. Summarizing, you should use alternative content when you like to create content that is accessible for people who browse the Web without plugins [http://www.adobe.com/devnet/flash/articles/progressive\_enhancement\_03.html ](.md), create search-engine-friendly content [http://www.adobe.com/devnet/flash/articles/progressive\_enhancement\_04.html ](.md) or tell visitors that they can have a richer user experience by downloading the Flash plugin.

#### What are the downsides of the nested-objects method? ####

When you take a close look at the cross-browser support of this markup approach, you will find the following deficiencies:
  * Safari 1.2.2 and lower will ignore all nested param elements
  * Internet Explorer 6+ on Windows XP SP2+ and Opera 9+ will include a click-to-activate mechanism
  * There is a risk that an outdated Flash player will run your Flash content with the possibility of broken or no content at all

The SWFFix JavaScript library will attempt to solve these issues. You can best see it an add-on, which primary goals are to fix issues of using standards compliant markup and to add functionality to improve the user experience.

### STEP 2: Include the SWFFix JavaScript library in the head of your HTML page ###

The SWFFix library consists of one external JavaScript file (currently 9.8Kb, GZIPed 3.2Kb). SWFFix will be executed as soon as it is read and will perform all DOM manipulations as soon as the DOM is loaded - for all browsers that support this, like IE, Firefox, Safari and Opera 9+ - or otherwise as soon as the onload event fires:

```
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
<html xmlns="http://www.w3.org/1999/xhtml" lang="en" xml:lang="en">
  <head>
    <title>SWFFix - step 2</title>
    <meta http-equiv="Content-Type" content="text/html; charset=iso-8859-1" />

    <script type="text/javascript" src="swffix.js"></script>

  </head>
  <body>
    <div>
      <object classid="clsid:D27CDB6E-AE6D-11cf-96B8-444553540000" width="780" height="420">
        <param name="movie" value="myContent.swf" />
        <!--[if !IE]>-->
        <object type="application/x-shockwave-flash" data="myContent.swf" width="780" height="420">
        <!--<![endif]-->
          <p>Alternative content</p>
        <!--[if !IE]>-->
        </object>
        <!--<![endif]-->
      </object>
    </div>
  </body>
</html>
```

### STEP 3: Register your Flash content with the SWFFix library and tell SWFFix what to do with it ###

First add a unique `id` to the outer `object` tag that defines your Flash content. Second add the `SWFFix.registerObject` method:
  1. The first argument (String, required) specifies the `id` used in the markup.
  1. The second argument (String, optional) specifies the Flash player version your content is published for. When used it activates the Flash version detection for a SWF to determine whether to show Flash content or force alternative content by doing a DOM manipulation. While Flash version numbers normally consist of major.minor.release.build, SWFFix only looks at the first 3 numbers, so both "WIN 9,0,18,0" (IE) or "Shockwave Flash 9 [r18](https://code.google.com/p/swffix/source/detail?r=18)" (all other browsers) will translate to "9.0.18".
  1. The third argument (String, optional) can be used to activate Adobe express install [http://www.adobe.com/cfusion/knowledgebase/index.cfm?id=6a253b75 ](.md) and specifies the URL of your express install SWF file. Express install displays a standardized Flash plugin download dialog instead of your Flash content when the required plugin version is not available. A default expressInstall.swf file is packaged with the project. It also contains the corresponding expressInstall.fla and AS files (in the SRC directory) to let you create your own custom express install experience.

```
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
<html xmlns="http://www.w3.org/1999/xhtml" lang="en" xml:lang="en">
  <head>
    <title>SWFFix - step 3</title>
    <meta http-equiv="Content-Type" content="text/html; charset=iso-8859-1" />
    <script type="text/javascript" src="swffix.js"></script>

    <script type="text/javascript">
    SWFFix.registerObject("myId", "9.0.0", "expressInstall.swf");
    </script>

  </head>
  <body>
    <div>

      <object id="myId" classid="clsid:D27CDB6E-AE6D-11cf-96B8-444553540000" width="780" height="420">

        <param name="movie" value="myContent.swf" />
        <!--[if !IE]>-->
        <object type="application/x-shockwave-flash" data="myContent.swf" width="780" height="420">
        <!--<![endif]-->
          <p>Alternative content</p>
        <!--[if !IE]>-->
        </object>
        <!--<![endif]-->
      </object>
    </div>
  </body>
</html>
```

## How to embed multiple SWF files into one HTML page (using option 1)? ##

Just repeat steps 1 and 3 (As mentioned under option 1: "How to embed Flash content using standards compliant markup and use SWFFix to solve its issues?") to add as many SWF files to your page as you like.

## How to use SWFFix to dynamically embed Flash content? (option 2) ##

### STEP 1: Create alternative content using standards compliant markup ###

SWFFix' dynamic embed method follows the principle of progressive enhancement [http://www.adobe.com/devnet/flash/articles/progressive\_enhancement.html ](.md) and replaces alternative HTML content for Flash content when enough JavaScript and Flash plug-in support is available. First define your alternative content and label it with an `id`:

```
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
<html xmlns="http://www.w3.org/1999/xhtml" lang="en" xml:lang="en">
  <head>
    <title>SWFFix dynamic embed - step 1</title>
    <meta http-equiv="Content-Type" content="text/html; charset=iso-8859-1" />
  </head>
  <body>
    
    <div id="myContent">
      <p>Alternative content</p>
    </div>
    
  </body>
</html>
```

### STEP 2: Include the SWFFix JavaScript library in the head of your HTML page ###

The SWFFix library consists of one external JavaScript file (currently 9.8Kb, GZIPed 3.2Kb). SWFFix will be executed as soon as it is read and will perform all DOM manipulations as soon as the DOM is loaded - for all browsers that support this, like IE, Firefox, Safari and Opera 9+ - or otherwise as soon as the onload event fires:

```
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
<html xmlns="http://www.w3.org/1999/xhtml" lang="en" xml:lang="en">
  <head>
    <title>SWFFix dynamic embed - step 2</title>
    <meta http-equiv="Content-Type" content="text/html; charset=iso-8859-1" />
  
    <script type="text/javascript" src="swffix.js"></script>

  </head>
  <body>
    <div id="myContent">
      <p>Alternative content</p>
    </div>
  </body>
</html>
```

### STEP 3: Embed your SWF with JavaScript ###

`SWFFix.embedSWF(swfUrl, id, width, height, version, expressInstallSwfurl, flashvars, params, attributes)` has five required and four optional arguments:
  1. swfUrl (String, required) specifies the URL of your SWF
  1. id (String, required) specifies the `id` of the HTML element (containing your alternative content) you would like to have replaced by your Flash content
  1. width (String, required) specifies the width of your SWF
  1. height (String, required) specifies the height of your SWF
  1. version (String, required) specifies the Flash player version your SWF is published for (format is: "major.minor.release")
  1. expressInstallSwfurl (String, optional) specifies the URL of your express install SWF and activates Adobe express install [http://www.adobe.com/cfusion/knowledgebase/index.cfm?id=6a253b75 ](.md)
  1. flashvars (Object, optional) specifies your flashvars with name:value pairs
  1. params (Object, optional) specifies your nested `object` element `params` with name:value pairs
  1. attributes (Object, optional) specifies your `object`'s attributes with name:value pairs

```
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
<html xmlns="http://www.w3.org/1999/xhtml" lang="en" xml:lang="en">
  <head>
    <title>SWFFix dynamic embed - step 3</title>
    <meta http-equiv="Content-Type" content="text/html; charset=iso-8859-1" />
    <script type="text/javascript" src="swffix.js"></script>
    
    <script type="text/javascript">
    SWFFix.embedSWF("myContent.swf", "myContent", "300", "120", "9.0.0");
    </script>

  </head>
  <body>
    <div id="myContent">
      <p>Alternative content</p>
    </div>
  </body>
</html>
```

#### How can you configure your Flash content? ####

You can add the following often-used optional attributes [http://www.w3schools.com/tags/tag\_object.asp ](.md) to the `object` element:
  * `id`
  * `name`
  * `class`
  * `align`

You can use the following optional Flash specific `param` elements [http://www.adobe.com/cfusion/knowledgebase/index.cfm?id=tn\_12701 ](.md):
  * `play`
  * `loop`
  * `menu`
  * `quality`
  * `scale`
  * `salign`
  * `wmode`
  * `bgcolor`
  * `base`
  * `swliveconnect`
  * `flashvars`
  * `devicefont` [http://www.adobe.com/cfusion/knowledgebase/index.cfm?id=tn\_13331 ](.md)
  * `allowscriptaccess` [http://www.adobe.com/cfusion/knowledgebase/index.cfm?id=tn\_16494 ](.md)
  * `seamlesstabbing` [http://www.adobe.com/support/documentation/en/flashplayer/7/releasenotes.html ](.md)
  * `allowfullscreen` [http://www.adobe.com/devnet/flashplayer/articles/full\_screen\_mode.html ](.md)
  * `allownetworking` [http://livedocs.adobe.com/flash/9.0/main/00001079.html ](.md)

#### How do you use JavaScript Objects to define your flashvars, params and object's attributes? ####

You can best define JavaScript Objects using the Object literal notation, which looks like:

```
<script type="text/javascript">

var flashvars = {};
var params = {};
var attributes = {};

SWFFix.embedSWF("myContent.swf", "myContent", "300", "120", "9.0.0","expressInstall.swf", flashvars, params, attributes);

</script>
```

You can add your name:value pairs while you define an object (note: please make sure not to put a comma behind the last name:value pair inside an object)

```
<script type="text/javascript">

var flashvars = {
  name1: "hello",
  name2: "world",
  name3: "foobar"
};
var params = {
  menu: "false"
};
var attributes = {
  id: "myDynamicContent",
  name: "myDynamicContent"
};

SWFFix.embedSWF("myContent.swf", "myContent", "300", "120", "9.0.0","expressInstall.swf", flashvars, params, attributes);

</script>
```

Or add properties and values after its creation by using a dot notation:

```
<script type="text/javascript">

var flashvars = {};
flashvars.name1 = "hello";
flashvars.name2 = "world";
flashvars.name3 = "foobar";

var params = {};
params.menu = "false";

var attributes = {};
attributes.id = "myDynamicContent";
attributes.name = "myDynamicContent";

SWFFix.embedSWF("myContent.swf", "myContent", "300", "120", "9.0.0","expressInstall.swf", flashvars, params, attributes);

</script>
```

Which can also be written as (the less readable shorthand version for the die-hard scripter who love one-liners):

```
<script type="text/javascript">

SWFFix.embedSWF("myContent.swf", "myContent", "300", "120", "9.0.0","expressInstall.swf", {name1:"hello",name2:"world",name3:"foobar"}, {menu:"false"}, {id:"myDynamicContent",name:"myDynamicContent"});

</script>
```

If you don't want to use an argument you can define it as 'null' or as an empty Object:

```
<script type="text/javascript">

var flashvars = null;
var params = {};
var attributes = {
  id: "myDynamicContent",
  name: "myDynamicContent"
};

SWFFix.embedSWF("myContent.swf", "myContent", "300", "120", "9.0.0","expressInstall.swf", flashvars, params, attributes);

</script>
```

The flashvars Object is a shorthand notation that is there for your ease of use. You could potentially ignore it and specify your flashvars via the params Object:

```
<script type="text/javascript">

var flashvars = null;
var params = {
  menu: "false",
  flashvars: "name1=hello&name2=world&name3=foobar"
};
var attributes = {
  id: "myDynamicContent",
  name: "myDynamicContent"
};

SWFFix.embedSWF("myContent.swf", "myContent", "300", "120", "9.0.0","expressInstall.swf", flashvars, params, attributes);

</script>
```

## How to embed multiple SWF files into one HTML page (using option 2)? ##

Just repeat steps 1 and 3 (As mentioned under option 2: "How to use SWFFix to dynamically embed Flash content?") to add as many SWF files to your page as you like.

## How can you use SWFFix to retrieve Flash player related information with JavaScript? ##

SWFFix contains a public API that enables you to retrieve Flash player related information with JavaScript.

`SWFFix.getFlashPlayerVersion()` returns a JavaScript object containing the major version (`major`:Number), minor version (`minor`:Number) and release version (`release`:Number) of an installed Flash player:

```
var playerVersion = SWFFix.getFlashPlayerVersion(); // returns a JavaScript object
var majorVersion = playerVersion.major; // access the major, minor and release version numbers via their respective properties
```

`SWFFix.hasFlashPlayerVersion(versionNumbersString)` returns a Boolean to indicate whether or not a specific version of the Flash plugin is installed:

```
if (SWFFix.hasFlashPlayerVersion("9.0.18")) {
  // has Flash
}
else {
  // no Flash
}
```

Please note that while Flash version numbers normally consist of major.minor.release.build, SWFFix only looks at the first 3 numbers, so both "WIN 9,0,18,0" (IE) or "Shockwave Flash 9 [r18](https://code.google.com/p/swffix/source/detail?r=18)" (all other browsers) will translate to "9.0.18".

## How does SWFFix differ from libraries like SWFObject and UFO? ##

Embed option 2 (as described under "How to use SWFFix?") works similar as SWFObject and UFO, and uses JavaScript to dynamically replace marked up alternative content by Flash content.

Embed option 1 departs from this conceptual principle by using standards compliant markup  to embed Flash content and use JavaScript to solve issues that cannot be solved by markup alone. The advantages of this new method are that the actual authoring of standards compliant markup is promoted and that the mechanism of inserting Flash content doesn't rely on JavaScript anymore, so it degrades properly:
  * If you have the Flash plug-in installed, but have JavaScript disabled or a use a browser that doesn't support JavaScript, you will still be able to see your Flash content
  * Flash will now also run on a device like Sony PSP, which has very poor JavaScript support
  * Automated tools like RSS readers are able to pick up Flash content

The advantage of the second option over the first is that it is easier to author, because it is less verbose and it contains no redundant code.

## What are the risks of using SWFFix? ##

### When using embed option 1 ###

Users that have JavaScript disabled or have browsers that don't support JavaScript at all or not well enough may face a degraded user experience caused by active content or in the worst case, broken Flash content. Let's calculate the odds:
  * 4% of all web users have no JavaScript support or have JavaScript disabled (source: thecounter.com [http://www.thecounter.com/stats/2007/June/javas.php ](.md)). When you consider that IE6, IE7 and Opera have a market share of around 80% [http://www.thecounter.com/stats/2007/June/browser.php ](.md), you can do the math that there is a risk of 3.2% (4% of 80%) on a degraded user experience (having to click to activate active content).
  * When you publish your Flash content for Flash player version 8 and a user has Flash Player 6 installed, there is a 0.2% (around 5% version gap of 4% no JavaScript) chance on broken or no content at all. When you publish your Flash content for Flash player version 8 and a user has Flash Player 7 installed this chance is already reduced to 0.1% (source: Adobe [http://www.adobe.com/products/player\_census/flashplayer/version\_penetration.html ](.md)).

Please note that although this approach still has its risks, the odds will be equal or lower than any other Flash embed method available.

### When using embed option 2 ###

Users that have JavaScript disabled or have browsers that don't support JavaScript at all, will never be able to see any Flash content at all (regardless whether the latest Flash player is installed or not), however they will see alternative content instead. The odds:
  * 4% of all web users have no JavaScript support or have JavaScript disabled (source: thecounter.com [http://www.thecounter.com/stats/2007/June/javas.php ](.md)).

## Does SWFFix support MIME type application/xhtml+xml? ##

SWFFix does NOT support XML MIME types, which is a design decision.

There are a number of reasons why we are not supporting this:
  * Only a very small (non-significant) amount of web authors is using it
  * We are unsure if it is the direction to go. Internet Explorer does not support it and all other major browser vendors are aiming their arrows at a new standard way of HTML parsing (with HTML 5), which departs from the current W3C vision of parsing HTML as XML
  * We save a considerate amount of file size and effort (testing, issue resolving) by not supporting it