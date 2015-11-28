# Angular-Animations-ng-view
This module allows us to create beautiful looking applications. Today we’ll be looking at how to animate ng-view.
What We’ll Be Building

Let’s say we have a single page application where we want to animate the page change. Clicking on a link will slide one view out and another into view.

We’ll use:

ngRoute for our page routing
ngAnimate to create our page animations
CSS Animations will be applied to the pages
Each of our pages will have different animations when leaving or entering the view
How Does It Work?

Let’s take a look at how ngAnimate works. ngAnimate will add and remove CSS classes to different Angular directives based on if they are entering or leaving the view. For example, when we load up a site, whatever is populated in ng-view gets a .ng-enter class.

All we have to do is apply a CSS animation to that .ng-enter class and that will be applied upon entry.

ngAnimate Works On: ngRepeat, ngInclude, ngIf, ngSwitch, ngShow, ngHide, ngView, and ngClass

Definitely go through the ngAnimate documentation to see more of what ngAnimate can do. Now let’s see it in action.

Starting Our Application
Here are the files we’ll need.
- index.html
    - style.css
    - app.js
    - page-home.html
    - page-about.html
    - page-contact.html
Let’s start up an index.html file. We’ll load up AngularJS, ngRoute, and ngAnimate. Oh and don’t forget Bootstrap for stylings.

<!-- index.html -->
<!DOCTYPE html>
<html>
<head>

    <!-- CSS -->
    <!-- load bootstrap (bootswatch version) -->
    <link rel="stylesheet" href="//netdna.bootstrapcdn.com/bootswatch/3.1.1/readable/bootstrap.min.css">
    <link rel="stylesheet" href="style.css">
    
    <!-- JS -->
    <!-- load angular, ngRoute, ngAnimate -->
    <script src="http://code.angularjs.org/1.2.13/angular.js"></script>
    <script src="http://ajax.googleapis.com/ajax/libs/angularjs/1.2.13/angular-route.js"></script>
    <script src="http://ajax.googleapis.com/ajax/libs/angularjs/1.2.13/angular-animate.js"></script>
    <script src="app.js"></script>

</head>

<!-- apply our angular app -->
<body ng-app="animateApp">

    <!-- inject our views using ng-view -->
    <!-- each angular controller applies a different class here -->
    <div class="page {{ pageClass }}" ng-view></div>
        
</body>
</html>

Our Angular Application app.js

Now we will create our Angular application with routing so that we can change pages without a page refresh. For more information on Angular routing, check out our other tutorial: Single Page Apps with AngularJS Routing and Templating.

// app.js

// define our application and pull in ngRoute and ngAnimate
var animateApp = angular.module('animateApp', ['ngRoute', 'ngAnimate']);

// ROUTING ===============================================
// set our routing for this application
// each route will pull in a different controller
animateApp.config(function($routeProvider) {

    $routeProvider

        // home page
        .when('/', {
            templateUrl: 'page-home.html',
            controller: 'mainController'
        })

        // about page
        .when('/about', {
            templateUrl: 'page-about.html',
            controller: 'aboutController'
        })

        // contact page
        .when('/contact', {
            templateUrl: 'page-contact.html',
            controller: 'contactController'
        });

});


// CONTROLLERS ============================================
// home page controller
animateApp.controller('mainController', function($scope) {
    $scope.pageClass = 'page-home';
});

// about page controller
animateApp.controller('aboutController', function($scope) {
    $scope.pageClass = 'page-about';
});

// contact page controller
animateApp.controller('contactController', function($scope) {
    $scope.pageClass = 'page-contact';
});
We have now created our application, created our routing, and created our controllers.

Each controller will have its own pageClass variable. This is applied to our ng-view in the index.html file so that we have a different class for each page. With a different class for each page, we will be able to apply different animations to each specific page.

Views page-home.html, page-about.html, page-contact.html

These will be clean and straightforward. We just need some text for each and the links to the other pages.

<!-- page-home.html -->
<h1>Angular Animations Shenanigans</h1>
<h2>Home</h2>

<a href="#about" class="btn btn-success btn-lg">About</a>
<a href="#contact" class="btn btn-danger btn-lg">Contact</a>
 <!-- page-about.html -->
<h1>Animating Pages Is Fun</h1>
<h2>About</h2>

<a href="#" class="btn btn-primary btn-lg">Home</a>
<a href="#contact" class="btn btn-danger btn-lg">Contact</a>
 <!-- page-contact.html -->
<h1>Tons of Animations</h1> 
<h2>Contact</h2>

<a href="#" class="btn btn-primary btn-lg">Home</a>
<a href="#about" class="btn btn-success btn-lg">About</a>
We now have our pages and they will be injected into our main index.html file with the power of Angular routing.

