# ANGULAR-JS
AngularJS is an extensible and exciting new JavaScript MVC framework developed by Google for building well-designed, structured and interactive single-page applications (SPA). It lays strong emphasis on Testing and Development best practices such as templating and declarative bi-directional data binding. 

## Noob

# Important AngularJS Components and their usage

• angular.module() defines a module

• Module.controller() defines a controller

• Module.directive() defines a directive

• Module.filter() defines a filter

• Module.service() or Module.factory() or
Module.provider() defines a service

• Module.value() defines a service from an existing
 object Module

• ng-app attribute sets the scope of a module

• ng-controller attribute applies a controller to the
 view

• $scope service passes data from controller to the
 view

• $filter service uses a filter

• ng-app attribute sets the scope of the module

# Bootstrapping AngularJS application: 

Bootstrapping in HTML:

<div ng-app=”moduleName”></div>

Manual bootstrapping:

angular.bootstrap(document,[“moduleName”])

# Expressions: 

{{ 4+5 }} -> yields 9

{{ name }} -> Binds value of name from current
scope and watches for changes to name

{{ ::name }} -> Binds value of name from current
scope and doesn’t watch for change (Added in
AngularJS 1.3)

# Module: 

Create a module named myModule1 that depends on
myModule2 and myModule2:

angular.module(“myModule1”,[“myModule2”,
“myModule2”])

Get reference to the module myModule1

angular.module(“myModule1”)

# Defining a Controller and using it:

i. With $scope:

angular.module(“myModule”).
controller(“SampleController”,
function($scope,){
 //Members to be used in view for binding
 $scope.city=”Hyderabad”;
});

In the view:
<div ng-controller=”SampleController”>
 <!-- Template of the view with binding
 expressions using members of $scope -->
 <div>{{city}}</div>
</div>

ii. Controller as syntax:
angular.module(“myModule”).
controller(“SampleController”, function(){
 var controllerObj = this;
 //Members to be used on view for binding
 controllerObj.city=”Hyderabad”;
});

In the view:
<div ng-controller=”SampleController as ctrl”>
 <div>{{ctrl.city}}</div>
</div>

# Defining a Service:

angular.module(“myModule”).

service(“sampleService”, function(){
 var svc = this;
 var cities=[“New Delhi”, “Mumbai”,
 “Kolkata”, “Chennai”];
 
 svc.addCity = function(city){
 cities.push(city);
 };
 svc.getCities = function(){
 return cities;
 }
});

The members added to instance of the service are visible to the outside world. Others are private to the service. Services are singletons, i.e. only one instance of the service is created in the lifetime of an AngularJS application.

# Factory:

angular.module(“myModule”).
factory(“sampleFactory”, function(){
 var cities = [“New Delhi”, “Mumbai”,
 “Kolkata”, “Chennai”];
 function addCity(city){cities.push(city);}
 function getCities(){return cities;}
 return{
 getCities: getCities,
 addCity:addCity
 };
});

A factory is a function that returns an object. The members that are not added to the returning object, remain private to the factory. The factory function is executed once and the result is stored. Whenever an application asks for a factory, the application returns the same object. This behavior makes the factory a singleton.

# Value:

angular.module(“myModule”).value(“sampleValue”, {
 cities : [“New Delhi”, “Mumbai”, “Kolkata”,
 “Chennai”],
 addCity: function(city){cities.push(city);},
 getCities: function(){return cities;}
});

A value is a simple JavaScript object. It is created just once, so value is also a singleton. Values can’t contain private members. All members of a value are public.

# Constant:

angular.module(“myModule”).

constant(“sampleConstant”,{pi: Math.PI});

A constant is also like a value. The difference is, a constant can be injected into config blocks, but a value cannot be injected.

# Provider:

angular.module(“myModule”).
provider(“samplePrd”, function(){
 this.initCities = function(){
 console.log(“Initializing Cities…”);
 };
 this.$get = function(){
 var cities = [“New Delhi”, “Mumbai”,
 “Kolkata”, “Chennai”];
 function addCity(city){
 cities.push(city);
 }
 function getCities(){return cities;}
 return{ getCities: getCities,addCity:addCity
 };
 }
});

A provider is a low level recipe. The $get() method of the provider is registered as a factory. Providers are available to config blocks and other providers. Once application
configuration phase is completed, access to providers is prevented. After the configuration phase, the $get() method of the providers are executed and they are available as factories.
Services, Factories and values are wrapped inside provider with $get() method returning the actual logic implemented inside the provider.

# Config block:
angular.module(“myModule”).config(function
(samplePrdProvider, sampleConstant){
 samplePrdProvider.init();
 console.log(sampleConstant.pi);
});

Config block runs as soon as a module is loaded. As the name itself suggests, the config block is used to configure the application. Services, Factories and values are not available for config block as they are not created by this time. Only providers and constants are accessible inside the config block. Config block is executed only once in the lifetime of an Angular application.

# Run block:

angular.module(“myModule”).run(function(<any
services, factories>){
 console.log(“Application is configured. Now inside run
 block”);
});
  
Run block is used to initialize certain values for further use, register global events and anything that needs to run at the beginning of the application. Run block is executed after config block, and it gets access to services, values and factories. Run block is executed only once in the lifetime of an Angular application.
  
# Filters: 
  
angular.module(“myModule”).
filter(“dollarToRupeee”, function(){
 return function(val){
 return “Rs. “ + val * 60;
 };
});
Usage:
<span>{{price | dollarToRupee}}</span>
  
Filters are used to extend the behavior of binding expressions and directives. In general, they are used to format values or to apply certain conditions. They are executed whenever the value bound in the binding expression is updated.

  
# Directives:
  
  myModule.directive(“directiveName”, function
(injectables) {
 return {
 restrict: “A”,
 template: “<div></div>”,
 templateUrl: “directive.html”,
 replace: false,
 transclude: false,
 scope: false,
  require: [“someOtherDirective”],
controller: function($scope, $element,
$attrs, $transclude, otherInjectables) { ... },
link: function postLink(scope, iElement,
 iAttrs) { ... },
priority: 0,
terminal: false,
compile: function compile(tElement, tAttrs,
 transclude) {
return {
 pre: function preLink(scope, iElement,
 iAttrs, controller) { ... },
 post: function postLink(scope,
 iElement, iAttrs, controller) { ... }
}
 }
 };
});
Directives add the capability of extending HTML. They are the
most complex and the most important part of AngularJS. A
directive is a function that returns a special object, generally
termed as Directive Definition Object. The Directive Definition
Object is composed of several options as shown in the above
snippet. Following is a brief note on them:
  
  • restrict: Used to specify how a directive can be used. Possible
values are: E (element), A (Attribute), C (Class) and M (Comment).
Default value is A
• template: HTML template to be rendered in the directive
• templateUrl: URL of the file containing HTML template of the
element
• replace: Boolean value denoting if the directive element is to
be replaced by the template. Default value is false
• transclude: Boolean value that says if the directive should
preserve the HTML specified inside directive element after
rendering. Default is false
• scope: Scope of the directive. It may be same as the scope of
surrounding element (default or when set to false), inherited
from scope of the surrounding element (set to true) or an
isolated scope (set to {})
• require: A list of directive that the current directive needs.
Current directive gets access to controller of the required
directive. An object of the controller is passed into link function
  
  of the current directive.
• controller: Controller for the directive. Can be used to
manipulate values on scope or as an API for the current
directive or a directive requiring the current directive
• priority: Sets priority of a directive. Default value is 0.
Directive with higher priority value is executed before a
directive with lower priority
• terminal: Used with priority. If set to true, it stops execution
of directives with lower priority. Default is false
• link: A function that contains core logic of the directive.
It is executed after the directive is compiled. Gets access
to scope, element on which the directive is applied (jqLite
object), attributes of the element containing the directive and
controller object. Generally used to perform DOM manipulation
and handling events
• compile: A function that runs before the directive is compiled.
Doesn’t have access to scope as the scope is not created yet.
Gets an object of the element and attributes. Used to perform
DOM of the directive before the templates are compiled and
before the directive is transcluded. It returns an object with two
link functions:
 o pre link: Similar to the link function, but it is executed
before the directive is compiled. By this time, transclusion is
applied
 o post link: Same as link function mentioned above
  
# Most used built-in directives:
 
  • ng-app: To bootstrap the application

  • ng-controller: To set a controller on a view

  • ng-view: Indicates the portion of the page to be
updated when route changes

  • ng-show / ng-hide: Shows/hides the content within
the directive based on boolean equivalent of value
assigned

  • ng-if: Places or removes the DOM elements under this directive based on boolean equivalent of value 16
assigned
  
• ng-model: Enables two-way data binding on any
input controls and sends validity of data in the input
control to the enclosing form
  
• ng-class: Provides an option to assign value of
a model to CSS, conditionally apply styles and use
multiple models for CSS declaratively
  
• ng-repeat: Loops through a list of items and copies
the HTML for every record in the collection
  
• ng-options: Used with HTML select element to
render options based on data in a collection
  
• ng-href: Assigns a model as hyperlink to an anchor
element
  
• ng-src: Assigns a model to source of an image
element
  
• ng-click: To handle click event on any element
  
• ng-change: Requires ng-model to be present
along with it. Calls the event handler or evaluates the
assigned expression when there is a change to value
of the model
  
• ng-form: Works same as HTML form and allows
nesting of forms
  
• ng-non-bindable: Prevents AngularJS from
compiling or binding the contents of the current DOM
element
  
• ng-repeat-start and ng-repeat-end: Repeats top-level
attributes
  
• ng-include: Loads a partial view
  
• ng-init: Used to evaluate an expression in the current scope
  
• ng-switch conditionally displays elements
  
• ng-cloak to prevent Angular HTML to load before
bindings are applied.
  
