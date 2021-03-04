

## [![Netcore Logo](https://netcore.in/wp-content/themes/netcore/img/Netcore-new-Logo.png)](http:www.netcore.in)  FCM SDK For Native Android Apps
Smartech is a omni channel platform that delivers everything you need to drive mobile engagement and create valuable consumer relationships on mobile. The Smartech android SDK enables your native android app to use all of the features. This guide contains all the information you need to integrate the Smartech Android SDK into your app.

### Prerequisites
1.  A Smartech Account.
2.  `google-services.json` file from [Firebase Console](https://console.firebase.google.com/)
3.  Server Key​​ from ​Firebase Console.
4.  A Smartech Android App Id which can be obtained from the Smartech panel.
5.  Install or update Visual Studio to the latest version.
6.  Ensure that the Android app targets API level 16 (Jelly Bean) or later and uses Gradle 4.1 or later.

###  Android Project:
##### 1. Create new Android project.
##### 2. Add Reference of Smartech.dll.
##### 3. Add google-services.json file in app directory of the project.
##### 4. Adding following Nuget Packages to android project.​

```java
Install-Package GoogleGson -Version 2.8.5
Install-Package Newtonsoft.Json -Version 12.0.3
Install-Package Xamarin.GooglePlayServices.Ads -Version 71.1720.1
Install-Package Xamarin.GooglePlayServices.Base -Version 71.1610.0  
Install-Package Xamarin.Firebase.Messaging -Version 71.1740.0
Install-Package Xamarin.AndroidX.Work.Runtime -Version 2.5.0
Install-Package Xamarin.Kotlin.StdLib.Jdk7 -Version 1.3.50.1

**Note:​​**
-   One can avoid using **‘com.google.android.gms:play-services-ads’**
dependency if an app does not want Smartech to fetch Advertising Id of the device.

#### To register device for push notifications
To register the device for receiving push notifications from Smartech panel​, add given snippet inside the **onCreate method of the Application class​​**.
```java
Smartech smartech = Smartech.GetInstance(new Java.Lang.Ref.WeakReference(ApplicationContext));
smartech.InitializSdk(this);
```
#### To capture user login
To capture login activity of the user, add given snippet inside the
activity when the user gets logged in successfully.
```java
smartech.UserIdentity = "<unique_user_identity>";
smartech.Login("<unique_user_identity>");
```
#### To clear user's identity
To clear user's identity, call below method.
```java
smartech.clearUserIdentity();
```

#### To capture user logout
Call the Smartech logout method after the user log-out from the application. If you want to clear the user identity pass true in the parameter. When you logout the user with identity clear, you won't be receiving any personalised push notifications.
```java
smartech.LogoutAndClearUserIdentity(true);
```
**Note:​​** Avoid passing true in  ‘LogoutAndClearUserIdentity’ method if one wants to track user activity
even if user has logged out of the application.

#### To capture custom activity
To capture custom activity performed by the user, add given snippet as per
the requirement.
```java
 smartech.TrackEvent("Page Browse", payload);

e.g.
try
{
  IDictionary<string, Java.Lang.Object> payload = new Dictionary<string, Java.Lang.Object>();
  payload.Add("name", "test");
  payload.Add("prid", 2);
  payload.Add("price", 15000.00);
  smartech.TrackEvent("Page Browse", payload);
}
catch (Exception ex)
{
  Console.WriteLine(ex.Message);
}
```
**Note​​:** Prior implementation of custom activity tracking with event id will be also supported by the SDK.
#### To capture user attributes
To capture and map user attributes, add given snippet as per the
requirement.
```java
smartech.UpdateUserProfile(<user-data>);

e.g. 
try
{
 IDictionary<string, Java.Lang.Object> userData = new Dictionary<string, Java.Lang.Object>();
 userData.Add("FIRST NAME", edtxtName.Text);
 userData.Add("LAST NAME", edtxtLastName.Text);
 userData.Add("AGE", edtxtAge.Text);
 smartech.UpdateUserProfile(userData);
}
catch (Exception ex){}
```
**Note:** Use attribute name in capital letters as shown above.

#### To change push notification icon color (From SDK version 1.2.8)
To change the color of notification icon, add given snippet inside the **onCreate method of the Application class​​**.
```java
NetcoreSDK.SetPushIconColor(context, <argb>);

e.g.
NetcoreSDK.SetPushIconColor(context, ContextCompat.GetColor(this.BaseContext,Android.Resource.Color.HoloRedLight));
```
#### To reset push notification icon color 
To reset color of notification icon add given snippet as per the requirement.  
```java
NetcoreSDK.ResetPushIconColor(context);
```

#### To use custom push notification icon 
With the launch of Android 5 (Lollipop), the notification icons are rendered differently in the notification bar at the top. By default, Smartech SDK uses the app’s icon for both, the small icon as well as the large icon.

To set a custom notification icon, add the following meta data entry in your `AndroidManifest.xml`
```xml
<meta-data  
android:name="SMT_LARGE_NOTIFICATION_ICON"  
android:value="<notification_icon_name>"/> 

<meta-data  
android:name="SMT_SMALL_NOTIFICATION_ICON"  
android:value="<notification_icon_name>"/>
```
**Note:**  "**notification_icon_name**" is the name of your file in the drawable directory without the file extension.
The method **NetcoreSDK.setPushIcon()** is deprecated from SDK version 1.2.8.

The notification icon being used should strictly be in .png format as per
[Google’s guidelines](https://developer.android.com/guide/practices/ui_guidelines/icon_design_status_bar)​. Preferable size for the push notification icons are
mentioned below.
```c
drawable-mdpi        :     24 x 24
drawable-hdpi        :     36 x 36 
drawable-xhdpi        :     48 x 48
drawable-xxhdpi        :     72 x 72
drawable-xxxhdpi    :     96 x 96
```

#### To fetch delivered push notifications
To fetch delivered push notifications, add given snippet as per the
requirement.
```java
JSONArray notifications = NetcoreSDK.GetNotifications(context);
OR
JSONArray notifications = NetcoreSDK.GetNotifications(context, <count>);
```
**Note:** The method returns a ‘JSONArray’ of delivered push notifications for
the user. ​**By default the method returns last 10 delivered notifications​​**.

#### To implement push notifications in existing FCM class
To use push notifications from Smartech panel along with custom set up of the FCM class, add given snippet ​**inside the FCM receiver class​​**.
```java
bool pushFromSmartech = smartech.HandlePushNotification(p0.Data.MyToString());
```
**Note​​:** The method returns a boolean value.

- Returns **true​**, if the push notification is received from the Smartech panel. It will also render the push notification without any extra efforts further.

- Return ​**false​**, if the push notification is not received from the Smartech panel. In this case, handle the push notification at your end as per the
requirement.
#### To opt out user from being tracked (GDPR Policy)
If the end user wants to opt out of being tracked, add given snippet as per the
requirement.
```java
smartech.OptOut(<boolean_flag>);
```
**Note​​:** The method accepts a boolean value.

- If an end user wants to opt out, the flag should be passed as **true**. Once the user opts out, SDK will not be able to track that particular user further and no communications will be received by that user.

- If an end user wants to opt in, the flag should be passed as **false**. Once the user opts in, SDK will be able to track that particular user further and next communications will be received by that user.

#### To implement location tracking
In order to track user location and pass it further to Smartech, add given
snippet as per the requirement.
```java
smartech.SetUserLocation(<obj_location>);

e.g.
Android.Locations.Location location = new Android.Locations.Location("");
location.Latitude = 19.185664;
location.Longitude = 72.9808896;
smartech.SetUserLocation(location);
```
#### To clear user identity
In order to wipe out user identity from the SDK, add given snippet as per the
requirement.
```java
NetcoreSDK.ClearIdentity(context);
```
#### To set existing FCM token
To set existing FCM token of the application to the SDK, add given snippet as per the requirement.
```java
smartech.DevicePushToken = "<token>";

e.g.
smartech.DevicePushToken = "a34dcQAseweer";
```
#### To get GUID of the user
To obtain GUID of the user from the SDK, add given snippet as per the requirement.
```java
String guid = smartech.DeviceUniqueId;
```
#### To get FCM token of the user
To obtain the FCM token of the user from the SDK, add given snippet as per the requirement.
```java
String token = smartech.DevicePushToken;
```
#### To implement deeplink in the application
To implement deeplink in the application, add given snippet **inside AndroidManifest.xml file with in the Activity Tag**.
```xml
IntentFilter(new[]  {  Android.Content.Intent.ActionView  },  
DataScheme  =  <scheme>,  
DataHost  =  <host>,  
AutoVerify  =true,  
Categories  =  new[]  {  Android.Content.Intent.CategoryDefault,  Android.Content.Intent.CategoryBrowsable  })]

