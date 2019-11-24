# Export - Import Database
Useful for migrating site as well. Export the page descriptions. Make sure to read what all are exported. Then Import descriptions. Then import media.

## Export
**Main Article**: https://www.mediawiki.org/wiki/Manual:DumpBackup.php

Use PuTTy to reach the maintenance folder of the wiki installaption and run the script like:

```bash
php dumpBackup.php --full > dump.xml
```

## Import:
**Main Article:** https://www.mediawiki.org/wiki/Manual:Importing_XML_dumps#Using_Special:Import

### Directly via Special:Import
*Interwiki importance:*
https://www.mediawiki.org/wiki/Topic:Uhg6zbln33benp0x

You need to add an interwiki prefix to the respective field on special page "Import", e.g. if the xml file is from English wikipedia use "en" as prefix, if you are importing from a wiki called "foo.bar.com" and you are not sure about its prefix invent one. In this case "foo" appears to be a reasonable prefix. Remember to always use the same prefix for xml files coming from the same wiki. Moreover always used different prefixes for different wiki, so imports from "foo.baz.com" need e.g. "foo2" or so as prefix.

### Or via import script:
https://www.mediawiki.org/wiki/Manual:Importing_XML_dumps#Using_importDump.php,_if_you_have_shell_access

## Then import all images. 
Make a giant folder of all media. Specify the files you accept via file uploads. Then
https://www.mediawiki.org/wiki/Manual:ImportImages.php

In LocalSettings.php, something like this: 
```php
$wgEnableUploads = true;
# $wgUseImageMagick = true;
# $wgImageMagickConvertCommand = "/usr/bin/convert";
$wgFileExtensions = array( 'png', 'gif', 'jpg', 'jpeg', 'doc', 'pdf', 'ppt', 'mp4');
$wgMaxUploadSize = 1024 * 1024 * 150; #150 MB
```