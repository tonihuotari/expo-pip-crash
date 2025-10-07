Reproduce crash by:

1.
Add android:supportsPictureInPicture="true" to AndroidManifest:
<manifest xmlns:android="http://schemas.android.com/apk/res/android">
  <uses-permission android:name="android.permission.INTERNET"/>
  <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE"/>
  <uses-permission android:name="android.permission.SYSTEM_ALERT_WINDOW"/>
  <uses-permission android:name="android.permission.VIBRATE"/>
  <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
  <queries>
    <intent>
      <action android:name="android.intent.action.VIEW"/>
      <category android:name="android.intent.category.BROWSABLE"/>
      <data android:scheme="https"/>
    </intent>
  </queries>
  <application android:name=".MainApplication" android:label="@string/app_name" android:icon="@mipmap/ic_launcher" android:roundIcon="@mipmap/ic_launcher_round" android:allowBackup="true" android:theme="@style/AppTheme" android:supportsRtl="true" android:enableOnBackInvokedCallback="false">
    <meta-data android:name="expo.modules.updates.ENABLED" android:value="false"/>
    <meta-data android:name="expo.modules.updates.EXPO_UPDATES_CHECK_ON_LAUNCH" android:value="ALWAYS"/>
    <meta-data android:name="expo.modules.updates.EXPO_UPDATES_LAUNCH_WAIT_MS" android:value="0"/>
    <activity 
    android:name=".MainActivity" 
    android:supportsPictureInPicture="true" 
    android:configChanges="keyboard|keyboardHidden|orientation|screenSize|screenLayout|uiMode" android:launchMode="singleTask" android:windowSoftInputMode="adjustResize" android:theme="@style/Theme.App.SplashScreen" android:exported="true" android:screenOrientation="portrait">
      <intent-filter>
        <action android:name="android.intent.action.MAIN"/>
        <category android:name="android.intent.category.LAUNCHER"/>
      </intent-filter>
      <intent-filter>
        <action android:name="android.intent.action.VIEW"/>
        <category android:name="android.intent.category.DEFAULT"/>
        <category android:name="android.intent.category.BROWSABLE"/>
        <data android:scheme="exp+myandroidapp"/>
      </intent-filter>
    </activity>
  </application>
</manifest>

2. Add this to MainActivity:

import android.app.PictureInPictureParams;
import android.util.Rational;

override fun onUserLeaveHint() {
    super.onUserLeaveHint()
    if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
      val pip = PictureInPictureParams.Builder()
        .setAspectRatio(Rational(16, 9)) // Adjust as needed
        .build()
      enterPictureInPictureMode(pip)
    }
  }

3. Run app and press home button to trigger Picture-in-Picture and then app crashes:

Error logs:
 ERROR  Your app just crashed. See the error below.
java.lang.RuntimeException: Unable to start activity ComponentInfo{com.axelinternet.reactnativeplayerexample/expo.modules.devlauncher.launcher.errors.DevLauncherErrorActivity}: java.lang.reflect.InvocationTargetException
  android.app.ActivityThread.performLaunchActivity(ActivityThread.java:2957)
  android.app.ActivityThread.handleLaunchActivity(ActivityThread.java:3032)
  android.app.ActivityThread.-wrap11(Unknown Source:0)
  android.app.ActivityThread$H.handleMessage(ActivityThread.java:1696)
  android.os.Handler.dispatchMessage(Handler.java:105)
  android.os.Looper.loop(Looper.java:164)
  android.app.ActivityThread.main(ActivityThread.java:6944)
  java.lang.reflect.Method.invoke(Native Method)
  com.android.internal.os.Zygote$MethodAndArgsCaller.run(Zygote.java:327)
  com.android.internal.os.ZygoteInit.main(ZygoteInit.java:1374)