e.g.
IntentFilter(new[]  {  Android.Content.Intent.ActionView  },  
DataScheme  =  "http",  
DataHost  =  "home.netcore.com",  
AutoVerify  =true,  
Categories  =  new[]  {  Android.Content.Intent.CategoryDefault,  Android.Content.Intent.CategoryBrowsable  })]
```

#### Handling Custom In-App HTML
You must implement the InAppCustomtion HTMLListener and call smartech.setInAppCustomHTMLListener() method in your application class if you are using custom HTML in-app messages.
```java
[Application]
    public class NetcoreApplication : Application,IInAppCustomHTMLListener
    {
        public NetcoreApplication(IntPtr handle, JniHandleOwnership ownerShip) : base(handle, ownerShip)
        {
        }

        public override void OnCreate()
        {
            base.OnCreate();
            Smartech smartech = Smartech.GetInstance(new Java.Lang.Ref.WeakReference(ApplicationContext));
            smartech.InitializeSdk(this);
            smartech.SetInAppCustomHTMLListener(this);
        }

        public void CustomHTMLCallback(IDictionary<string, Java.Lang.Object> p0)
        {
            Console.WriteLine("InAppCustomListener" + p0.ToString());

        }
    }
```

#### To fetch Advertising Id
To fetch Advertising Id using Smartech SDK of the device, add given snippet **in the AndroidManifest.xml file inside the application tag**.
```xml
<meta-data  android:name="com.google.android.gms.ads.APPLICATION_ID"  android:value=<applicationId>  />
<meta-data
android:name="SMT_USE_AD_ID"
android:value="<value>"/>

