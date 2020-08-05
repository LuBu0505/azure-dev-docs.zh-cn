---
author: alexeystrakh
ms.author: alstrakh
ms.date: 07/27/2020
ms.service: mobile-services
ms.topic: include
ms.openlocfilehash: a5872650356f8b3ae9b33381615d6128cb2d8d72
ms.sourcegitcommit: cf23d382eee2431a3958b1c87c897b270587bde0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/29/2020
ms.locfileid: "87401380"
---
### <a name="configure-required-android-packages"></a>配置所需的 Android 包

生成应用时，包会[自动进行链接](https://github.com/react-native-community/cli/blob/master/docs/autolinking.md)。 你还可以使用几个附加步骤来完成配置过程。

### <a name="configure-android-manifest"></a>配置 Android 清单

在“android/app/src/main/AndroidManifest.xml”中，验证包名称、权限和所需的服务。 请确保已注册 `RNPushNotificationPublisher` 和 `RNPushNotificationBootEventReceiver` 接收方，并且注册了 `RNPushNotificationListenerService` 服务。 通知元数据可用于自定义推送通知的外观。

  ```xml
  <manifest xmlns:android="http://schemas.android.com/apk/res/android"
      package="YOUR_PACKAGE_NAME">

        <uses-permission android:name="android.permission.INTERNET" />
        <uses-permission android:name="android.permission.WAKE_LOCK" />
        <uses-permission android:name="android.permission.VIBRATE" />
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED"/>

        <application
          android:name=".MainApplication"
          android:label="@string/app_name"
          android:usesCleartextTraffic="true"
          android:icon="@mipmap/ic_launcher"
          android:roundIcon="@mipmap/ic_launcher_round"
          android:allowBackup="false"
          android:theme="@style/AppTheme">

          <meta-data  android:name="com.dieam.reactnativepushnotification.notification_channel_name"
                      android:value="PushDemo Channel"/>
          <meta-data  android:name="com.dieam.reactnativepushnotification.notification_channel_description"
                      android:value="PushDemo Channel Description"/>
          <meta-data  android:name="com.dieam.reactnativepushnotification.notification_foreground"
                      android:value="true"/>
          <meta-data  android:name="com.dieam.reactnativepushnotification.notification_color"
                      android:resource="@android:color/white"/>

          <receiver android:name="com.dieam.reactnativepushnotification.modules.RNPushNotificationPublisher" />
          <receiver android:name="com.dieam.reactnativepushnotification.modules.RNPushNotificationBootEventReceiver">
              <intent-filter>
                  <action android:name="android.intent.action.BOOT_COMPLETED" />
              </intent-filter>
          </receiver>

          <service
              android:name="com.dieam.reactnativepushnotification.modules.RNPushNotificationListenerService"
              android:exported="false" >
              <intent-filter>
                  <action android:name="com.google.firebase.MESSAGING_EVENT" />
              </intent-filter>
          </service>

          <activity
            android:name=".MainActivity"
            android:label="@string/app_name"
            android:configChanges="keyboard|keyboardHidden|orientation|screenSize|uiMode"
            android:launchMode="singleTask"
            android:windowSoftInputMode="adjustResize">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
          </activity>
          <activity android:name="com.facebook.react.devsupport.DevSettingsActivity" />
        </application>

  </manifest>
  ```

### <a name="configure-google-services"></a>配置 Google 服务

在“android/app/build.gradle”中注册 Google 服务：

```gradle
dependencies {
  ...
  implementation 'com.google.firebase:firebase-analytics:17.3.0'
  ...
}

apply plugin: 'com.google.gms.google-services'
```

将在 FCM 安装过程中下载的“google-services.json”文件复制到项目文件夹“android/app/”中。

### <a name="handle-push-notifications-for-android"></a>处理 Android 的推送通知

你配置了现有 `RNPushNotificationListenerService` 服务来处理传入的 Android 推送通知。 此服务以前在应用程序清单中注册过。 它处理传入通知并将其代理到跨平台 React Native 部分。 无需执行其他步骤。
