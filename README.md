# Request Android Storage Permission for Titanium SDK 5.1.0+

## Description

for Titanium SDK 5.1.0+ & Android 6.0+

## Accessing the ti-android-requeststoragepermission Module

To access this module from JavaScript, you would do the following:

    var ti_android_requeststoragepermission = require("com.boxoutthinkers.reqstorageperm");

The ti_android_requeststoragepermission variable is a reference to the Module object.

## Reference

from Titanium SDK
https://github.com/appcelerator/titanium_mobile/blob/5_2_X/android/modules/media/src/java/ti/modules/titanium/media/MediaModule.java

## Usage
It's work on controller after window opend. (not alloy.js)

	// request storage permission
	if (OS_ANDROID) {
		var requestStoragePermission = function() {
			var RSP = require("com.boxoutthinkers.reqstorageperm");
			if (!RSP.hasStoragePermission()) {
				RSP.requestStoragePermissions(function(e) {
					if (e.success) {
						// success
						Ti.API.info('requestStoragePermission : success');
					} else {
						Ti.API.error('requestStoragePermission : error');
					}
				});
			} else {
				Ti.API.info('requestStoragePermission : already have');
			}
		}
		// do
		requestStoragePermission();
	}


## Author

Dongwoo Gim, BOXOUT THINKERS

## License

MIT License
