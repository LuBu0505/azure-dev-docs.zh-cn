---
author: alexeystrakh
ms.author: alstrakh
ms.date: 07/27/2020
ms.service: mobile-services
ms.topic: include
ms.openlocfilehash: eefda17bfa94261984dfbd86d36bd39a32f7d0ea
ms.sourcegitcommit: cf23d382eee2431a3958b1c87c897b270587bde0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/29/2020
ms.locfileid: "87401377"
---
### <a name="configure-required-ios-packages"></a>配置所需的 iOS 包

生成应用时，包会[自动进行链接](https://github.com/react-native-community/cli/blob/master/docs/autolinking.md)。 你只需安装本机 Pod 即可：

```bash
npx pod-install
```

### <a name="configure-infoplist-and-entitlementsplist"></a>配置 Info.plist 和 Entitlements.plist

1. 进入“PushDemo/ios”文件夹，打开“PushDemo.xcworkspace”工作区，选择最上面的项目“PushDemo”，然后选择“签名和功能”选项卡。

1. 更新捆绑标识符，使其与预置描述文件中使用的值匹配。

1. 使用“+”按钮添加两个新功能：

    - 后台模式功能和时钟周期远程通知。
    - 推送通知功能

### <a name="handle-push-notifications-for-ios"></a>处理 iOS 的推送通知

1. 打开“AppDelegate”并添加以下导入：

    ```objective-c
    #import <UserNotifications/UNUserNotificationCenter.h>
    ```

1. 通过添加 `UNUserNotificationCenterDelegate`，更新“AppDelegate”所支持的协议列表：

    ```objective-c
    @interface AppDelegate : UIResponder <UIApplicationDelegate, RCTBridgeDelegate, UNUserNotificationCenterDelegate>
    ```

1. 打开“AppDelegate.m”并配置所有必需的 iOS 回调：

    ```objective-c
    #import <UserNotifications/UserNotifications.h>
    #import <RNCPushNotificationIOS.h>

    ...

    // Required to register for notifications
    - (void)application:(UIApplication *)application didRegisterUserNotificationSettings:(UIUserNotificationSettings *)notificationSettings
    {
     [RNCPushNotificationIOS didRegisterUserNotificationSettings:notificationSettings];
    }

    // Required for the register event.
    - (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken
    {
     [RNCPushNotificationIOS didRegisterForRemoteNotificationsWithDeviceToken:deviceToken];
    }

    // Required for the notification event. You must call the completion handler after handling the remote notification.
    - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo
    fetchCompletionHandler:(void (^)(UIBackgroundFetchResult))completionHandler
    {
      [RNCPushNotificationIOS didReceiveRemoteNotification:userInfo fetchCompletionHandler:completionHandler];
    }

    // Required for the registrationError event.
    - (void)application:(UIApplication *)application didFailToRegisterForRemoteNotificationsWithError:(NSError *)error
    {
     [RNCPushNotificationIOS didFailToRegisterForRemoteNotificationsWithError:error];
    }

    // IOS 10+ Required for localNotification event
    - (void)userNotificationCenter:(UNUserNotificationCenter *)center
    didReceiveNotificationResponse:(UNNotificationResponse *)response
             withCompletionHandler:(void (^)(void))completionHandler
    {
      [RNCPushNotificationIOS didReceiveNotificationResponse:response];
      completionHandler();
    }

    // IOS 4-10 Required for the localNotification event.
    - (void)application:(UIApplication *)application didReceiveLocalNotification:(UILocalNotification *)notification
    {
     [RNCPushNotificationIOS didReceiveLocalNotification:notification];
    }

    //Called when a notification is delivered to a foreground app.
    -(void)userNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler
    {
      completionHandler(UNAuthorizationOptionSound | UNAuthorizationOptionAlert | UNAuthorizationOptionBadge);
    }
    ```
