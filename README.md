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


## (4) Circular ImageView

Description : 

A fast circular ImageView perfect for profile images. 

## Usage

Add the following dependency to your build.gradle file.

```groovy
   compile 'de.hdodenhof:circleimageview:2.1.0'  
```

Inside your layout add this XML tag

```XML
<de.hdodenhof.circleimageview.CircleImageView
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/profile_image"
    android:layout_width="96dp"
    android:layout_height="96dp"
    android:src="@drawable/profile"
    app:civ_border_width="2dp"
    app:civ_border_color="#FF000000"/>
```

## (5) Picasso

Images add much-needed context and visual flair to Android applications. Picasso allows for hassle-free image loading in your application—often in one line of code!


Many common pitfalls of image loading on Android are handled automatically by Picasso:

* Handling ImageView recycling and download cancelation in an adapter.
* Complex image transformations with minimal memory use.
* Automatic memory and disk caching.


## Usage

Add the following dependency to your build.gradle file.

```groovy
    compile 'com.squareup.picasso:picasso:2.5.2'
```

Add one of the following lines into your Activity

```java
Picasso.with(context).load(R.drawable.landing_screen).into(imageView1); // Loading from drawable folder
Picasso.with(context).load("file:///android_asset/DvpvklR.png").into(imageView2); // Loading from a file
Picasso.with(context).load(url).into(imageView3); // Loading from an online URL.
```

## (6) Card.io

Description : 

card.io provides fast, easy credit card scanning in mobile apps.

## Usage

## Requirements for card scanning

* Rear-facing camera.
* Android SDK version 16 (Android 4.1) or later.
* armeabi-v7a, arm64-v8, x86, or x86_64 processor.

Add the following dependency to your build.gradle file.

```groovy
compile 'io.card:android-sdk:5.4.2'
```

In your activit, call this method whenever you need to scan your card.

```java
public void onScanPress(View v) {
    Intent scanIntent = new Intent(this, CardIOActivity.class);

    // customize these values to suit your needs.
    scanIntent.putExtra(CardIOActivity.EXTRA_REQUIRE_EXPIRY, true); // default: false
    scanIntent.putExtra(CardIOActivity.EXTRA_REQUIRE_CVV, false); // default: false
    scanIntent.putExtra(CardIOActivity.EXTRA_REQUIRE_POSTAL_CODE, false); // default: false

    // MY_SCAN_REQUEST_CODE is arbitrary and is only used within this activity.
    startActivityForResult(scanIntent, MY_SCAN_REQUEST_CODE);
}
```

Next, we'll override onActivityResult() to get the scan result.

```java
@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
    super.onActivityResult(requestCode, resultCode, data);

    if (requestCode == MY_SCAN_REQUEST_CODE) {
        String resultDisplayStr;
        if (data != null && data.hasExtra(CardIOActivity.EXTRA_SCAN_RESULT)) {
            CreditCard scanResult = data.getParcelableExtra(CardIOActivity.EXTRA_SCAN_RESULT);

            // Never log a raw card number. Avoid displaying it, but if necessary use getFormattedCardNumber()
            resultDisplayStr = "Card Number: " + scanResult.getRedactedCardNumber() + "\n";

            // Do something with the raw number, e.g.:
            // myService.setCardNumber( scanResult.cardNumber );

            if (scanResult.isExpiryValid()) {
                resultDisplayStr += "Expiration Date: " + scanResult.expiryMonth + "/" + scanResult.expiryYear + "\n";
            }

            if (scanResult.cvv != null) {
                // Never log or display a CVV
                resultDisplayStr += "CVV has " + scanResult.cvv.length() + " digits.\n";
            }

            if (scanResult.postalCode != null) {
                resultDisplayStr += "Postal Code: " + scanResult.postalCode + "\n";
            }
        }
        else {
            resultDisplayStr = "Scan was canceled.";
        }
        // do something with resultDisplayStr, maybe display it in a textView
        // resultTextView.setText(resultDisplayStr);
    }
    // else handle other activity results
}
```






