---
title: Phonegap / Cordova + AppCache
---

> Note: since writing this, the App Cache has been depricated in favor of Service Workers - fiddler-like interseptors that can implement caching among other things. While implementation is alot more flexible, it does need a bit of bootstraping, e.g. with help of [Toolbox project](https://github.com/GoogleChrome/sw-toolbox)

Cordova is great, right? Develop once, run everywhere etc etc. But what about updates? What about being backward compatible with all the new web services you are developing on the site, packaging the app and worrying about versions… no, we web developers can’t stand it.

First order of business – how to do updates. Common misconception is that traditional binary app stores (ios/android) don’t want the apps to self-update. It’s actually not true – [you are allowed to self-update as long as the code you are updating runs in WebView](http://stackoverflow.com/a/3677759/467198). Bingo!

<!--break-->

There are solutions on the market that give you such self-update mechanism, for example [WorkLight](http://www-01.ibm.com/software/mobile-solutions/worklight/) that was recently acquired by IBM. It’s really not terribly hard to write this yourself if all you need is updates – get a zip file with all html/css/whatnot, download when application starts, extract, then actually render the UI.

However, there exists a system that is designed to do just that, and it’s [arguably](http://www.alistapart.com/articles/application-cache-is-a-douchebag/) pretty good at that. The [support](http://caniuse.com/offline-apps) for AppCache is very good both on modern desktop and mobile. The only question is how you’d use AppCache with Cordova.

Well, obviously your app can be a web site. Or web site can be the app… It’s really the same for web developers. I call most of the sites I create applications, because they are applications, from the customer point of view. Plus they can be [pinned](http://msdn.microsoft.com/en-us/library/ie/gg618532(v=vs.85).aspx) / [clipped](http://developer.apple.com/library/ios/#DOCUMENTATION/AppleApplications/Reference/SafariWebContent/ConfiguringWebApplications/ConfiguringWebApplications.html) / [made into an icon](https://developers.google.com/chrome/web-store/docs/get_started_simple)

<iframe width="560" height="315" src="https://www.youtube.com/embed/SLjuOPXjHno" frameborder="0" allowfullscreen=""></iframe>

I. Steps on your site
1. Make sure you have offline manifest on the page that will be opened by Cordova
2. Double check – is there a redirect from the URL you think you are opening and actual URL that opens up? Appcache doesn’t work with redirects, so you have to have correct URL. For example, i had http://localhost/offline redirect to http://localhost/offline/ – and it didn’t work.

II-a. Cordova iOS steps:

1. [Set up the project](http://docs.phonegap.com/en/2.0.0/guide_getting-started_ios_index.md.html).
2. Allow Cordova to go out on the internet. In your project, go to Resources/Cordova.plist . Add record to External Hosts with your server name. You can just put star (*), but if someone makes your side redirect elsewhere you are in trouble – so only use for development.
3. Make sure cordova opens links inside its own webview, instead of safari, by changing OpenAllWhitelistURLsInWebView property to Yes.

II-b. Cordova Android steps:

1. [Set up the project.](http://docs.phonegap.com/en/2.0.0/guide_getting-started_android_index.md.html)
2. Allow Cordova to go out on the internet. In your project, open res/xml/config.xml and uncomment  ```<access origin=”.*”/>``` (this is only for development, see note in step 2 for iOS)

3. Add ```<preference name=”stay-in-webview” value=”true” />``` to stay inside cordova
4. Adjust Android Cordova to enable AppCache. For some strange reason, it is not enabled by default for Android (while local storage and SQLLite are) – see setup() method in CordovaWebView.java file. It looks like in iOS UIWebView has it on by default and doesn’t let you mess with it. To fix this, just go into the only java file you should have in your app, under src/your.name.space/CordovaActivity.java (your file name could be different) and adjust the onCreate method to be like so:
```
public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        super.init(); // Initialize appView, since it's only initialized inside loadUrl by default
        android.webkit.WebSettings settings = super.appView.getSettings(); // get WebView settings
        String appCachePath = this.getCacheDir().getAbsolutePath(); // Set path to appcache
        settings.setAppCachePath(appCachePath);
        settings.setAllowFileAccess(true); // Let browser write files - doesn't work without this
        settings.setAppCacheEnabled(true); // Enable app cache
        super.loadUrl("file:///android_asset/www/index.html");
}
```
Update June 4 2013: Per comments, this is fixed in Phonegap 2.5, so code above in #4 is not needed anymore (i.e. leave it alone the way cordova generates it, don’t modify)

III. Finally, the Cordova app

This one is very simple, as you might imagine. In www/index.html all you need to have is:
```
<!DOCTYPE html>
<script type="text/javascript" charset="utf-8" src="cordova-2.0.0.js"></script>
<script>
    function onBodyLoad()
    {        
        document.addEventListener("deviceready", function(){
            location.href = "http://192.168.2.108/offline/";
        }, false);
    }
    </script>
<body onload="onBodyLoad()"></body>
```
Replace the URL with your local testing site that you can start/stop to test how offline is working, or with any other URL.
That’s it, start the app!


## Actually using Cordova APIs

One caveat is that at least on Apple AppStore the wrapped [site has to be more than site](http://stackoverflow.com/questions/10887397/legality-of-using-web-resources-and-appcache-inside-uiwebview-phonegap). It has to have some additional functionality that is not available via the web. And that’s where Cordova APIs come to the rescue, with access to all the interesting APIs a regular WebView/UIWebView doesn’t have access.

The first thing you have to do is detect that web site was opened from inside Cordova. It doesn’t look like there’s currently an API in cordova.js to tell us it loaded inside the Cordova web view. The only semi-working solution is to detect that onDeviceReady didn’t fire – but you have to wait to determine this fact, and that will delay your page loading in regular browser. Easier way is to pass some querystring parameter, url?fromcordova=true , and then include javascript and execute ondeviceready handler based on this parameter.

Second, cordova javascript actually has two versions – one for iOS and one for Android (and I am assuming others for other platforms). File name is same, content is different. The reason is that on iOS it’s using custom URL handler gap:// to communicate with Objective-C side. On Android, it’s overriding exec, prompt and alert methods in JavaScript and then on Java side in DroidGap class it’s intercepting these calls and actually using them to interact with JavaScript side (especially prompt, that can return data back to JavaScript). So the platform ID has to be another parameter in your URL, for example: url?fromcordova=true&platform=iOS

 Note: the above setup instructions are for Cordova 2.0.0 – in previous versions some things still had PhoneGap names.