Now all the boring stuff is done. Our app should work and be able to change pages nicely. Now let’s get to the part we actually wanted, the animations!

Animating Our App style.css

All of the animations we will create will be with CSS. This is great because all we had to do was add ngAnimate and now, without any changes to our Angular code, we can apply CSS animations.

Two classes that ngAnimate adds to our ng-view are .ng-enter and .ng-leave. You can guess what each of those are used for.

Base Styles

Here we’ll add some base styles so that our app doesn’t look too boring.


/* make our pages be full width and full height */
/* positioned absolutely so that the pages can overlap each other as they enter and leave */
.page        {
    bottom:0; 
    padding-top:200px;
    position:absolute; 
    text-align:center;
    top:0;  
    width:100%; 
}

.page h1    { font-size:60px; }
.page a     { margin-top:50px; }

/* PAGES (specific colors for each page)
============================================================================= */
.page-home      { background:#00D0BC; color:#00907c; }
.page-about     { background:#E59400; color:#a55400; }
.page-contact   { background:#ffa6bb; color:#9e0000; }
With that, we have base styles for all three pages. As we click through our app, we can see those applied with the colors and spacing.

CSS Animations

Let’s define our animations and then we’ll look at how we can apply them to each of the pages as they enter and leave.

Vendor Prefixes: We will be using the default CSS attributes without the vendor prefixes. The full code will have all the vendor prefixes.

Let’s make 6 different animations. Each page will have their very own ng-enter and ng-leave animation.

/* style.css */
...

/* ANIMATIONS
============================================================================= */

/* leaving animations ----------------------------------------- */
/* rotate and fall */
@keyframes rotateFall {
    0%      { transform: rotateZ(0deg); }
    20%     { transform: rotateZ(10deg); animation-timing-function: ease-out; }
    40%     { transform: rotateZ(17deg); }
    60%     { transform: rotateZ(16deg); }
    100%    { transform: translateY(100%) rotateZ(17deg); }
}

/* slide in from the bottom */
@keyframes slideOutLeft {
    to      { transform: translateX(-100%); }
}

/* rotate out newspaper */
@keyframes rotateOutNewspaper {
    to      { transform: translateZ(-3000px) rotateZ(360deg); opacity: 0; }
}

/* entering animations --------------------------------------- */
/* scale up */
@keyframes scaleUp {
    from    { opacity: 0.3; -webkit-transform: scale(0.8); }
}

/* slide in from the right */
@keyframes slideInRight {
    from    { transform:translateX(100%); }
    to      { transform: translateX(0); }
}

/* slide in from the bottom */
@keyframes slideInUp {
    from    { transform:translateY(100%); }
    to      { transform: translateY(0); }
}
With these animations all made, we will now apply them to our pages.

Entering and Leaving Animations

To apply our animations to our pages, we will just apply these animations to .ng-enter or .ng-leave.

/* style.css */
...

    .ng-enter           { animation: scaleUp 0.5s both ease-in; z-index: 8888; }
    .ng-leave           { animation: slideOutLeft 0.5s both ease-in; z-index: 9999; }

...
Now our application will animate just like that. Pages will slide out to the left when leaving and scale up when entering. We also added z-index so that the page leaving would be placed above the one entering.

Let’s look at creating page specific animations.

Page Specific Animations

We created separate Angular controllers for each of the pages. Inside of these controllers we added a pageClass and applied that to our <div ng-view>. We’ll use these classes to call out a page specifically.

Instead of the .ng-enter and .ng-leave code above, let’s make them page specific.

...

    .ng-enter       { z-index: 8888; }
    .ng-leave       { z-index: 9999; }

    /* page specific animations ------------------------ */

    /* home -------------------------- */
    .page-home.ng-enter         { animation: scaleUp 0.5s both ease-in; }
    .page-home.ng-leave         { transform-origin: 0% 0%; animation: rotateFall 1s both ease-in; }

    /* about ------------------------ */
    .page-about.ng-enter        { animation:slideInRight 0.5s both ease-in; }
    .page-about.ng-leave        { animation:slideOutLeft 0.5s both ease-in; }

    /* contact ---------------------- */
    .page-contact.ng-leave      { transform-origin: 50% 50%; animation: rotateOutNewspaper .5s both ease-in; }
    .page-contact.ng-enter      { animation:slideInUp 0.5s both ease-in; }

...
Now each page will have its own unique enter and leave animation!

Conclusion

It’s fairly easy to add animations to your Angular application. All you have to do is load up ngAnimate and create your CSS animations. Hopefully this has helped you see a couple cool things you can animate when using ng-view.

We’ll be creating other articles on how to animate with ngRepeat, ngSwitch, and all of its brothers.
