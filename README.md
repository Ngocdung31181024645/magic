About
Magic is designed to provide magic objects that have global Getters and Setters, Designed to let you write even better Promise implementations.

Magic Objects are not usable as traditional objects, but meant to be used as a tool to bridge into the gap of 'catch all' getter/setters that are not avail to V8 JS Land by default.

Magic is designed to implement similar functionality as PHP's __set and __get methods.

MagicObjects by themselves are useless, its up to you as a developer to implement them in a useful way, primarily for Promise based implementations.

Requirements
This functionality is done through V8 C++ calls and MUST BE COMPILED. If you compiled Node.JS from source, you likely already have all dependencies.

To compile the needed C++ Libraries, simply go to the folder you have installed Magic and type make makelibs. If you are a developer working on a fork of Magic, then simply type make to not have build files automatically cleaned up, speeding up subsequent compiles.

Install
Magic is ready to be installed from NPM, but may also be manually added to your project with git submodules or a clone. First CD to your project root. Ensure a directory named node_modules exists.

Install with NPM:

npm install magic
Install with GIT:

As a submodule:
git submodule add git://github.com/aikar/magic node_modules/magic
git submodule init
git submodule update
As a plain clone:
git clone git://github.com/aikar/magic node_modules/magic
After installing from GIT, you must compile the libraries:

make makelibs
NPM will automatically run that command for you.

Usage
Magic exports a single function, in which you can call passing a getter and optional setter and it will return a MagicObject. Any time a property is set or get on this object, the callback will fire instead.

  var magic = require('magic');
  magic(function (name) {
      console.log('GET', name);
  }, function (name, value) {
      console.log('SET', name, '=', value);
  });
That's all there is to it. An example of chaining:

  function configChain() {
    var requestChain = [];
    function getter(name) {
      requestChain.push(name);
      return _chain();
    }
    function setter(name, value) {
      getter(name);// add final key to chain
      var key = requestChain.join('.');
      console.log('Setting', key, 'to', value);
    }
    function _chain() {
      return magic(getter, setter);
    }
    return _chain();
  }
  
  configChain().foo.bar.baz.hello.world = 42;
This will cause Setting foo.bar.baz.hello.world to 42 to print to screen.

Important Note
This Object CAN NOT be used like a normal object!! If you try to set a property on the object inside of the setter, you will throw an error.

You must wrap the usage of these methods in a closure in order to store data.

Tests
Magic uses the Vows test suite. please refer to vowsjs.org for installation. To run the tests, simply type ./runTest.js from the node_modules/magic folder.

Contributing
If you would like to contribute, please fork the project on github, make changes and submit pull requests back to me.

Please follow my same coding styles (spaces, no tabs!) and add new test for new functionality.

Harmony Proxies
I've found out afterwards that this functionality is actually brought in the spec for ES5 Harmony Proxies. I will not be conforming to the Harmony Proxies spec as a module has already been written to do that.

If your wanting the full Harmony Proxy spec, then use this lib:

https://github.com/samshull/node-proxy

Magic will be a simpler and more basic implementation, however I will expand magic to do things outside of the Harmony Proxy spec too.

If you only need the basic concept of getter/setters, then Magic will do the job. If you want the extra features of Harmony Proxies, then use the above, however some of the features will likely end up in Magic too, just I want Magic to be free to expand to what ever will make it better, including features NOT part of the Harmony Proxy spec.
