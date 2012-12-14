
When we built Rattata.js, we were first building an application based on it before implementing the actual underlying framework. Step by step, we could watch the application becoming alive.
It's a rough MVC architecture built in JavaScript for your upcoming web app.

Rattata.js is still under development. Feel free to contribute and share your thoughts.

## So let's start — how do I begin writing an app?
Since Rattata.js uses stealJS as resource mananager and stealJS supports folder structure templates to rapidly instantiate oftenly used folder structures, a Rattata.js template to generate the foundation file for a Rattata.js based application is part of the rep as well.

Before doing anything, be sure to set up the right folder hierachy. Rattata.js depends on the following libs you need to download before starting developing apps. They are not part of the Rattata.js download, so please download them manually:

- steal (https://github.com/jupiterjs/steal)
- jQote2 (https://github.com/aefxx/jQote2)
- jQuery (https://github.com/jquery/jquery)

Download them and create the following structure:

	/steal
		/steal.js (and other stealJS files)
	/rattata
		/rattata.js
		/plugins
		/jquery.js
		/jquery.jqote2.min.js

Now you can start by generating a foundation for your app:

0. Navigate to your steal folder using the Terminal.
1. Run **./js steal/generate/rattataApp MyFirstApp**. A folder named *MyFirstApp* will be created automatically next to */steal* and */rattata* that contains some demo controllers, models and views.
3. Open *MyFirstApp/yFirstApp.js* and start extending the foundation.

## The longer version of creating an app
Let's have a look at the manual way of instantiating a new Rattata.js based application.

**Create a new folder** for your app with the following structure:
	
```
/yourApp
    /resources
        /js (this is where external JS plugins are stored)
        /css (your css files)
        /models (your data model descriptions)
        /views (your HTML templates)
        /controllers (your magical controllers)´
```
  
**Make a .js file** in the root of your app folder and paste the following lines of code in:

```javascript
steal( 'appmvc/appmvc' ).then(function(){
    var yourApp = {
        dependencies:	['css/base'], // include the css file 'css/base.css'
        models:			['facebook'],
        views:			['welcomeView'],
        controllers:	['welcome'],
        skeleton:		'#app',
    }
    app.build(yourApp);
});
```
	
This acts as the bootstrap file. Here, you have specified the basic setting of your app: you have one model, one view and one controller and all of the HTML goes into the #app div of your DOM.

Next, **make a .html file** in your app root folder and paste something similiar to the following lines of code:
	
```html
<!doctype html>
<html>
  <head>
	</head>
	<body>
		<h1>This is a Rattata.js based application</h1>
        <div id="app"></div>
		<script type="text/javascript" src="../steal/steal.js?appmvcdemo/appmvcdemo.js">
        </script>
	</body>
</html>
```

Now let's **construct a controller**. Just create a file *welcome.js* in the */resources/controllers* folder and paste the following lines:
	
```javascript
app.controllers.extend('welcome',{
   isMainController: true,
      
      main: function(){
         app.views.welcomeView.render({}).show();
      },
      
      'click reload': function(){
         var myFBname = prompt("What's your nickname on facebook?");
         app.models.facebook.getFullname({id: myFBname}, function(data) { 
            app.views.welcomeView.render(data).show();
         });
      }	
   });
```

The main() function will be executed automatically as soon as the app is being executed, since we've marked the controller as main controller. If we click on a UI element with a class called 'uiReload' ('ui' is the UI element prefix to separate CSS classes from UI classes), you get prompted for your facebook nickname, which will be used to fetch your fullname.

Let's build **our model** 'facebook'. Make a file 'facebook.js' in your '/resources/models' folder:
	
```javascript
app.models.extend('facebook',{
   'getFullname': ['GET', 'http://graph.facebook.com/{id}', function(result){
      result.fullname = result.first_name + result.last_name;
      return result;
   }]
});
```
	
Here, we're creating a new model named 'facebook' and specify one method called 'getFullname' which gets its data via GET from the social graph using the parameter {id} given by our controller. The optional function defines a data enrichment operation to post-process the AJAX data result. In this case, we are synthetically creating a new property 'fullname' using the first_ and last_name properties.

And now, we're creating **the view**: Make a file 'welcomeView.html' in the right folder and paste the following lines:
	
```html
   <h2>Hello World<*= (this.fullname!=null) ? ', '+this.fullname : '' *></h2>
	<p>Click on the button below to get a personalized Welcome message.</p>
	<button class="uiReload">Give me a personalized Welcome message, please!</button>´
```

You see: Rattata.js views *do* contain logic. All data being passed to a view is available via the *this* reference.

**Run** the app from file:/// since we're AJAX requesting to a foreign URL.

That's it. Nice, huh?

## License
rattata.js is released under the MIT license (http://en.wikipedia.org/wiki/MIT_License).

## What libraries does Rattata depend on?
Rattata.js uses *jQuery* for coding acceleration, *stealJS* for resource management & code compression and *jQote2* as templating engine.
