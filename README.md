# ACS Push Notifications Library

Titanium CommonJS Library to register device for ACS Push Notifications.  Simply add this file to your project and follow the usage instructions below.

## Motivation
Push notifications with Titanium are sometimes a bit esoteric.  With this library I'm simply trying to make it super easy to implement

## Requirements
This library support Push Notifications via ACS for iOS, Android and Blackberry.  Before you use this library, you need to:

* Make sure your Titanium App is provisioned for Cloud Services.
* Obtain your Google Cloud Messaging credentials and Apple Push Notifications Certificate as explained [here](http://docs.appcelerator.com/titanium/3.0/#!/guide/Push_Notifications)
* Request BlackBerry 10 appId [docs](https://gist.github.com/pec1985/8ad59783cd5b4adc45a2), official support in 3.3.0.GA

## Installation
You could simply grab the acspush.js file and drop it into your project folder, for Titanium Classic somewhere inside **/Resources**, and for Alloy inside the **/app/lib** folder. 

I'm also providing the module as an actual Module Package.  To use this one, create (if you don't already have it) a modules folder in the same level as your tiapp.xml.  Then copy the commonjs folder into the modules folder.

After doing this, open your tiapp.xml file, look for the modules section and add the module:

	<module platform="commonjs" version="1.0.0">com.alcoapps.acspushmod</module>
	
> NOTE: If you don't care about the module version you can simply leave out the version argument

## Usage

```js
// set android-only options
var androidOptions={
    focusAppOnPush:false,
    showAppOnTrayClick:true,
    showTrayNotification:true,
    showTrayNotificationsWhenFocused:false,
    singleCallback:true
}

// set blackberry-only options
var blackberryOptions={
    appId : "4427-7h6l37627mrr0I3956a74om7643M17l7921",
    ppgUrl : "http://cp4427.pushapi.eval.blackberry.com",
    usePublicPpg : true,
    launchApplicationOnPush : true
}

// set cross-platform event
var onReceive=function(evt){
    alert('A push notification was received!');
    console.log('A push notification was received!' + JSON.stringify(evt));
}

// set android-only event
var onLaunched=function(evt){
    alert('A push notification was received - onLaunched');
    console.log('A push notification was received!' + JSON.stringify(evt));
}

// set android-only event
var onFocused=function(evt){
    alert('A push notification was received - onFocused');
    console.log('A push notification was received!' + JSON.stringify(evt));
}

// load library
var ACSP=require('acspush');

// create instance with your own or the user's username and password
var ACSPush=new ACSP.ACSPush('your_acs_admin_uid','your_acs_admin_pwd');

// or make it as guest
//var ACSPush=new ACSP.ACSPush();

// set the channel to subscribe to
var channel='All users';

// register this device
ACSPush.registerDevice(channel,onReceive,onLaunched,onFocused,androidOptions,blackberryOptions);

// unregister this device
ACSPush.unsubscribeFromChannel(channel,token,onSuccess,onFail);
```

## Sending messages to your subscribers

### Using the ACS Dashboard

To send to your iOS or Android registered devices simply log on to your ACS Dashboard, go to the Push Notifications Tab, fill out the screen and send.

![acsiosandroid](http://s27.postimg.org/5ixtazxwz/Screen_Shot_2014_03_31_at_11_51_28_AM.png)

### Sending to Blackberry subscribers

To send to your Blackberry registered devices, you can use [this node.js script](https://github.com/pec1985/BB10-Push-Server) created by [Pedro Enrique](https://github.com/pec1985).

### Sending from other apps or creating your own console

If you wish to send Push Notifications from other apps, or as a result of an operation on your website or back-end service, you can use any of these scripts:

* PHP : [https://github.com/ricardoalcocer/acsphppushnotifications](https://github.com/ricardoalcocer/acsphppushnotifications) 
* PERL : [http://ulizama.com/2014/05/using-perl-to-send-acs-push-notifications/](http://ulizama.com/2014/05/using-perl-to-send-acs-push-notifications/)

## Credits
This module is based on code by my buddy [Pablo Rodríguez](https://github.com/pablorr18), now with some additional sugar and converted into a reusable CommonJS Module.

BlackBerry 10 support by [Hazem Khaled](http://github.com/hazemkhaled)

## License
Licensed under the terms of the [MIT License](alco.mit-license.org)