Caused by java.lang.reflect.InvocationTargetException
  java.lang.reflect.Constructor.newInstance0(Native Method)
  java.lang.reflect.Constructor.newInstance(Constructor.java:334)
  androidx.lifecycle.viewmodel.internal.JvmViewModelProviders.createViewModel(JvmViewModelProviders.kt:38)
  androidx.lifecycle.ViewModelProvider$NewInstanceFactory.create(ViewModelProvider.android.kt:185)
  androidx.lifecycle.ViewModelProvider$AndroidViewModelFactory.create(ViewModelProvider.android.kt:309)
  androidx.lifecycle.ViewModelProvider$AndroidViewModelFactory.create(ViewModelProvider.android.kt:291)
  androidx.lifecycle.ViewModelProvider$AndroidViewModelFactory.create(ViewModelProvider.android.kt:265)
  androidx.lifecycle.SavedStateViewModelFactory.create(SavedStateViewModelFactory.android.kt:142)
  androidx.lifecycle.SavedStateViewModelFactory.create(SavedStateViewModelFactory.android.kt:112)
  androidx.lifecycle.viewmodel.ViewModelProviderImpl_androidKt.createViewModel(ViewModelProviderImpl.android.kt:34)
  androidx.lifecycle.viewmodel.ViewModelProviderImpl.getViewModel$lifecycle_viewmodel_release(ViewModelProviderImpl.kt:60)
  androidx.lifecycle.viewmodel.ViewModelProviderImpl.getViewModel$lifecycle_viewmodel_release$default(ViewModelProviderImpl.kt:43)
  androidx.lifecycle.ViewModelProvider.get(ViewModelProvider.android.kt:92)
  androidx.lifecycle.ViewModelLazy.getValue(ViewModelLazy.kt:52)
  androidx.lifecycle.ViewModelLazy.getValue(ViewModelLazy.kt:35)
  expo.modules.devlauncher.launcher.errors.DevLauncherErrorActivity.getViewModel(DevLauncherErrorActivity.kt:17)
  expo.modules.devlauncher.launcher.errors.DevLauncherErrorActivity.onCreate(DevLauncherErrorActivity.kt:39)
  android.app.Activity.performCreate(Activity.java:7183)
  android.app.Instrumentation.callActivityOnCreate(Instrumentation.java:1220)
  android.app.ActivityThread.performLaunchActivity(ActivityThread.java:2910)
  android.app.ActivityThread.handleLaunchActivity(ActivityThread.java:3032)
  android.app.ActivityThread.-wrap11(Unknown Source:0)
  android.app.ActivityThread$H.handleMessage(ActivityThread.java:1696)
  android.os.Handler.dispatchMessage(Handler.java:105)
  android.os.Looper.loop(Looper.java:164)
  android.app.ActivityThread.main(ActivityThread.java:6944)
  java.lang.reflect.Method.invoke(Native Method)
  com.android.internal.os.Zygote$MethodAndArgsCaller.run(Zygote.java:327)
  com.android.internal.os.ZygoteInit.main(ZygoteInit.java:1374)
Caused by java.lang.NullPointerException: null cannot be cast to non-null type expo.modules.devlauncher.DevLauncherController
  expo.modules.devlauncher.compose.models.ErrorViewModel.<init>(ErrorViewModel.kt:62)
  java.lang.reflect.Constructor.newInstance0(Native Method)
  java.lang.reflect.Constructor.newInstance(Constructor.java:334)
  androidx.lifecycle.viewmodel.internal.JvmViewModelProviders.createViewModel(JvmViewModelProviders.kt:38)
  androidx.lifecycle.ViewModelProvider$NewInstanceFactory.create(ViewModelProvider.android.kt:185)
  androidx.lifecycle.ViewModelProvider$AndroidViewModelFactory.create(ViewModelProvider.android.kt:309)
  androidx.lifecycle.ViewModelProvider$AndroidViewModelFactory.create(ViewModelProvider.android.kt:291)
  androidx.lifecycle.ViewModelProvider$AndroidViewModelFactory.create(ViewModelProvider.android.kt:265)
  androidx.lifecycle.SavedStateViewModelFactory.create(SavedStateViewModelFactory.android.kt:142)
  androidx.lifecycle.SavedStateViewModelFactory.create(SavedStateViewModelFactory.android.kt:112)
  androidx.lifecycle.viewmodel.ViewModelProviderImpl_androidKt.createViewModel(ViewModelProviderImpl.android.kt:34)
  androidx.lifecycle.viewmodel.ViewModelProviderImpl.getViewModel$lifecycle_viewmodel_release(ViewModelProviderImpl.kt:60)
  androidx.lifecycle.viewmodel.ViewModelProviderImpl.getViewModel$lifecycle_viewmodel_release$default(ViewModelProviderImpl.kt:43)
  androidx.lifecycle.ViewModelProvider.get(ViewModelProvider.android.kt:92)
  androidx.lifecycle.ViewModelLazy.getValue(ViewModelLazy.kt:52)
  androidx.lifecycle.ViewModelLazy.getValue(ViewModelLazy.kt:35)
  expo.modules.devlauncher.launcher.errors.DevLauncherErrorActivity.getViewModel(DevLauncherErrorActivity.kt:17)
  expo.modules.devlauncher.launcher.errors.DevLauncherErrorActivity.onCreate(DevLauncherErrorActivity.kt:39)
  android.app.Activity.performCreate(Activity.java:7183)
  android.app.Instrumentation.callActivityOnCreate(Instrumentation.java:1220)
  android.app.ActivityThread.performLaunchActivity(ActivityThread.java:2910)
  android.app.ActivityThread.handleLaunchActivity(ActivityThread.java:3032)
  android.app.ActivityThread.-wrap11(Unknown Source:0)
  android.app.ActivityThread$H.handleMessage(ActivityThread.java:1696)
  android.os.Handler.dispatchMessage(Handler.java:105)
  android.os.Looper.loop(Looper.java:164)
  android.app.ActivityThread.main(ActivityThread.java:6944)
  java.lang.reflect.Method.invoke(Native Method)
  com.android.internal.os.Zygote$MethodAndArgsCaller.run(Zygote.java:327)
  com.android.internal.os.ZygoteInit.main(ZygoteInit.java:1374)
