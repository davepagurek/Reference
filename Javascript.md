# Javascript

## Node console I/O
```javascript
var sys = require("sys");

var stdin = process.openStdin();

stdin.addListener("data", function(d) {
  // note:  d is an object, and when converted to a string it will
  // end with a linefeed.  so we (rather crudely) account for that  
  // with toString() and then substring() 
  console.log("you entered: " + d.toString().trim() );
});
```

## Underscore/lodash
### Functional programming
```javascript
_.chain([{name: 'curly', age: 25}, {name: 'moe', age: 21}, {name: 'larry', age: 23}])
  .sortBy((stooge) => stooge.age)
  .map((stooge) => `${stooge.name} is ${stooge.age}`)
  .first()
  .value();
// "moe is 21"

_.range(1, 11).map((x) => x*x)
// [ 1, 4, 9, 16, 25, 36, 49, 64, 81, 100 ]

_.mapObject({a: 1, b: 2}, (k, v) => `${k} is ${v}`)
// {a: "a is 1", b: "b is 2"}

_.fold(_.range(1, 11), (sum, x) => sum+x, 0)
// 75

_.groupBy(_.range(1, 11), (x) => x < 5 ? "lt", "gte")
// { lt: [ 1, 2, 3, 4 ], gte: [ 5, 6, 7, 8, 9, 10 ] }

_.every([2, 4, 6, 8], (x) => x % 2 == 0)
// true

var greet    = function(name){ return "hi: " + name; };
var exclaim  = function(statement){ return statement.toUpperCase() + "!"; };
var welcome = _.compose(greet, exclaim);
welcome('moe');
// 'hi: MOE!'
```

### List methods
```javascript
_.intersection([1, 2, 3], [3, 4, 5])
// [ 3 ]

_.union([1, 2, 3], [3, 4, 5])
// [1, 2, 3, 4, 5]

_.contains([1, 2, 3], 3);
// true

_.object(['moe', 'larry', 'curly'], [30, 40, 50]);
// {moe: 30, larry: 40, curly: 50}

_.object([['moe', 30], ['larry', 40], ['curly', 50]]);
// {moe: 30, larry: 40, curly: 50}

_.keys({a: 1, b: 2})
// [ "a", "b" ]

_.values({a: 1, b: 2})
// [ 1, 2 ]

_.pairs({one: 1, two: 2, three: 3});
// [["one", 1], ["two", 2], ["three", 3]]

_.extend({a: 1}, {b: 2})
// {a: 1, b: 2}
```

### 
```
var fibonacci = _.memoize(function(n) {
  return n < 2 ? n: fibonacci(n - 1) + fibonacci(n - 2);
});
// Caches results when used

var throttled = _.throttle(updatePosition, 100);
// Limits the function to run at most once every 100ms
```
