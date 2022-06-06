# flutter_notify

## dependencies:
 - firebase_core: 
 - firebase_messaging: 
 - overlay_support: 

## Getting Started

registerNotification and For handling notification when the app is in background
but not terminated
```dart
 void registerNotification() async {
 await Firebase.initializeApp();
 _messaging = FirebaseMessaging.instance;

 FirebaseMessaging.onBackgroundMessage(_firebaseMessagingBackgroundHandler);

 NotificationSettings settings = await _messaging.requestPermission(
  alert: true,
  badge: true,
  provisional: false,
  sound: true,
 );

 if (settings.authorizationStatus == AuthorizationStatus.authorized) {
  print('User granted permission');

  FirebaseMessaging.onMessage.listen((RemoteMessage message) {
   print(
           'Message title: ${message.notification?.title}, body: ${message.notification?.body}, data: ${message.data}');

   // Parse the message received
   PushNotification notification = PushNotification(
    title: message.notification?.title,
    body: message.notification?.body,
    dataTitle: message.data['title'],
    dataBody: message.data['body'],
   );

   setState(() {
    _notificationInfo = notification;
    _totalNotifications++;
   });

   if (_notificationInfo != null) {
    // For displaying the notification as an overlay
    showSimpleNotification(
     Text(_notificationInfo!.title!),
     leading: NotificationBadge(totalNotifications: _totalNotifications),
     subtitle: Text(_notificationInfo!.body!),
     background: Colors.purpleAccent.withOpacity(5.0),
     duration: const Duration(seconds: 2),
    );
   }
  });
 } else {
  print('User declined or has not accepted permission');
 }
}

```


For handling notification when the app is in terminated state
```dart
  checkForInitialMessage() async {
 await Firebase.initializeApp();
 RemoteMessage? initialMessage =
 await FirebaseMessaging.instance.getInitialMessage();

 if (initialMessage != null) {
  PushNotification notification = PushNotification(
   title: initialMessage.notification?.title,
   body: initialMessage.notification?.body,
   dataTitle: initialMessage.data['title'],
   dataBody: initialMessage.data['body'],
  );

  setState(() {
   _notificationInfo = notification;
   _totalNotifications++;
  });
 }
}


```
