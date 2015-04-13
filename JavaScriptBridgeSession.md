## Step 4: JavaScript Bridge Session ##

I mentioned earlier that using the WidgetSession was not secure because the secret was compiled into the .swf.  Facebook has created a secure JavaScript API that does not require us to compile the secret into our .swf.  Unfortunately (for now) we have to use this API as a bridge for our own communications.  This results in slightly slower response and somewhat limited functionality (we can’t upload pictures with the JS API).  But in my opinion it’s a descent (temporary) price to pay for a secure application.

For this example I am using version 0.9.1 of the API.  Make sure you are using the most up-to-date version.

To get this started, let’s again visit our Facebook application settings page.  We want our app to point to an HTML page and to use an iFrame.

On your server you need to setup some things so that the JS API works.  (Detailed instructions can be found [here](http://wiki.developers.facebook.com/index.php/JavaScript_Client_Library)) First you need to download and copy the xd\_receiver.htm to your website.  I suggest just putting it on the root (you’ll need to pass the path to it later).

Next setup your .html file.  You’ll need to include the client API.  You’ll also need some way to load your .swf.  I prefer to use [swfobject](http://code.google.com/p/swfobject/) so I am demonstrating that in my example.  (make sure to copy the swfobject.js file to your server as well).

In this file we are starting up the JavaScript API client and, similarly to the PHP client, forcing the client to be logged in before they continue.  Once that happens the nested function will run which loads the .swf into the page.  Make sure to pass the site relative location of the xd\_receiver.htm file.

There are a few things that are gathered from the api, the uid of the logged in user, the api key, the special secret used only for this session, a session key, and the time that session key expires.  Those things must be passed into the .swf for it to operate correctly.  Additionally we are also passing in the name of the JS api instance that we created.  This is so that the flash element can communicate with the JS element.

Also the id of the flash object MUST be flashContent so that the javascript can return the reply back into flash.

```
<html>
<head>
<title>ActionScript AS3 API Example</title>

<!-- include the API from facebook's server --> 
<script src="http://static.ak.facebook.com/js/api_lib/FacebookApi.debug.js" type="text/javascript"></script>
<!-- include swfobject library -->
<script type="text/javascript" src="swfobject.js"></script>
</head>
<body>
<script type="text/javascript">
//create a new instance of the JS API passing in my application key and the location of my xd_receiver.htm file
var fb_js_api = new FB.ApiClient('e495d90d0f06bea9aba23e93be22722b', '/xd_receiver.htm', null);
			
//the first call (and only in our use) is to .requireLogin.  If this session 
//hasn't been "validated" the user will either be asked to login and 
//returned, or the page could just refresh.  Once this happens the session 
//will be set and can be forwarded onto the flash app so that it can use the API.
		
fb_js_api.requireLogin(function(exception)
{
	//this function will only be called once we have a valid session
				
	//define the flashVars
	flashVars = {
		user_id: api.get_session().uid,
		api_key: api.apiKey,
		secret: api.get_session().secret,
		session_key: api.get_session().session_key,
		expires: api.get_session().expires,
		fb_js_api_name: “fb_js_api”
	};
				
	//now bring our .swf to the party.  I prefer to use swfobject to get 
        //this done but there are other solutions if you prefer.  Make sure to 
        //pass in the flashvars.
	
        swfobject.embedSWF("helloWorld.swf", "flashContent", "100%", "100%", "9.0.0", "expressInstall.swf", flashVars);
});
</script>

<div id="flashContent"></div>
</body>
</html>
```

Next we have to change our Flex application to operate with the JSBridge.  This is the easy part, just change the startWidgetSesstion to startJSBridgeSession.  When we do that we need to pass in all of the properties that we passed in as flashVars.

```
private function init():void
{
   //create our facebook instance
   fBook = new Facebook();
   fBook.addEventListener(Event.COMPLETE, onFacebookReady);
			
   //start up a JSBridge instance
   var fv:Object = Application.application.parameters;
   fBook.startJSBridgeSession(fv.api_key, fv.secret, fv.session_key, fv.expires, fv.user_id, fv.fb_js_api_name);
}
```

And that’s it!  Test your application again from your http://apps.facebook.com/appName link and you should have a more secure application.