## Step One: Project Setup ##

The first thing you’ll have to do is to setup an application on Facebook.  To do so you have to have the [developer](http://www.facebook.com/developer) application installed.  Once installed, go to the “My Applications” section.  Click “Apply for another key”.  This will bring you to the application setup page.  (Alternately you could just edit the settings of an existing application).

When you’re editing your fields make sure to pay attention to these fields:

**Callback URL:** Make sure to put the full path to your application on YOUR server.  For example I used http://pbking.com/projects/facebook-api/helloWorld/helloWorld.html


**Canvas Page URL:** This is the URL that people will go to from within facebook to use your application.  It must be unique and can’t contain numbers, symbols or spaces.  Also, if you plan on using the JavaScript API to bridge your ActionScript calls (the most secure way to connect when running an embedded app) then you MUST select “Use iframe”.  The less secure (but easier to use) “Widget Session” can be used in either an FBML or iframe canvas.  This example uses FBML for the Widget Session


**Application Type:**  If you plan to run your application as a desktop application (VERY handy when building and testing your application locally) you must select Desktop here.  This does not prevent you from running your application as an embedded application on a Facebook Canvas page as well.


**Can your application be added on Facebook?:** I assume you would want this to be selected.  While you’re at it you probably want users (and possible pages) to be able to add your application as well.


**Post-Add & Side Nav URL:** You could do a lot more with these settings, but for our simple example just point them to the URL you defined for the Canvas Page URL.


And that’s it!  Once you’ve setup your application you’ll be returned to the main “My Applications page.  Take note of your API Key and Secret.  You’re going to need them to connect.


[Step 2 - Desktop Session](DesktopSession.md) 