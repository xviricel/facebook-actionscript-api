## Part Three: Embedded application (WidgetSession) ##

Using the Widget session on the Facebook API is not secure.  Your secret is compiled into your .swf and a determined hacker could decompile it and do bad things with it.  However, it is the simplest and fastest way to connect to Facebook from an embedded application.

There are a number of ways to do this but the easiest is to use the FBML tag 

&lt;fb:swf/&gt;

 to load your .swf into a Canvas page. (details)  First change your application settings to use FBML instead of an iFrame.

Also make sure that your callback url is pointing to the .php file that we’re about to make.

For this you’ll also have to install the Facebook PHP API.  There are lots of instructions and examples of doing this on http://developer.facebook.com.

Next create your callback file and put some FBML in it.  We’re going to let the PHP API handle the login and such and only show the .swf once the user has logged in.  My php file looks a bit like this:
```
<?php
require_once 'facebook.php';

$appapikey = 'e495d90d0f06bea9aba23e93be22722b';
$appsecret = 'MY_SUPER_SECRET_SECRET';
$facebook = new Facebook($appapikey, $appsecret);
$user_id = $facebook->require_login();

echo "<fb:swf swfsrc=’http://pbking.com/projects/facebook-api/helloWorld/helloWorld.swf’ height=”100%” width=”100%”>";
?>
```

Next we need to modify our Flex application to run as a widget. (Here I'm referring to [part 2](DesktopSession.md) of this HOWTO).  That part couldn’t be simpler.  Simply change the startDesktopSession to startWidgetSession and pass in your flashVars.  (Since this is a Flex example the flashVars are gotten from the Application instance.)  The API will handle pulling the necessary bits from the flashVars passed into the .swf by the fb:swf tag.  You can also get rid of the button from the desktop example since there isn’t a need to login again.
```
private function init():void
{
   //create our facebook instance
   fBook = new Facebook();
   fBook.addEventListener(Event.COMPLETE, onFacebookReady);
			
   //start up a widget instance
   var flashVars:Object = Application.application.parameters;
   fBook.startDesktopSession(flashVars, "YOUR_API_KEY", "YOUR_SECRET");
}
```
Now just compile your .swf and put it on our server.  Make sure your 

&lt;fb:swf&gt;

 tag correctly points to it (it must be an ABSOLUTE path!).  Then just browse to your application on facebook (for example http://apps.facebook.com/ashelloWorld) and viola!  You’re app is on Facebook!

[Step 4 - !JavaScript Bridge Session](JavaScriptBridgeSession.md)