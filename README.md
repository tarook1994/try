# Dependencies

## (1) Dexter


Description : 

Dexter is an Android library that simplifies the process of requesting permissions at runtime.

Android Marshmallow includes a new functionality to let users grant or deny permissions when running an app instead of granting them all when installing it. This approach gives the user more control over applications but requires developers to add lots of code to support it.

The official API is heavily coupled with the Activity class. Dexter frees your permission code from your activities and lets you write that logic anywhere you want.

## Usage


Include the library in your build.gradle

```groovy
compile 'com.karumi:dexter:2.3.1'
```

To start using the library you just need to call Dexter with a valid Activity:


```java
Dexter.withActivity(this)
	.withPermission(Manifest.permission.CAMERA)
	.withListener(new PermissionListener() {
		@Override public void onPermissionGranted(PermissionGrantedResponse response) {/* ... */}
		@Override public void onPermissionDenied(PermissionDeniedResponse response) {/* ... */}
		@Override public void onPermissionRationaleShouldBeShown(PermissionRequest permission, PermissionToken token) {/* ... */}
	}).check();
```

This library was used to request **STORAGE** and **CAMERA** permissions.


## (2) ZXing Scanner

description : 

Android library projects that provides easy to use and extensible Barcode Scanner views based on ZXing and ZBar.

## Usage

(1) Add the following dependency to your build.gradle file.

```groovy
    compile 'me.dm7.barcodescanner:zxing:1.9'
```

(2) Add camera permission to your AndroidManifest.xml file:

```xml
<uses-permission android:name="android.permission.CAMERA" />
```
(3) Use it in your activity as the example provided
```java
public class SimpleScannerActivity extends Activity implements ZXingScannerView.ResultHandler {
    private ZXingScannerView mScannerView;

    @Override
    public void onCreate(Bundle state) {
        super.onCreate(state);
        mScannerView = new ZXingScannerView(this);   // Programmatically initialize the scanner view
        setContentView(mScannerView);                // Set the scanner view as the content view
    }

    @Override
    public void onResume() {
        super.onResume();
        mScannerView.setResultHandler(this); // Register ourselves as a handler for scan results.
        mScannerView.startCamera();          // Start camera on resume
    }

    @Override
    public void onPause() {
        super.onPause();
        mScannerView.stopCamera();           // Stop camera on pause
    }

    @Override
    public void handleResult(Result rawResult) {
        // Do something with the result here
        Log.v(TAG, rawResult.getText()); // Prints scan results
        Log.v(TAG, rawResult.getBarcodeFormat().toString()); // Prints the scan format (qrcode, pdf417 etc.)

        // If you would like to resume scanning, call this method below:
        mScannerView.resumeCameraPreview(this);
    }
}
```

The library was used to scan the credit card.

## (3) Volley

Description : 

Volley is an HTTP library that makes networking for Android apps easier and, most importantly, faster.

## Usage

Add the following dependency to your build.gradle file.

```groovy
    compile 'com.android.volley:volley:1.0.0'
```

Inside Your activity


```java
JsonObjectRequest tabbRequest = new JsonObjectRequest(Request.Method.POST, url, parameters, new Response.Listener<JSONObject>() {
                @Override
                public void onResponse(JSONObject response) {
                    Log.d("response: ", response.toString());
                    UserService.saveLoginDetails(context, response);
                    callback.onSuccess(response);
                }
            }, new Response.ErrorListener() {
                @Override
                public void onErrorResponse(VolleyError error) {
                    Log.d("response: ", error.toString());
                    callback.onFailure(error);
                }
            }) {
                @Override
                public Map<String, String> getHeaders() throws AuthFailureError {
                    MiscHelper miscHelper = new MiscHelper();
                    Map<String, String> MyData = new HashMap<>();
                    return MyData;
                }
            };

            VolleySingleton.getInstance(context).addToRequestQueue(tabbRequest);
        }
```

JSONRequest is used to send a request with a raw (JSON) body. It takes 4 parameters. 


1- HTTP method (GET, POST, PUT, etc)

2- The JSON Object of the body

3- Interface of reponse listener. 

4- Interface of error listener

The reponse is handled on the onResponse callback method while the error is handled on the onError callback method.

      
