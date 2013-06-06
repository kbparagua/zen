# Javascript

* Use a global namespace for every app and put global variables under it.
  ```javascript
  window.appName = window['appName'] || {};
  
  appName.global = 100;
  appName.fx = function(){};
  ```

* Prepend jQuery objects with `$`.
  ```js
  var $body = $('body');
  ```

* Looping through items:

  ```js
    var items = [1, 2 , 3, 4, 5, 6, 7, 8, 9, 10];
    
    // Bad
    // Always computing length on each condition check.
    for (var i = 0; i < items.length; i++){
      var item = items[i];
      console.log(item);
    }
    
    
    // Good
    // No length computation during condition check.
    for (var i = 0, length = items.length; i < length; i++){
      var item = items[i];
      console.log(item);
    }
    
    
    // Better
    // Do not use this if items may contain falsy values (null, undefined, '', 0, false)
    for (var i = 0, item; item = items[i]; i++){
      console.log(item);
    }
  ```
