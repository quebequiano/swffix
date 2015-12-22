## SWFFix release notes ##

### Version 0.3 public alpha ###

**Release date: August 20th, 2007**

This version includes the following changes:

  1. The API got an overhaul to optimize the library’s usability. It is more simplified and looks like:
    * SWFFix.registerObject(objectIdStr, swfVersionStr, xiSwfUrlStr); [We removed the object notation. ](.md)
    * SWFFix.embedSWF(swfUrlStr, replaceElemIdStr, widthStr, heightStr, swfVersionStr, xiSwfUrlStr, flashvarsObj, parObj, attObj); [Is new and replaces the previous dynamic embed method. It now also supports express install and is closer to the SWFObject/UFO notation. ](.md)
    * SWFFix.getFlashPlayerVersion();
    * SWFFix.hasFlashPlayerVersion(versionStr);
  1. We compressed the library (with Dojo Shrinksafe: [http://alex.dojotoolkit.org/shrinksafe/ ](.md)) for file size optimization, and created a SRC directory that includes the uncompressed library (for means of development) and express install source files
  1. We fixed an express install related bug and now check if a SWF’s height is 137px (instead of the earlier 130px) or higher to avoid Download.Fail status calls to fire

### Version 0.2 public alpha ###

**Release date: July 25th, 2007**