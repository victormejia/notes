## 5 Service Flavors

  * provider: configurable provider object. The provider knows how to create the service.
  * factory : internally calls the provider function.
  * service : wrapper around the factory function, which then calls the provider function
  * value : thin wrapper around factory
  * constant : doesn't call factory/provider function
  

## ```$provide```

Objects that know how to create injectable services are known as ```providers```.
Built-in service, has several methods to register components with the injector. 
It creates a ```provider``` which contains a function to create a ```service```.



### ```$provide.provider()```

```JavaScript
$provide.provider('books', function () {
  this.$get = function () {
    var appName = 'Book Logger';
    return {
      appName: appName;
    };
  }
});
```

This creates a ```booksProvider```. ```$get``` gets called to create a service.


```JavaScript

(function () {

  var app = angular.module('app', []);
  
  app.provider('books', function () {
    this.$get = function () {
      
      var appName = 'Book Logger';
      var appDesc = 'Track which books you want to read'
      
      var version = '1.0';
      
      if (includeVersionInTitle) {
        appName += ' ' + version;
      }
      
      return {
        appName: appName,
        appDesc: appDesc
      };
      
    };
    
    var includeVersionInTitle = false;
    this.setIncludeVersionInTitle = function (value) {
      includeVersionInTitle = value;
    }
    
  });
  
  app.config(function (booksProvider) {
    booksProvider.setIncludeVersionInTitle(true);
  })'

})()

```

## ```$provide.factory()```

Use it when additional config is unncessary. Internally, it calls the provider
function to register a factory function that will return a service instance. The factory
function is set as the ```$get``` value.
```JavaScript
(function () {

  angular.module('app')
    .factory('dataService', dataService);
    
  function dataService() {
    return {
      getAllBooks: getAllBooks,
      getAllReaders: getAllReaders
    }
  }

}());

// how to use it

function BooksController($scope, dataService) {
  $scope.allBooks = dataService.getAllBooks
}
```

## ```$provide.service()```
Calls factory function, which calls provider funciton. Treats function as a constructor.
Executes constructor function with ```$injector.instantiate```, the ```new``` operator.
```JavaScript
(function () {
  
  angular.module('app')
    .service('logger', BookAppLogger);
  
  function LoggerBase() {}
  
  LoggerBase.prototype.output = function (message) {
    console.log('LoggerBase: ' + message);
  }l
  
  function BookAppLogger() {
    LoggerBase.call(this);
    
    this.logBook = function (book) {
      console.log('Book: ' + book.title);
    }
  }
  
  BookAppLogger.prototype = Object.create(LoggerBase.protytype);
  
}());
```


  