e.g.
<meta-data  android:name="com.google.android.gms.ads.APPLICATION_ID"  android:value=<abc....>  />
<meta-data
android:name="SMT_USE_AD_ID"
android:value="1"/>
```
**Note​​:** The method accepts either **‘0’** or **‘1’** as value.

- If an app wants Smartech SDK to fetch Advertising Id of the device, use **‘1’** as value.

- If an app does not want Smartech SDK to fetch Advertising Id of the device, use **‘0’** as value.

#### To fetch custom payload from push notifications
To fetch custom payload data from the push notifications, **add given snippet in the activity of the application** as per the requirement.
```java
Bundle  bundle  =  Intent.Extras;  
if  (bundle  !=  null)  
foreach  (var  key  in  bundle.KeySet())  
{  
Log.Debug("TAG",  key  +  " : "  +  bundle.Get(key).ToString());  
Toast.MakeText(this.BaseContext,  key  +  " : "  +  bundle.Get(key).ToString(),  ToastLength.Long).Show();  
}
```
#### To fetch unread push notification count
To fetch unread push notification count from the NetcoreSDK, add given snippet as per the requirement.
```java
int count = NetcoreSDK.GetUnreadNotificationsCount(Context context);
```
**Note​​:** The method returns the count of unread notifications only that has been received in the application via Smartech SDK. 