# Project Architecture

![architecture](https://firebasestorage.googleapis.com/v0/b/korashare-71b1f.appspot.com/o/project-arch.PNG?alt=media&token=d354d3e7-00da-4ab2-9a39-ffb5e669f018)

In this section, the project architecture will be discussed and we will be giving a breif about every package classs.

## (1) Activity Package

![activity](https://firebasestorage.googleapis.com/v0/b/korashare-71b1f.appspot.com/o/activity.PNG?alt=media&token=bf5ca832-dbe5-405a-b356-280fc27046b6)

This is were the activities live. We will give a breif about every activity.

### (1) AddCardActivity

![activity](https://firebasestorage.googleapis.com/v0/b/korashare-71b1f.appspot.com/o/addcard.png?alt=media&token=c5198bc0-60ad-486a-a295-3356bbb7ad04)

This is the activity were the user can add his card. The user can enter the card manually or scan it.


### (2) ChangePasswordActivity

This is were the user can change his password. It requires the old password, the new password and confirming the new password.

### (3) CreditCardListAcitivity

This activity contains the list of credit cards the user entered. The user might take some actions on any card.

### (4) ForgotPasswordAcitivty

The user is redirected to this activity whenever he clicks on the forgot password button. The activity asks the user for his email and then sends him an email with the new password.

### (5) LoginActivity

![activity](https://firebasestorage.googleapis.com/v0/b/korashare-71b1f.appspot.com/o/login.png?alt=media&token=a20bd535-e8d7-41f4-bdc6-3253eb6947d5)

This activity is the launcher activity. The user can navigate through different activities using the sign up, connect with google or facebook or sign in buttons. The login of facebook and google login is also handled from this activity.

### (6) MainActivity

This is the activity where the user is navigated to when he signs in successfully. The activity holder a navigation drawer to navigate to different activities and holds a frame layout as a parent view for different fragments to be viewed inside it.


### (7) PaymentActivity

The user is navigated to this activity whenever a payment is done. The activity holds a frame layout as a parent for fragments to live in. The fragments may be successfull transaction or failed transaction.

### (8) ScannerActivity

This activity is responsible for scanning the bar code or QR code. The activity is implemented with the "ZXing" library discussed before.

### (9) SecurityPinActivity

![activity](https://firebasestorage.googleapis.com/v0/b/korashare-71b1f.appspot.com/o/auth.png?alt=media&token=0e6e42c9-178e-4c0a-89e1-b6d9b1b5e541)


This is the activity where the user is asked to enter his authorization pin.

### (10) SettingsActivity

This is the activity where the user may view and edit his info.

### (11) SigninActivity & SignupActivity

These activities are responsible for letting the user enter thier credentials to be able to sign in and sign up.

![activity](https://firebasestorage.googleapis.com/v0/b/korashare-71b1f.appspot.com/o/singup.png?alt=media&token=3e6b6bc6-d143-4f55-aeea-d1d2438b2934)

### (12) SplashScreen

This activity is empty and redundant.

### (13) TermsWebAcitivity

This activity view to the users the terms that he must accept after he registers and before he proceed with the main activity. The activity consists of a fragment that contains a webview that appears to the user.

### (14) TransactionDetails & TransactionsActivity

These activities are used to view the transactions and the details of every transaction. 

## (2) Adapters Package

![activity](https://firebasestorage.googleapis.com/v0/b/korashare-71b1f.appspot.com/o/adap.PNG?alt=media&token=e3f1dff4-c9e4-4e87-ac12-f6d48f06a28d)

The adapters are a bridge between an AdapterView and the underlying data for that view. This is were we assign to ListView, RecyclerView and GridView (any dynamic holders that varies in size and data) their corresponding data. Therefore for every View that holds a list of items we must implement its own adapter. 


## (3) Callback Package

![activity](https://firebasestorage.googleapis.com/v0/b/korashare-71b1f.appspot.com/o/call.PNG?alt=media&token=c4d10f88-5683-4300-8468-e02dbd9b200d)

This package contains interfaces to the "Volley" library which are implemented in the classes that handles the API calls. By implementing these interfaces in the activity, we will be able to handle the logic of the implemented methods whenever called.


![activity](https://firebasestorage.googleapis.com/v0/b/korashare-71b1f.appspot.com/o/callb.PNG?alt=media&token=f47478ab-ee14-4be2-a011-667113d9197d)

As an example, we will be viewing the VolleyObjectCallback with implements the methods onSuccess() & onFailure.
The onSuccess is called whenever an API call is successfull unlike the onFailure which is called whenever the API call is having some errors.

## (4) Database Package

![activity](https://firebasestorage.googleapis.com/v0/b/korashare-71b1f.appspot.com/o/data.PNG?alt=media&token=adf46895-bcd9-4577-a6e2-ea6ce1a90870)

This package implements an SQLite database for storing data related to user, vendors, merchants, etc. The picture below will illustrate more.

![activity](https://firebasestorage.googleapis.com/v0/b/korashare-71b1f.appspot.com/o/datab.PNG?alt=media&token=ee1604b1-10b0-408f-bb87-2356a3594c40)

We have here 7 tables 
* User table
* Merchant table
* Vendor table 
* Category table
* MenuItem table
* Bank table
* Role table

## (5) Dialog Package

This package is redundant and not used.

## (6) Fragment Package

![activity](https://firebasestorage.googleapis.com/v0/b/korashare-71b1f.appspot.com/o/fragment.PNG?alt=media&token=d15d611d-2cc4-4246-9c55-6793b266a487)

Most of these fragments are inflated to the MainActivity based on the users navigation. For example, the Main fragment is the fragment that appears first when the user logins successfully.


![activity](https://firebasestorage.googleapis.com/v0/b/korashare-71b1f.appspot.com/o/main.png?alt=media&token=b48729ac-71e8-4d96-a453-d2ef45494738)

When the users click on the scan to pay button, the user is navigated to the ScanActivity which we discussed in the Activities Package.
Based on the user's clicks, the fragments replace each other making the whole applicaiton nearly run on a single activity (Except for some activities like signup, change password, etc).


## (7) Misc Package

![activity](https://firebasestorage.googleapis.com/v0/b/korashare-71b1f.appspot.com/o/misc.PNG?alt=media&token=b1c56e19-b7aa-45e7-92cd-988e06198d39)

The misc package contains some implemented mehtods but never used.


## (8) Model Package

![activity](https://firebasestorage.googleapis.com/v0/b/korashare-71b1f.appspot.com/o/model.PNG?alt=media&token=9e6bb890-8b0a-4839-a803-d095c37bb9a8)

This is a package contains data models as java classes. As shown above, this is the list of models used in the code. For example a user consists of the following parameters.

![activity](https://firebasestorage.googleapis.com/v0/b/korashare-71b1f.appspot.com/o/user.PNG?alt=media&token=2231a1d3-49b4-473c-a326-bf56dbe524f6)

The model also contains getters and setters to easily access the object's parameters.

![activity](https://firebasestorage.googleapis.com/v0/b/korashare-71b1f.appspot.com/o/getter.PNG?alt=media&token=a3732d91-7b4b-460f-889c-8fac208f1a1b)


The model contains converting a user to JSON and vice versa to be able to parse and send data corretly to the API.

![activity](https://firebasestorage.googleapis.com/v0/b/korashare-71b1f.appspot.com/o/data2.PNG?alt=media&token=a31b9c65-5a52-48ef-97a1-7c2ce4d5d524)

## (9) Serive Package

![activity](https://firebasestorage.googleapis.com/v0/b/korashare-71b1f.appspot.com/o/service.PNG?alt=media&token=a345df37-29e4-4489-ba18-6144a0fa3df7)

This package is one of the most important packages in the project. The package contains all the logic behind the API calls, Parsing responses and sending requests to the server. Every service will be discussed into details.





















