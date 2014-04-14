# Node.js Style Guide

This is a guide for writing consistent and aesthetically pleasing node.js code.
It is inspired by what is popular within the community, and flavored with some
personal opinions.

This guide was created by [Felix Geisendörfer](http://felixge.de/) and is
licensed under the [CC BY-SA 3.0](http://creativecommons.org/licenses/by-sa/3.0/)
license. You are encouraged to fork this repository and make adjustments
according to your preferences.

![Creative Commons License](http://i.creativecommons.org/l/by-sa/3.0/88x31.png)

[Original repo](https://github.com/felixge/node-style-guide)

This has been modified for use by LN eCommerce teams.

## 2 Spaces for indention

Use 2 spaces for indenting your code and swear an oath to never mix tabs and
spaces - a special kind of hell is awaiting you otherwise.

## No trailing whitespace

Just like you brush your teeth after every meal, you clean up any trailing
whitespace in your JS files before committing. Otherwise the rotten smell of
careless neglect will eventually drive away contributors and/or co-workers.

## Use Semicolons

According to [scientific research][hnsemicolons], the usage of semicolons is
a core values of our community. Consider the points of [the opposition][], but
be a traditionalist when it comes to abusing error correction mechanisms for
cheap syntactic pleasures.

[the opposition]: http://blog.izs.me/post/2353458699/an-open-letter-to-javascript-leaders-regarding
[hnsemicolons]: http://news.ycombinator.com/item?id=1547647

## 100 characters per line

Soft-limit your node.js code to 100 characters per line, with a hard limit of 120 characters.  Jade template files may
be 120 characters per line.

Some may prefer 80-column lines, but with the emergence of wide-screen monitors and disuse of printed page, the effort
involved to foist that on new hires is not worth it.  In fact, it tends to make code less easy to digest, because it
makes the code take up more lines and vertical space.  So we enforce 100-column lines instead for code.

That said, you should make every effort to avoid signficant levels of indentation.

## Use single quotes

Use single quotes, unless you are writing JSON.

*Right:*

```js
var foo = 'bar';
```

*Wrong:*

```js
var foo = "bar";
```

## Opening braces go on the same line

Your opening braces go on the same line as the statement.

*Right:*

```js
if (true) {
  console.log('winning');
}
```

*Wrong:*

```js
if (true)
{
  console.log('losing');
}
```

Also, notice the use of whitespace before and after the `if` condition.

## if/else statements must have closing brace on same line as else

*Right:*

```js
if (true) {
  console.log('winning');
} else {
  console.log('bi-winning');
}
```

*Wrong:*

```js
if (true) {
  console.log('losing');
}
else {
  console.log('bi-losing');
}
```

## Use lodash library _.each instead of for statement

It is a standard of ours to use lodash library `_.each` statement instead of writing `for` statements.  `_.each`
gives you the value of each array element, or the value and key of each object.  It avoids having to use `var` to
declare loop variables, and syntax warnings about having the same loop variable in two different `for` statements
within single code block.

This also provides protection from the unexpected side effects of [hoisting](hoisting), which effects both `if` and
`for` statements.

[hoisting]: http://www.adequatelygood.com/JavaScript-Scoping-and-Hoisting.html

Be sure to name your value and key (if applicable) parameters meaningfully (don't use names like 'key' and 'value').

`for` statements may be used to iterate through characters in a string or Buffer, if you are writing some low-level
utility function around parsing that must be fast, but this is discouraged.

*Right:*

```js
var _ = require('lodash');

var fruits = ['apple', 'banana', 'cranberry'];

_.each(fruits, function(inFruit) {
  console.log('saw fruit: ' + inFruit);
});

var veggies = {
  'bell pepper': 'green',
  onion: 'yellow'
};

_.each(veggies, function(color, veggie) {
  console.log('saw veggie: ' + veggie + ' with color: ' + color);
});
```

*Wrong:*

```js
var fruits = ['apple', 'banana', 'cranberry'];

for (var i = 0; i < fruits.length; i++) {
  console.log('saw fruit: ' + fruits[i]);
}

var veggies = {
  'bell pepper': 'green',
  onion: 'yellow'
};

for (var veggie in veggies) {
  var color = veggies[veggie];
  console.log('saw veggie: ' + veggie + ' with color: ' + color);
}
```

## Use lodash library _.contains to check inclusion of element in an Array

The lodash library `_.contains` function must be used if you want to check that an element is present in an Array.  You
must not use `Array.indexOf()`, since it is easy to mistake the meaning and invert the logic.

*Right:*

```js
var fruits = ['apple', 'banana', 'cranberry'];

if (_.contains(fruits, 'banana')) {
  console.log('winning');
}
```

*Wrong:*

```js
var fruits = ['apple', 'banana', 'cranberry'];

if (fruits.indexOf('banana') !== -1) {
  console.log('losing');
}
```

## Declare one variable per var statement

Declare one variable per var statement, it makes it easier to re-order the
lines. Ignore [Crockford][crockfordconvention] on this, and put those
declarations wherever they make sense.

*Right:*

```js
var keys   = ['foo', 'bar'];
var values = [23, 42];

var object = {};
while (items.length) {
  var key = keys.pop();
  object[key] = values.pop();
}
```

*Wrong:*

```js
var keys = ['foo', 'bar'],
    values = [23, 42],
    object = {},
    key;

while (items.length) {
  key = keys.pop();
  object[key] = values.pop();
}
```

[crockfordconvention]: http://javascript.crockford.com/code.html

## Declare one variable per var statement

It may be a convention in some third-party code to use a single 'var' statement at top of file to house the 'require'
statements, but we would like to move away from that practice since it causes headaches when trying to read diff
output.

## Use lowerCamelCase for variables, properties and function names

Variables, properties and function names should use `lowerCamelCase`.  They
should also be descriptive. Single character variables and uncommon
abbreviations should generally be avoided.

*Right:*

```js
var adminUser = db.query('SELECT * FROM users ...');
```

*Wrong:*

```js
var admin_user = db.query('SELECT * FROM users ...');
```

## Use UpperCamelCase for class names

Class names should be capitalized using `UpperCamelCase`.

*Right:*

```js
function BankAccount() {
}
```

*Wrong:*

```js
function bank_Account() {
}
```

## Use lowerCamelCase for globals and constants

Constants should be declared as regular variables or static class properties, using `lowerCamelCase` letters.

Node.js / V8 actually supports mozilla's [const][const] extension, but unfortunately that cannot be applied to class
members, nor is it part of any ECMA standard.  It also breaks Sonar, so use `var` instead.

Constants should be used sparingly, and should almost never be singleton values.  It's nearly always clearer to use a
literal value than to define a constant for it, unless you are doing significant computational work.  Maps or
collections involving Arrays and Objects are fine, when the situation calls for it.

As a general rule, don't export constants in modules or expose them in classes; that can lead to bad API design.

Exposing module-level globals for reading can sometimes be OK.  For writes it is preferred to export a function called
`setup(options)` which can modify them in bulk.

*Right:*

```js
var mopToIcon = {
  '1000': 'visa.png',
  '1001': 'discover.png'
};

function File() {
  this.modMs = this.modTime * 1000;
  this.effectivePerms = 0777 & this.perms;
}
```

*Wrong:*

```js
var Second = 1 * 1000;

function File() {
}
File.FULL_PERMISSIONS = 0777;
```

*Wrong:*

```js
const SECOND = 1 * 1000;

function File() {
}
File.fullPermissions = 0777;
```

[const]: https://developer.mozilla.org/en/JavaScript/Reference/Statements/const

## Inheritance done right

Inheritance is tricky, especially when modules are involved, so we'll define the following guidelines.

* Classes are always defined using a named function (see `function Result()` below)
* That class is always set as a key of `exports`; never overwrite the entire module.exports or exports object with a class
* Class names and exported classes will be UpperCamelCase, and will have same name
* Always have `var self = this;` be the first line of every constructor and class method, and never use `this` outside of that one line; this allows us to categorically avoid weird edge cases around use of `this` in function calls
* Always have a single blank line after the above `var self = this;` statement
* The defintion of the constructor should never end with a semicolon
* Use `util.inherits` to do inheritance (see `util.inherits(SuccessResult, Result)` below); do this before the exports line
* Within the constructor of the child class, always run `ChildClass.super_.call`, replacing ChildClass appropriately (see `function SuccessResult()` below); unfortunately, the child class must be explicitly named in this `.call` statement
* This `.call` statement must include `self` as first argument and any remaining functions; if the arguments to child and parent constructor differ significantly, the design is questionable
* An acceptable alternative to `.call` is to use `.apply(self, arguments)`; this should be done if the arguments do not need to be changed before calling parent; note that we mean the literal automatic Javascript variable named `arguments` (see `function AmbivalentResult()` below for an example)
* If taking the constructor takes the recommended `options` argument, take steps to avoid overwriting its members, such as with `lodash` and its `_.extend()` function below.

Here is an example of inheritance done correctly:

```js
var _ = require('lodash');
var util = require('util');

//
// Result base class
//

function Result(inOptions) {
  var self = this;

  _.extend(self, {
    detail: null,
    message: null,
    status: null
  }, inOptions);
}

exports.Result = Result;

Result.prototype.getDefaultMessage = function getDefaultMessage() {
  var self = this;

  return 'unknown error: ' + JSON.stringify(_.pick(self, 'detail', 'status', 'message'));
};

//
// SuccessResult derived class
//

function SuccessResult(inOptions) {
  var self = this;

  inOptions = _.extend({
    detail: settings.serviceName + '_common_success',
    message: 'call successful'
  }, inOptions);

  // Initialize Result superclass
  SuccessResult.super_.call(self, inOptions);

  self.status = 'success';

  // an object to use when returning early from an async flow
  self.response = null;
}

util.inherits(SuccessResult, Result);
exports.SuccessResult = SuccessResult;

SuccessResult.prototype.getDefaultMessage = function getDefaultMessage() {
  var self = this;

  return 'unknown success reason: ' + JSON.stringify(_.pick(self, 'detail', 'status', 'message'));
};

//
// AmbivalentResult derived class
//

function AmbivalentResult(inOptions, existingErr) {
  var self = this;

  // Initialize Result superclass
  AmbivalentResult.super_.apply(self, arguments);
}

util.inherits(AmbivalentResult, Result);
exports.AmbivalentResult = AmbivalentResult;
```

## Object / Array creation

Do not use trailing commas. Do put *short* declarations on a single line. Only quote
keys when your interpreter complains:

*Right:*

```js
var a = ['hello', 'world'];
var b = {
  good: 'code',
  'is generally': 'pretty',
  'static': 'is a reserved keyword by ECMA'
};
```

*Wrong:*

```js
var a = [
  'hello', 'world'
];
var b = {"good": 'code'
        , is generally: 'pretty'
        };
```

## Use symmetric braces/brackets

Don't propagate bad practices from some members of the Perl community.  If you find yourself more than 6 columns from
your previous line, rethink your actions and prepare for rewrite.

*Right:*

```js
var collection = {
  foo: ['bar', 'baz']
};
var arr = [
  'long-element-needing-indentation',
  'longer-element-needing-indentation'
];
```

*Wrong:*

```js
var collection = {
                    foo: ['bar',
                          'baz'
                         ]
                 }
                 ;
var arr = ['long-element-needing-indentation',
           'longer-element-needing-indentation'
        ];
```

## Don't use the === operator unless you mean it

The triple equality operator is almost never what you actually mean when writing real programs.

If you want to check whether a field is truthy, omit the operator entirely, like this:

```js
if (apple) {
  console.log('apple is truthy');
}
```

If you want to check whether an Object field exists, use lodash library `_.has` function:

```js
var apple = {
  core: 1,
  stem: 2
};

if (_.has(apple, 'core')) {
  console.log('apple has a core');
}
```

If you want to check whether two variables point to the same Object, use `==`:

```js
var apple1 = {
  core: 1,
  stem: 2
};
var apple2 = {
  core: 1,
  stem: 2
};

if (apple1 == apple2) {
  console.log('apple1 is the same object as apple2');
}
```

You should generally avoid checking deep equality on two objects except in test cases.  For those, use a function
provided by the test suite:

```js
expect({ foo: 'bar' }).to.deep.equal({ foo: 'bar' });
```

When you must distinguish between several different falsy values, only then should you use the `===` operator.  Never
use `typeof` for this purpose.

*Right:*

```js
var a = 0;
if (a === 0) {
  console.log('winning');
}

var b;
if (b === undefined) {
  console.log('bi-winning');
}
```

*Wrong:*

```js
var a = 0;
if (a == 0) {
  console.log('losing');
}

var b;
if (typeof b === 'undefined') {
  console.log('bi-losing');
}
```

[comparisonoperators]: https://developer.mozilla.org/en/JavaScript/Reference/Operators/Comparison_Operators

## Usage of ternary operator

The ternary operator should be split up into multiple lines when any of the terms is complex.  If you do this as part
of an assignment, line up the '=', '?' and ':' characters.

But it should be kept to a single line when all of the terms are simple.

Parens should not be used around the condition part of the operation (the `a == b` part below), unless the condition is
complex.

If the ternary operation is part of a string concatenation or array, parens should be placed around the entire constuct.

### Simple case

*Right:*

```js
var foo = a == b ? 1 : 2;
```

*Wrong:*

```js
var foo = (a == b)
  ? 1
  : 2;
```

### Complex case

*Right:*

```js
var foo = apple.length == banana.length * 2
        ? apple.slice(banana, 2, 1)
        : banana.slice(apple, 2, 1);
var pear = { verb: 'crunchy' };
var bar = (pear === undefined ? 'i have no' : 'i have a ' + pear.verb) + ' pear';
```

*Wrong:*

```js
var foo = (apple.length == banana.length * 2) ? apple.slice(banana, 2, 1) : banana.slice(apple, 2, 1);
var pear = { verb: 'crunchy' };
var bar = ((pear === undefined) ? 'i have no' : ('i have a ' + pear.verb)) + ' pear';
```

## Do not extend built-in prototypes

Do not extend the prototype of native JavaScript objects. Your future self will
be forever grateful.

*Right:*

```js
var a = [];
if (_.isEmpty(a)) {
  console.log('winning');
}
```

*Wrong:*

```js
Array.prototype.empty = function() {
  return !this.length;
}

var a = [];
if (a.empty()) {
  console.log('losing');
}
```

## Use of ! in condiitons

Use a single space between `!` and the condition.

*Right:*

```js
var arr = [1];
if (! _.isEmpty(a)) {
  console.log('winning');
}
if (! (_.contains(arr, 2) || _.contains(arr, 3))) {
  console.log('winning');
}
```

*Wrong:*

```js
var arr = [1];
if (!_.isEmpty(a)) {
  console.log('losing');
}
if (!(_.contains(arr, 2) || _.contains(arr, 3))) {
  console.log('losing');
}
```

## Use descriptive conditions

Any non-trivial conditions should be assigned to a descriptive variable.  We define non-trivial to mean: a condition
composed of two or less operations.

Exception: If a trivial condition would be used more than once in the same function, or you have multiple similar of
them in the same function, then it should be treated as non-trivial and have its own descriptive variable.

*Right:*

```js
if (user.isAdmin() || user.isModerator()) {
  console.log('winning');
}
```

*Wrong:*

```js
var isAuthorized = (user.isAdmin() || user.isModerator());
if (isAuthorized) {
  console.log('losing');
}
```

*Right:*

```js
var isAuthorized = (user.isAdmin() || user.isModerator());
var isStaff = (user.isProfessor() || user.isJanitor());

if (isAuthorized && isStaff) {
  console.log('winning');
}
```

*Wrong:*

```js
if ((user.isAdmin() || user.isModerator()) && (user.isProfessor() || user.isJanitor())) {
  console.log('losing');
}
```

## Write small functions when possible

Keep your functions short. A good function fits on a slide that the people in
the last row of a big room can comfortably read. So don't count on them having
perfect vision and limit yourself to ~15 lines of code per function.

If you are doing operations requiring control flow, however, it is preferable to have a large outer function that
contains state and small inner functions which manipulate that state when callbacks finish.  Be sure to name these
functions.

## When callbacks are used in if statements, do not return early

While deep nesting of if-statements should be avoided, avoid `return` statements when possible, as overusing them can
mask problems.

It's easier to reason about callback behavior and catch control flow problems when you use `if/else` in preference to
`if/return`.  Due to node.js being so callback-focused, this means you should almost never use `return`.

For complex computational work that does not involve callbacks, `return` statements are preferable to deeply-nested
`if` statements.

*Right:*

```js
if (errorHappened) {
  callback(new Error('error happened'));
} else {
  callback(null, 'winning');
}
```

*Wrong:*

```js
if (errorHappened) {
  callback(new Error('error happened'));
  return;
}

// oops, forgot my callback
console.log('losing');

// add it back after my service crashes
callback(null, 'still losing');
```

*Right:*

```js
function isPercentage(val, callback) {
  if (val >= 0) {
    if (val < 100) {
      callback(true);
    } else {
      callback(false);
    }
  } else {
    callback(false);
  }
}
```

*Wrong:*

```js
function isPercentage(val, callback) {
  if (val < 0) {
    callback(false);
    return;
  }

  if (val > 100) {
    callback(false);
    return;
  }

  callback(true);
}
```

For this particular example it is preferable to shorten things even further, and minimize the number of code paths
which can execute callbacks:

*Even better:*

```js
function isPercentage(val) {
  var isInRange = (val >= 0 && val <= 100);
  callback(isInRange);
}
```
## Function naming

Functions should _always_ be named.  This is for clarity in both the code itself and in stacktraces
when an exception occurs.  It also results in better heap and CPU profiles, which is crucial for tracking
own problems.  Named functions are also more performant than anonymous functions that are not named.

When naming functions, strive for a balance of being both succinct and descriptive.

(While writing tests, it is okay for functions to not be named (ie. `it('should do ...', function() { ...`).  Mocha
makes heavy use of anonymous functions as callbacks, and those functions are just a means to an end for writing tests.)
Extremely trivial one-line functions can also sometimes go without names.

*Right:*

```js
var jadeFiles = _.filter(filenames, function jadeOnly(filename) {
  return _s.strRightBack(filename, '.') === 'jade';
});
```

*Wrong:*

```js
var jadeFiles = _.filter(filenames, function(filename) {
  return _s.strRightBack(filename, '.') === 'jade';
});
```

*Also Wrong:*

```js
var jadeFiles = _.filter(filenames, function filterJadeFilesByExtension(filename) {
  return _s.strRightBack(filename, '.') === 'jade';
});
```

## No nested functions

Use function closures, but don't nest them overly. Otherwise your code will become a mess.  Nested functions also
perform badly, so try to keep functions defined at the top level (or within a class prototype) when reasonable.

If you have multiple asynchronous code blocks, use async.series() and safe-callback to split the code into tasks.

*Right:*

```js
setTimeout(function later() {
  client.connect(afterConnect);
}, 1000);

function afterConnect() {
  console.log('winning');
}
```

*Also Right:*

```js
setTimeout(function later() {
  client.connect(function() {
    console.log('still winning');
  });
}, 1000);
```

*Wrong:*

```js
setTimeout(function() {
  request('url',
          function() {
            client.connect(function() {
              console.log('losing');
            })
          })
  );
}, 1000);
```

## Use slashes for comments

Use slashes for both single line and multi line comments. Try to write comments that explain higher level mechanisms or
clarify difficult segments of your code. Don't use comments to restate trivial things.  Comments must have a single
space between the slash and text.

*Right:*

```js
// 'ID_SOMETHING=VALUE' -> ['ID_SOMETHING=VALUE'', 'SOMETHING', 'VALUE']
var matches = item.match(/ID_([^\n]+)=([^\n]+)/));

// This function has a nasty side effect where a failure to increment a
// redis counter used for statistics will cause an exception. This needs
// to be fixed in a later iteration.
function loadUser(id, cb) {
  // ...
}

var isSessionValid = (session.expires < Date.now());
if (isSessionValid) {
  // ...
}
```

*Wrong:*

```js
// Execute a regex
var matches = item.match(/ID_([^\n]+)=([^\n]+)/));

// Usage: loadUser(5, function() { ... })
function loadUser(id, cb) {
  // ...
}

// Check if the session is valid
var isSessionValid = (session.expires < Date.now());
// If the session is valid
if (isSessionValid) {
  // ...
}
```

*Also Wrong:*

```js
//Execute a regex
var matches = item.match(/ID_([^\n]+)=([^\n]+)/));
```

## Do not write commented documentation for general program usage

If you have a requirement to write documentation on usage of an entire codebase, then use a README.md file or internal
site.  If your intended audience is internal-only, you should prefer to write documentation hosted on an internal site,
and you should not use README.md files.

Your co-workers don't care about running "make docs", and care even less to read your entire source tree to understand
how to start your service.

If your code is for a standalone script, it may make sense to support the `--usage` parameter, in which case
documentation should be code, not comments.

*Wrong:*

```js
/**
NAME

Web application for the Front End SOA

SYNOPSIS

From the command-line

_(Only file-based configuration is considered.)_

    $ node myservice
    Myservice is listening on port 8080 ...
**/

exports.init = init;
```

## Do write in-code documentation on classes and utility functions that are nonobvious

If the purpose of a function or class is clear from name or code, then documentation may omitted.

For nonobvious functions: documentation should only cover immediate use of functions.  For classes: it may have a brief
overview of all functions which are properties of the class, as a means of describing how the class is meant to be
used.

For both: It should not be overly verbose. It should come just before the definition of the function or class. It
should not be at the beginning or end of the file.  It should never have placeholder garbage like "SYNOPSIS" in POD
docs; usage of any POD constructs in documentation is a failure.

If the documentation approaches 75% of the length of the code, or the code is too complex to explain succinctly, then
you should probably move the code into its own node.js module with separate code repo, and move the documentation into
internal website or README.md file for that module.

## Be careful about links to internal sites within code

Linking to internal sites (like Jira) in comments about code changes can be OK, but only if the change is non-obvious
product logic.  Such links must be accompanied with a brief, clear, 1-2 line description of the product requirement.
Avoid having too many of them in the same part of the codebase; prefer to replace old ones if requirements have
changed.

## Object.freeze, Object.preventExtensions, Object.seal, with, eval

Crazy shit that you will probably never need. Stay away from it.

## Getters and setters

Do not use setters, they cause more problems for people who try to use your
software than they can solve.

Feel free to use getters that are free from [side effects][sideeffect], like
providing a length property for a collection class.

[sideeffect]: http://en.wikipedia.org/wiki/Side_effect_(computer_science)
