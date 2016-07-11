# Request Android Storage Permission for Titanium SDK 5.1.0+

## Description

for Titanium SDK 5.1.0+ & Android 6.0+

## Accessing the ti-android-requeststoragepermission Module

To access this module from JavaScript, you would do the following:

    var ti_android_requeststoragepermission = require("com.boxoutthinkers.reqstorageperm");

The ti_android_requeststoragepermission variable is a reference to the Module object.

## Method
	1. hasStoragePermission();

	2. requestStoragePermissions(e);

	3. settingsOpen();
		- Application's setting page open in Android's Setting(System).

## Reference

from Titanium SDK
https://github.com/appcelerator/titanium_mobile/blob/5_2_X/android/modules/media/src/java/ti/modules/titanium/media/MediaModule.java

## Usage
It's work on controller after window opend. (index.js or another controller, but not alloy.js)

    // request storage permission
    function checkPermissions(callback) {
      // request storage permission
      if (OS_ANDROID) {
        var RSP = require("com.boxoutthinkers.reqstorageperm");

        var ownedPermission = function() {
          // done
          callback && callback();
        };

        var requestStoragePerm = function() {
          if (!RSP.hasStoragePermission()) {
            RSP.requestStoragePermissions(function (e) {
              if (e.success) {
                // success
                Ti.API.info('requestStoragePermission : success');
                ownedPermission();
              } else {
                // retry or settings open
                Ti.API.error('requestStoragePermission : error');
                var dialog = Ti.UI.createAlertDialog({
                  message: msg,
                  cancel: 1,
                  buttonNames: ["OK", "Open Setting"],
                  title: "Need Storeage Permission"
                });
                dialog.addEventListener('click', function(e){
                  if (e.index === e.source.cancel){
                    Ti.API.debug('settings open');
                    var resumedFn = function() {
                      requestStoragePerm();
                      Ti.Android.currentActivity.onStart = null;
                      Ti.Android.currentActivity.onResume = null;
                    };
                    Ti.Android.currentActivity.onStart = resumedFn;
                    Ti.Android.currentActivity.onResume = resumedFn;

                    // settings open
                    RSP.settingsOpen();
                  } else {
                    requestStoragePerm();
                  }
                });
                dialog.show();
              }
            });
          } else {
            Ti.API.info('requestStoragePermission : already have');
            ownedPermission();
          }
        };

        var checkNRequestStoragePerm = function () {
          if (!RSP.hasStoragePermission()) {
            var dialog = Ti.UI.createAlertDialog({
              message: "Need Storeage Permisson. Because, .......",
              ok: "OK",
              title: "Need Storeage Permission"
            });
            dialog.addEventListener('click', function(e){
              requestStoragePerm();
            });
            dialog.show();
          } else {
            Ti.API.info('requestStoragePermission : already have');
            ownedPermission();
          }
        };

        // do check
        checkNRequestStoragePerm();
      } else {
        // non android
        callback && callback();
      }
    }
    
    // run
    checkPermissions(function callback() {...});
  

## Author

Dongwoo Gim, BOXOUT THINKERS

## License

MIT License
