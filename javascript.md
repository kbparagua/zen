# Javascript

* Use a global namespace for every app and put global variables under it.

  Example:
  ```javascript
  window.appName = window['appName'] || {};
  
  appName.global = 100;
  appName.fx = function(){};
  ```
