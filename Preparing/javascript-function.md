# 16、2015-06-14
* 执行一个buffer中的js代码：`M-x js-send-buffer`

# 15、2015-06-13
* JavaScript: The Good Part 中关于curry的介绍
  Functions are values, and we can manipulate function values in interesting ways.

  Curryingallows us to produce a new function by combining a function and an argument:

      var add1 = add.curry(1);
      document.writeln(add1(6)); // 7
      add1is a function that was created by passing 1 to add’s currymethod. Theadd1
      function adds 1 to its argument. JavaScript does not have a currymethod, but we
      can fix that by augmentingFunction.prototype:
      Function.method('curry', function ( ) {
          var args = arguments, that = this;
          return function ( ) {
              return that.apply(null, args.concat(arguments));

          };

      }); // Something isn't right...

  The curry method works by creating a closure that holds that original function and the arguments to curry. It returns a function that, when invoked, returns the result of calling that original function, passing it all of the arguments from the invocation of curryand the current invocation. It uses theArray concatmethod to concatenate the two arrays of arguments together.

  Unfortunately, as we saw earlier, theargumentsarray is not an array, so it does not have theconcatmethod. To work around that, we will apply the arrayslicemethod on both of theargumentsarrays. This produces arrays that behave correctly with the concat method:

      Function.method('curry', function ( ) {
          var slice = Array.prototype.slice,
              args = slice.apply(arguments),
              that = this;
          return function ( ) {
              return that.apply(null, args.concat(slice.apply(arguments)));
          };
      });

* JavaScript: The Good Part 中关于apply的介绍

  ### function.apply(thisArg,argArray)  
  The apply method invokes a function, passing in the object that will be bound to this and an optional array of arguments. The apply method is used in the apply invocation pattern (Chapter 4):

      Function.method('bind', function (that) {
          // Return a function that will call this function as
          // though it is a method of that object.
          var method = this,
              slice = Array.prototype.slice,
              args = slice.apply(arguments, [1]);
          return function ( ) {
              return method.apply(that,
                                  args.concat(slice.apply(arguments, [0])));

          };

      });
      var x = function ( ) {
          return this.value;

      }.bind({value: 666});
      alert(x( )); // 666

  ### The Apply Invocation Pattern  
  Because JavaScript is a functional object-oriented language, functions can have methods.

  Theapplymethod lets us construct an array of arguments to use to invoke a function. It also lets us choose the value of this. The applymethod takes two parameters. The first is the value that should be bound to this. The second is an array of parameters.

      // Make an array of 2 numbers and add them.
      var array = [3, 4];
      var sum = add.apply(null, array); // sum is 7
      // Make an object with a status member.
      var statusObject = {
          status: 'A-OK'

      };
      // statusObject does not inherit from Quo.prototype,
      // but we can invoke the get_status method on
      // statusObject even though statusObject does not have
      // a get_status method.
      var status = Quo.prototype.get_status.apply(statusObject);
      // status is 'A-OK'
