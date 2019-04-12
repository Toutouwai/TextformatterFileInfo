# File Info

A textformatter module for ProcessWire. The module can add information to local Pagefile links in two ways: 

1. As extra markup before, within or after the link
2. As data attributes on the link (handy if you want to use a Javascript tooltip library, for instance)

## Screenshots

### Module config

![file-info-config](https://user-images.githubusercontent.com/1538852/56021059-a0f07b80-5d5c-11e9-8c11-9edc9ffe2d3f.png)

### Example of output

![file-info-output](https://user-images.githubusercontent.com/1538852/56021081-aa79e380-5d5c-11e9-9e3d-0510056e13e5.png)

## Installation

[Install](http://modules.processwire.com/install-uninstall/) the File Info module.

Add the textformatter to one or more CKEditor fields.

## Configuration

### Add markup action (and general)

* Select "Add markup to links"
* Select the Pagefile attributes that will be retrieved. The attribute "filesizeStrCustom" is similar to the core "filesizeStr" attribute but allows for setting a custom number of decimal places.
* If you select the "modified" or "created" attributes then you can define a date format for the value.
* Enter a class string to add to the links if needed.
* Define the markup that will be added to the links. Surround Pagefile attribute names in {brackets}. Attributes must be selected in the "Pagefile attributes" section in order to be available in the added markup. If you want include a space character at the start or end of the markup then you'll need >= PW 3.0.128.
* Select where the markup should be added: prepended or appended within the link, before the link, or after the link.

### Add data attributes action

* Select "Add data attributes to links"
* Select the Pagefile attributes that will be retrieved. These attributes will be added to the file links as data attributes. Attributes with camelcase names will be converted to data attribute names that are all lowercase, i.e. filesizeStrCustom becomes data-filesizestrcustom.

### Hook

If you want to customise or add to the attributes that are retrieved from the Pagefile you can hook `TextformatterFileInfo::getFileAttributes()`. For example:

```php
$wire->addHookAfter('TextformatterFileInfo::getFileAttributes', function(HookEvent $event) {
	$pagefile = $event->arguments(0);
	$page = $event->arguments(1);
	$field = $event->arguments(2);
	$attributes = $event->return;
	
	// Add a new attribute
	$attributes['sizeNote'] = $pagefile->filesize > 10000000 ? 'This file is pretty big' : 'This file is not so big';
	$event->return = $attributes;
});
```