# AngularJS Naming Conventions
  • While naming a file say an authentication controller,
end it with the object suffix. For eg: an authentication
controller can be renamed as auth–controller.js.
Similar service can be called as auth-service.js,
directive as auth-directive.js and a filter as auth-filter.js
• Create meaningful & short lower case file names that also
reflect the folder structure. For eg: if we have a login controller
inside the login folder which is used for creating users, call it
login-create-controller.js
• Similar a testing naming convention that you could follow
is if the filename is named as login-directive.js, call its test file
counterpart as login-directive_test.js. Similarly a test file for
login-service.js can be called as login-service_test.js
Use a workflow management tool like Yeoman plugin for
Angular that automates a lot of these routines and much more
for you. Also look at ng-boilerplate to get an idea of the project
and directory structure.

# Dependency Injection:
  
  AngularJS has a built-in dependency injector that keeps track
of all components (services, values, etc.) and returns instances
of needed components using dependency injection. Angular’s
dependency injector works based on names of the components.
A simple case of dependency injection:
myModule.controller(“MyController”, function($scope,
$window, myService){
});
Here, $scope, $window and myService are passed into the
controller through dependency injection. But the above code
will break when the code is minified. Following approach solves
it:
myModule.controller(“MyController”, [“$scope”,
“$window”, “myService”,
function($scope, $window, myService){
}]);
  
 # Routes
  Routes in AngularJS application are defined using
$routeProvider. We can define a list of routes and set one of
the routes as default using otherwise() method; this route
will respond when the URL pattern doesn’t match any of the
configured patterns.
  
 # Registering routes:
  myModule.config(function($routeProvider){
 $routeProvider.when(“/home”,
 {templateUrl:”templates/home.html”,
 controller: “HomeController”})
 .when(“/details/:id”, {template:
 “templates/details.html”,
 controller:”ListController”})
 .otherwise({redirectTo: “/home”});
});
  
#  Registering services:
  Angular provides us three ways to create and register our
own Services – using Factory, Service, and Provider. They are all
used for the same purpose. Here’s the syntax for all the three:
Service: module.service( ‘serviceName’, function );
Factory: module.factory( ‘factoryName’, function );
Provider: module.provider( ‘providerName’, function );
The basic difference between a service and a factory is that
service uses the constructor function instead of returning a
factory function. This is similar to using the new operator. So
you add properties to ‘this’ and the service returns ‘this’.
With Factories, you create an object, add properties to it and
then return the same object. This is the most common way of
creating Services.
If you want to create module-wide configurable services
which can be configured before being injected inside other
components, use Provider. The provider uses the $get function
to expose its behavior and is made available via dependency
injection.
  
# Some useful utility functions 
 
  • angular.copy - Creates a deep copy of source

  • angular.extend - Copy methods and properties from one
object to another

  • angular.element - Wraps a raw DOM element or HTML string
as a jQuery element

  • angular.equals - Determines if two objects or two values are
equivalent

  • angular.forEach - Enumerate the content of a collection

  • angular.toJson - Serializes input into a JSON-formatted string

  • angular.fromJson - Deserializes a JSON string

  • angular.identity - Returns its first argument

  • angular.isArray - Determines if a reference is an Array

  • angular.isDate - Determines if a value is a date

  • angular.isDefined - Determines if a reference is defined

  • angular.isElement - Determines if a reference is a DOM
element

  • angular.isFunction - Determines if a reference is a Function

  • angular.isNumber - Determines if a reference is a Number

  • angular.isObject - Determines if a reference is an Object

  • angular.isString - Determines if a reference is a String

  • angular.isUndefined - Determines if a reference is undefined

  • angular.lowercase - Converts the specified string to lowercase

  • angular.uppercase - Converts the specified string to
uppercase
  
  # $http:
  
  $http is Angular’s wrapper around XmlHttpRequest. It provides
a set of high level APIs and a low level API to talk to REST
services. Each of the API methods return $q promise object.
Following are the APIs exposed by $http:
  
• $http.$get(url): Sends an HTTP GET request to the URL
specified

  • $http.post(url, dataToBePosted): Sends an HTTP POST
request to the URL specified

  • $http.put(url, data): Sends an HTTP PUT request to the URL
specified

  • $http.patch(url, data): Sends an HTTP PATCH request to the
URL specified

  • $http.delete(url): Sends an HTTP DELETE request to the URL
specified

  • $http(config): It is the low level API. Can be used to send
any of the above request types and we can also specify other
properties to the request. Following are the most frequently
used config options:
  
 o method: HTTP method as a string, e.g., ‘GET’, ‘POST’, ‘PUT’, etc.
 o url: Request URL
 o data: Data to be sent along with request
 o headers: Header parameters to be sent along with the
 request
 o cache: caches the response when set to true
Following is a small snippet showing usage of $http:
  
$http.get(‘/api/data’).then(function(result){
 return result.data;
 }, function(error){
 return error;
});
  
  

