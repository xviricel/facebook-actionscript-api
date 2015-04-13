## Part Two: Desktop Session ##

Now that you’ve got your settings in Facebook all worked out let’s start a Flex project and get it to connect to the Server.  We’re going to do this with a desktop connection so that we can run it locally from FlexBuilder and won’t have to publish it to our site just yet.  I’m doing this example using FlexBuilder 3 but you should be able to follow along if you are using a different setup.

First create a new Flex project and drop in the facebook\_as3\_api.swc.

To start out our Application needs  to look something like the following.  When our application is ready we’ll create a facebook instance, listen for it to become ready, and fire up a desktop session passing in our api\_key and secret.  Additionally we need to have a button that we can click after we’ve logged into facebook.  The button is only visible if the Facebook session type is Desktop, and only if the facebook hasn’t connected yet.  When you run this you’ll be taken to a login prompt in your browser.  After you login click the button which calls the validateDesktopSesion() function.  Once the session is validated and connected onFacebookReady is called.

```
<?xml version="1.0" encoding="utf-8"?>
<mx:Application 
   xmlns:mx="http://www.adobe.com/2006/mxml" 
   layout="absolute"
   creationComplete="init();">
	
   <mx:Script>
      <![CDATA[
         import com.pbking.facebook.FacebookSessionType;
         import com.pbking.facebook.Facebook;

         [Bindable]
         public var fBook:Facebook;
			
         private function init():void
         {
            //create our facebook instance
            fBook = new Facebook();
            fBook.addEventListener(Event.COMPLETE, onFacebookReady);
			
            //start up a desktop instance
            fBook.startDesktopSession("YOUR_API_KEY", "YOUR_SECRET");
         }

         private function onFacebookReady(event:Event):void
         {
         }
      ]]>
   </mx:Script>
	
   <mx:Button x="10" y="10" 
      label="click once you have logged in to facebook"
      visible="{fBook.sessionType == FacebookSessionType.DESKTOP &amp;&amp; !fBook.isConnected}"
      click="fBook.validateDesktopSession();"/>
	
</mx:Application>
```

Now that you have a session starting it’s time to do something with it.  Let’s flesh out the onFacebookReady function.  Here we’re checking to see if our session has started correctly.  If it has we’re going to call the users.getInfo method to get the name of the logged in user (you!).

```
private function onFacebookReady(event:Event):void
{
   if(fBook.isConnected)
   {
      fBook.users.getInfo([fBook.user.uid], [UserFields.name], onGotName);
   }
   else
   {
      Alert.show("Facebook Connection Failed!");
   }
}
```

The result of that call is going to be sent to the onGotName function where we can toss up an alert message with the name of the logged in user!

```
private function onGotName(e:Event):void
{
   var d:GetUserInfoDelegate = e.target as GetUserInfoDelegate;
   if(d.success && d.users.length > 0)
   {
      var helloUser:FacebookUser = d.users[0];
      //note that because we are getting the info from fBook.user we could
      //instead use fBook.user.name rather than getting it from the delegate
      Alert.show("Hello World " + helloUser.name);
   }
}
```

And that’s it!  You’re now successfully making calls to the Facebook Server from within a Flex application!


[Step 3 - Widget Session](WidgetSession.md)