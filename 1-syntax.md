# Let

When using the let keyword, no hoisting occurs.

    'use strict';
    console.log(productId); // Error: productId is not defined
    let productId = 4;

When using 'let', variables not assigned a value are undefined.

    'use strict';
    let productId;
    console.log(productId); // productId is undefined (declared but not assigned)

The let keyword also isolates closures.

    'use strict';
    let upstreamFunctions = [];
    for (let i = 0; i < 2; i++) {
      updateFunctions.push(function() { return i; });
    }
    console.log(updateFunctions[0]()); // Outputs "0". If 'var' is used for variable i, it would update 2.

# Block Scoping

We have block scoping with curly braces. Variables declared inside blocks are scoped to the block.

    'use strict';
    let productId = 4;
    {
      let productId = 5000;
    }
    console.log(productId); // Outputs "4"

Block scoping also applies to conditionals.

    'use strict';
    let productId = 42;
    for(let productId = 0; productId < 10; productId++) 
    {
    }
    console.log(productId); // outputs "42"

# Const

Const variables behave similarly to other variables.

    'use strict';
    const MARKUP_PCT = 100;
    console.log(MARKUP_PCT); // outputs "100"

Const variables MUST BE initialized. Use of uninitialized constants will result in syntax error.

    'use strict';
    const MARKUP_PCT;
    console.log(MARKUP_PCT); // throws syntax error

Constants cannot be reassigned.

    'use strict';
    const MARKUP_PCT = 100;
    MARKUP_PCT = 10;
    console.log(MARKUP_PCT); // throws type error

# Arrow Functions

Arrow functions are a form of shorthand. You can leave off the function keyword and return statement. You cannot put the 'fat arrow' symbol on a new line.

    'use strict';
    var getPrice = () => 5.99;
    console.log(typeof getPrice); // outputs "function"

When your function only has one argument, you can omit the parentheses.

    'use strict';
    var getPrice = count => count * 4.00;
    console.log(getPrice(2)); // outputs "8"

When your function has multiple parameters, they must be enclosed in parentheses.

    'use strict';
    var getPrice = (count, tax) => count * 4.00 * (1 + tax);
    console.log(getPrice(2, .07)); // outputs "8.56"

You can declare a block as the function, but you must use a return keyword in this case.

    'use strict';
    var getPrice = (count, tax) => {
      var price = count * 4.00;
      price *= (1 + tax);
      return price;
    }
    console.log(getPrice(4, .07)); // outputs "8.56"

Arrow function main purpose is to deal with scoping of the 'this' keyword. In ES5, 'this' refers to the object on which the function is called.

    'use strict';
    document.addEventListener('click', function() {
      console.log(this);
    });
    // outputs "#document"

In ES6, 'this' refers to the context in which the function was called.

    'use strict';
    document.addEventListener('click', () => console.log(this));
    // outputs "Window {...}"

A more complex example:

    'use strict';
    var invoice = {
      number: 100,
      process: function() {
        return () => console.log(this.number);
      }
    };
    invoice.process()(); // outputs "100"

    // Since process is a function, when we invoke the function returned by process, we are running in the context of the process function which has access to the number property.

You cannot bind, call or apply new objects (contexts) to arrow functions.

    'use strict';
    var invoice = {
      number: 100,
      process: function() {
        return () => console.log(this.number);
      }
    };
    var newInvoice = {
      number: 200
    };
    invoice.process().bind(newInvoice)(); // outputs "100"

When declaring a function using the 'fat arrow', we don't have access to the prototype field.

    'use strict';
    var getPrice = () => 5.99;
    console.log(getPrice.hasOwnProperty('prototype')); // outputs "false"

# Default Function Parameters

Function parameters can have default values.

    'use strict';
    var getProduct = function(productId = 1000) {
      console.log(productId);
    };
    getPrice(); // outputs "1000"

The defaults are used even in the case of explicitly passing 'undefined'.

    'use strict';
    var getProduct = function(productId = 1000, type = 'software) {
      console.log(productId + ', ' + type);
    };
    getPrice(undefined, 'hardware'); // outputs "1000, hardware"

Default values can access both other parameters and other variables in the context.

    'use strict';
    var baseTax = 0.07;
    var getTotal = function(price, tax = price * baseTax) {
      console.log(price + tax);
    };
    getTotal(5.00); // outputs "5.35"

While not a best practice, the 'arguments' keywoard refers only to the arguments passed to the function.

    'use strict';
    var getTotal = function (price, tax = 0.07) {
      console.log(arguments.length);
    };
    getTotal(5.00); // outputs "1"

Default parameter values are subject to scoping.

    'use strict';
    var getTotal = function(price = adjustment, adjustment = 1.00) {
      console.log(price + adjustment);
    };
    getTotal(); // throws "SyntaxError: use before declaration"

# Rest

The 'rest' operator is an elipsis: '...' and is used to bundle up 'the rest' of the arguments passed to a function.

    'use strict';
    var showCategories = function(productId, ...categories) {
      console.log(categories);
    };
    showCategories(123, 'search', 'advertising'); // outputs "true"

The 'rest' operator bundles up all of the unassigned function parameters.

    'use strict';
    var showCategories = function(productId, ...categories) {
      console.log(categories);
    };
    showCategories(123, 'search', 'advertising'); // outputs "['search', 'advertising']"
    showCategories(123); // outputs "[]"

# Spread

Spread is essentially the opposite of rest, spreading out an array that is passed as an argument.

    'use strict';
    var prices = [12, 20, 18];
    var maxPrice = Math.max(...prices);
    console.log(maxPrice); // outputs "20"

Similar to ES5, Array values are undefined until assigned.

    'use strict';
    var newPriceArray = [,,];
    console.log(newPriceArray); // outputs "undefined, undefined" (the trailing comma is ignored)

The spread operator will also break a string into individual characters.

    'use strict';
    var maxCode = Math.max(...'43210');
    console.log(maxCode); // outputs "4"
    
# Object Literals

Object literals are shorthand.

    'use strict';
    var price = 5.99, quantity = 30;
    var productView = {
      price,
      quantity
    };
    console.log(productView); // outputs {price: 5.99, quantity: 30}

There is also shorthand notation for declaring functions in object literals.

    'use strict';
    var price = 5.99, quantity = 30;
    var productView = {
      price,
      quantity,
      calculateValue() {
        return this.price * this.quantity;
      }
    };
    console.log(productView.calculateValue()); // outputs 59.900000000000006

Shorthand function notation is as if you are using an arrow function, where 'this' refers to the context the function is executed in.

    'use strict';
    var price = 5.99, quantity = 30;
    var productView = {
      price: 7.99,
      quantity: 1,
      calculateValue() {
        return this.price * this.quantity;
      }
    };
    console.log(productView.calculateValue()); // outputs 59.900000000000006
    
You can use expressions with bracket notation inside the object literal.

    'use strict';
    var field = 'dynamicField';
    var price = 5.99;
    var productView = {
      [field]: price
    };
    console.log(productView); // outputs "{dynamicField: 5.99}"

# For ... Of Loops

// To Be Continued...



