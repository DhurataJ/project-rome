---
title: include file
description: include file
ms.topic: include
ms.assetid: 
---

### Create an instance of the platform

To get started simply instantiate the platform.

`MCDConnectedDevicesPlatform* platform = [MCDConnectedDevicesPlatform new];`

### Subscribe to MCDConnectedDevicesAccountManager

The platform requires an authenticated user to access the platform.  You'll need to subscribe to **MCDConnectedDevicesAccountManager** events to ensure a valid account is being used.

```ObjectiveC
[MCDConnectedDevicesPlatform* platform.accountManager.accessTokenRequested
     subscribe:^(MCDConnectedDevicesAccountManager* _Nonnull manager __unused,
                 MCDConnectedDevicesAccessTokenRequestedEventArgs* _Nonnull request __unused) {

                    // Get access token

                 }
```

```ObjectiveC
[MCDConnectedDevicesPlatform* platform.platform.accountManager.accessTokenInvalidated
     subscribe:^(MCDConnectedDevicesAccountManager* _Nonnull manager __unused,
                 MCDConnectedDevicesAccessTokenInvalidatedEventArgs* _Nonnull request) {

                      // Refresh and renew existing access token

                 }
```

### Subscribe to MCDConnectedDevicesNotificationRegistrationManager

Similarly, the platform uses notifications to deliver commands between devices.  Therefore, you must subscribe to the **MCDConnectedDevicesNotificationRegistrationManager** events to ensure the cloud registration states are valid for the account being used.  Verify the the state using **MCDConnectedDevicesNotificationRegistrationState**

```ObjectiveC
[MCDConnectedDevicesPlatform* platform.notificationRegistrationManager.notificationRegistrationStateChanged
     subscribe:^(MCDConnectedDevicesNotificationRegistrationManager* manager __unused,
                 MCDConnectedDevicesNotificationRegistrationStateChangedEventArgs* args __unused) {

                     // Check state using MCDConnectedDevicesNotificationRegistrationState enum

                 }

```

### Start the platform
Now that the platform is initialized and event handlers are in place, you are ready to start discovering remote system devices.  

`[MCDConnectedDevicesPlatform* platform start];`

### Retrieve user accounts known to the app

It is important to ensure that the list of user accounts known to the app are properly synchronized with the **MCDConnectedDevicesAccountManager**.

Use **MCDConnectedDevicesAccountManager.addAccountAsync** to add a new user account.

```ObjectiveC
[MCDConnectedDevicesPlatform* platform.accountManager
     addAccountAsync:self.mcdAccount
     callback:^(MCDConnectedDevicesAddAccountResult* _Nonnull result, NSError* _Nullable error) {

     // Check state using **MCDConnectedDevicesAccountAddedStatus** enum

     }
```

To remove an invalid account you can use **MCDConnectedDevicesAccountManager.removeAccountAsync**

```ObjectiveC
 [MCDConnectedDevicesPlatform* platform.accountManager
     removeAccountAsync:existingAccount
     callback:^(MCDConnectedDevicesRemoveAccountResult* _Nonnull result __unused, NSError* _Nullable error) {

                    // Remove invalid user account

     }
```
