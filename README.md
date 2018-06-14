# Frinck SDK for Android

Frinck SDK is a wrapper library which detect beacons near you and present detailed information about beacon properties. It provide information like UUID, Major, Minor, Store/Mall Image/Video, Title and Description of store/mall as well as redirection url. When a beacon is detected, you will receive a notification with its image and title. By tapping on it, it will redirect you to its detail page.

## Getting Started
### Installation

Below are the two ways to integrate Frinck Android SDK into your app.

- **Option 1:** Add librarybeacon as New Module To Your Project

  If you want to modify lib files at the time of development, you have to integrate librarybeacon as an Android Library Module to your project.
  
  For that you have to clone the librarybeacon present in current repository and you can import it as an Android Library in your project by following steps:
 
* Open your project in Android Studio.
* Copy and paste the librarybeacon folder to your Project folder.
* On the root of your project directory create/modify the settings.gradle file. It should contain something like the following:
```groovy
include 'YourApp', 'librarybeacon'
```
* gradle clean & build/close the project and reopen/re-import it.
* Edit your app's build.gradle to add this in the "dependencies" section:
```groovy
dependencies {
//...
    compile project(':librarybeacon')
}
```

**Option 2:** Compiling  AAR file by yourself and adding only AAR file.

  If you want to compile the SDK manually. Go through Example, begin by cloning the repository locally or retrieving the source code. Open the project in Android Studio and run the application:

Output file can be found in librarybeacon/build/outputs/ with extension .aar
  
You can add that AAR file to your project by following below steps.

  * Click File > New > New Module.
  * Click Import .JAR/.AAR Package then click Next.
  * Enter the location of the compiled AAR or JAR file then click Finish.
  * On the root of your project directory create/modify the settings.gradle file. It should contain something like the following:
  ```groovy
  include 'YourApp', 'librarybeacon'
  ```
  * gradle clean & build/close the project and reopen/re-import it.
  * Edit your app's build.gradle to add this in the "depencies" section:
    ```groovy
    dependencies {
      //...
      compile project(':librarybeacon')
    }
    ```

## Usage

Library contains two App Components:

* **BeaconScanningAppCompatActivity** which can be extended by your activity as below:
  ```groovy
  public class MainActivity extends BeaconScanningAppCompatActivity {
  
  @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        }
  //.....
  
  }
  ```
  * **BeaconScanningFragment** which can be extended by your Fragment as below:
  ```groovy
  public class FirstFragment extends BeaconScanningFragment {
  
  @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        }
  //.....
  
  }
  ```
 
## Important Methods

App component classes have few main methods which are essential for registering callbacks and checking favorable conditions before starting scan or any connection request, mentioned below:

**setUserData()** This mehtod is for setting user data into sdk to identify the user. You can set it by simply calling this method and passing JSONObject of user data as below:

```groovy

	    {
                        "client_id":"2",
                        "first_name":"John",
                        "last_name":"sys"
            }
```

```groovy
BeaconScanningService.setUserData(context, jsonObject);
```

**clearUserData()** Remove user data when you logged out from the app by calling this method:

```groovy
BeaconScanningService.clearUserData(context);
```

**requestLocationPermission()** This method is used to request location permission and handle bluetooth and GPS state and can be used as below:

```groovy

@Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        requestLocationPermission();
    }
```

**EventBus.getDefault().register(this);** This method is used to register callbacks in extended class coming from **BeaconScanningService** Class and can be used as below:

```groovy
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
	EventBus.getDefault().register(this);
    }
```

**EventBus.getDefault().unregister(this);** This method is used to unregister callbacks in extended class coming from **BeaconScanningService** Class and can be used as below:

```groovy
    @Override
    protected void onDestroy() {
        super.onDestroy();
        if (EventBus.getDefault().isRegistered(this)) {
            EventBus.getDefault().unregister(this);
        }
    }
```

**onBeaconListReceive(EventBeaconList beaconList);** This method is event callback method which trigger when beacon found near you and you can get list of nearby beacons by calling beaconList.getBeaconsList() mehtod as mentioned below:

```groovy
@Subscribe(threadMode = ThreadMode.MAIN)
    public void onBeaconListReceive(EventBeaconList beaconList) {
        onBeaconListResult(beaconList.getBeaconsList());
    }
```

This is the main implemented method in which gives all nearby beacons with its data. Implement this mehtod as mentioned below and get your reqired data:

```groovy
@Override
    protected void onBeaconListResult(ArrayList<BeaconListData> beaconsList) {

    }
```

If you want redirection url click analysis in dashboard of your admin panel then you need to post event in your view's click listener when user clicks on your view and you need to pass beaconId and url of that beacon as below:

```groovy
urlView.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                EventBus.getDefault().post(new EventClickAnalysis(boolean isClicked, String beaconId, String url));
               
            }
        });
```


## Author

LetsNurtureGit, android.letsnurture@gmail.com

## License

Frinck Sdk is available under the MIT license. See the LICENSE file for more info.
