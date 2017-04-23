# Course work for Pluralsight course "Rapid ES6 Training"

https://app.pluralsight.com/library/courses/rapid-es6-training

# ES6 Compatibility Table

http://kangax.github.io/compat-table/es6/

# Syntax

## Let

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

## Block Scoping

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

## Const

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

## Arrow Functions

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

## Default Function Parameters

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

Default values can access other parameters and other variables in the context.

    'use strict';
    var baseTax = 0.07;
    var getTotal = function(price, tax = price * baseTax) {
      console.log(price + tax);
    };
    getTotal(5.00); // outputs "5.35"

While use of `arguments` is not a best practice, the 'arguments' keywoard refers only to the arguments passed to the function.

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

## Rest

The 'rest' operator (basically an elipsis: '...') is used to bundle up 'the rest' of the arguments passed to a function.

    'use strict';
    var showCategories = function(productId, ...categories) {
      console.log(categories);
    };
    showCategories(123, 'search', 'advertising'); // outputs "['search', 'advertising']"

The 'rest' operator bundles up all of the unassigned function parameters.

    'use strict';
    var showCategories = function(productId, ...categories) {
      console.log(categories);
    };
    showCategories(123, 'search', 'advertising'); // outputs "['search', 'advertising']"
    showCategories(123); // outputs "[]"

## Spread

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
    
## Object Literals

Object literals are shorthand.

    'use strict';
    var price = 5.99, quantity = 30;
    var productView = {
      price,
      quantity
    };
    console.log(productView); // outputs "{price: 5.99, quantity: 30}"

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
    console.log(productView.calculateValue()); // outputs "59.900000000000006"

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
    console.log(productView.calculateValue()); // outputs "59.900000000000006"
    
You can use expressions with bracket notation inside the object literal.

    'use strict';
    var field = 'dynamicField';
    var price = 5.99;
    var productView = {
      [field]: price
    };
    console.log(productView); // outputs "{dynamicField: 5.99}"

## For ... Of Loops

Shorthand for iterating over iterables.

    'use strict';
    var categories = ['hardware', 'software', 'vaporware'];
    for (var item of categories) {
      console.log(item);
    }
    // outputs
    // "hardware
    // software
    // vaporware"

For...Of can also be used to iterate over strings.

    'use strict';
    var codes = "ABCDF";
    var count = 0;
    for (var code of codes) {
      count++;
    }
    console.log(count); // outputs "5"

## Octal and Binary Literals

Octal values begin with `0o`. This prefix is case insensitive.

    'use strict';
    var value = 0o10;
    console.log(value); // outputs "8"

Binary values begin with `0b`. This prefix is base insensitive.

    'use strict';
    var value = 0b10;
    console.log(value); // outputs "2"

## Template Literals

Templates, in this context, refer to string variables with interpolated values and expressions.

Enclose the string in backticks and use ${} around interpolated values.

    'use strict';
    let invoiceNum = '1350';
    console.log(`Invoice Number: ${invoiceNum}`); // outputs "Invoice Number: 1350"

Escaping the `$` can be used to prevent interpolation.

    'use strict';
    let invoiceNum = '1350';
    console.log(`Invoice Number: \${invoiceNum}`); // outputs "Invoice Number: ${invoiceNum}"

Template literals maintain whitespace (new lines and tabs).

    'use strict';
    let message = `A
    B
    C`;
    console.log(message);
    // outputs
    // "A
    // B
    // C"

Template literals allow expressions.

    'use strict';
    let invoiceNum = '1350';
    console.log(`Invoice Number: ${"INV-" + invoiceNum}`); // outputs "Invoice Number: INV-1350"

When using an interpolated string as a function argument, interpolation happens before the function call.

    'use strict';

    function showMessage(message) {
      let invoiceNum = '99';
      console.log(message);
    }

    let invoiceNum = 1350;
    showMessage(`Invoice Number: ${invoiceNum}); // outputs "Invoice Number: 1350"

## Tagged Template Literals

When using tagged template literals, the first argument passed to the function is an array of the strings that make up the template.

    'use strict';
    function processInvoice(segments) {
      console.log(segments);
    }

    processInvoice `template`; // outputs "['template']"

Subsequent values are the interpolated values (if any).

    'use strict';
    function processInvoice(segments, ...values) {
      console.log(segments);
      console.log(values);
    }

    let invoiceNum = '1350';
    let amount = '2000';
    processInvoice `Invoice: ${invoiceNum} for ${amount}`;

    // outputs
    // ["Invoice: ", " for ", ""]
    // [1350, 2000]

## Destructuring Part 1: Arrays

Use square brackets to destructure an array.

    'use strict';
    let salary = ['32000', '50000', '75000'];
    let [ low, average, high ] = salary;
    console.log(average); // outputs "50000"
  
Destructured values are undefined by default.

    'use strict';
    let salary = ['32000', '50000',];
    let [ low, average, high ] = salary;
    console.log(high); // outputs "undefined"

You can also ignore values.

    'use strict';
    let salary = ['32000', '50000', '75000'];
    let [ low, , high ] = salary;
    console.log(high); // outputs "75000"

Can be combined with the Rest paramater.

    'use strict';
    let salary = ['32000', '50000', '75000'];
    let [ low, ...remaining ] = salary;
    console.log(remaining); // outputs ["50000", "75000"]

Can be used with defaults.

    'use strict';
    let salary = ['32000', '50000'];
    let [ low, average, high = '88000' ] = salary;
    console.log(high); // outputs "88000"

Destructuring works with multidimensional arrays.

    'use strict';
    let salary = ['32000', '50000', ['88000', '99000']];
    let [ low, average, [actualLow, actualHigh] ] = salary;
    console.log(actualLow); // outputs "88000"

You can keep declaration and assignment separate.
    
    'use strict';
    let salary = ['32000', '50000'];
    let low, average, high;
    [ low, average, high = '88000' ] = salary;
    console.log(high); // outputs "88000"
    
You can use destructuring in functions.

    'use strict';
    function reviewSalary([low, average], high = '88000') {
      console.log(average);
    }

    reviewSalary(['32000', '50000']); // outputs "50000"

## Destructuring Part 2: Objects

Use curly braces to destructure an object. Your variable names must match the object property keys.

    'use strict';
    let salary = {
      low: '32000',
      average: '50000',
      high: '75000'
    };
    let { low, average, high } = salary;
    console.log(high); // outputs "75000"

You can override property names with new variable names.

    'use strict';
    let salary = {
      low: '32000',
      average: '50000',
      high: '75000'
    };
    let { low: newLow, average: newAverage, high: newHigh } = salary;
    console.log(newHigh); // outputs "75000"

When destructuring an object, and separating the declaration from the assignment of the variables, you need to do something special.

    'use strict';
    let salary = {
      low: '32000',
      average: '50000',
      high: '75000'
    };
    let newLow, newAverage, newHigh;
    { low: newLow, average: newAverage, high: newHigh } = salary; // Syntax Error
    console.log(newHigh); 

Enclose the destructuring statement in parentheses.

    'use strict';
    let salary = {
      low: '32000',
      average: '50000',
      high: '75000'
    };
    let newLow, newAverage, newHigh;
    ({ low: newLow, average: newAverage, high: newHigh } = salary); 
    console.log(newHigh); // outputs "75000"

## Destructuring Part 3: Strings

Strings can be destructured in the same way as arrays, using square brackets.

    'use strict';
    let [ maxCode, minCode ] = 'AZ';
    console.log(`min: ${minCode}, max: ${maxCode}`); // outputs "min: A, max: Z"

## Destructuring Part 4: Edge Cases

Destructuring empty arrays:

    'use strict';
    let [high, low] = [,];
    console.log(`high: ${high}, low: ${low});
    // outputs "high: undefined, low: undefined

Destructuring `null`:

    'use strict';
    let [high, low] = null;
    console.log(`high: ${high}, low: ${low});
    // Runtime Error: Unable to get property 'Symbol.iterator' of undefined or null reference

Destructuring `undefined`:

    'use strict';
    let [high, low] = undefined;
    console.log(`high: ${high}, low: ${low});
    // Runtime Error: Unable to get property 'Symbol.iterator' of undefined or null reference

The actual error is a TypeError:

    'use strict';
    try {
      let [ high, low, ] = undefined;
    } catch (e) {
      console.log(e.name); // outputs "TypeError"
    }

Trailing commas are OK.

    'use strict';
    let [ high, low, ] = [500, 200];
    console.log(`high: ${high}, low: ${low}`);
    // outputs "high: 500, low: 200"

Destructuring nested arrays.

    'use strict';
    for (let [a, b] of [[5, 10]]) {
      console.log(`${a} ${b}`); / outputs "5 10"
    }

... and it is optimized

    'use strict';
    let count = 0;
    for (let [a, b] of [[5, 10]]) {
      console.log(`${a} ${b}`);
      count++;
    }
    console.log(count);
    // outputs
    // "5 10"
    // "1"

# Modules and Classes

## Module Basics

A module is only loaded once and is automatically loaded in 'strict' mode, which prevents things like assigning values to undeclared variables.

    projectId = 99; // Runtime Error: Variable undefined in strict mode
    console.log('in base.js'); 

You can use aliases for imported variables. Doing so will render the original import name as undefined.

    File module1.js:

    export let projectId = 99;
    export let projectName = 'BuildIt';
  
    File base.js:

    import { projectId as id, projectName } from 'module1.js';
    console.log( `${projectName} has id: ${id}`); // outputs BuildIt has id: 99

Import statements get hoisted and dependencies are executed first.
    
    File module1.js:

    export let projectId = 99;
    console.log('in module1');

    File base.js:

    console.log('starting in base');
    import { projectId } from 'module1.js';
    console.log('ending in base');

    // outputs
    // "in module 1"
    // "starting in base"
    // "ending in base"

Modules can have a default export, imported without curly braces.

    File module1.js:

    export let projectId = 99;
    let projectName = 'BuildIt';
    export default projectName;

    File base.js:

    import someValue from 'module1.js';
    console.log(someValue); // outputs "BuildIt"

You can also explicitly specify the default.

    File module1.js:

    export let projectId = 99;
    let projectName = 'BuildIt';
    export default projectName;

    File base.js:

    import { default as projectName } from 'module1.js';
    console.log(projectName); // outputs "BuildIt"

Using a default when one doesn't exist results in the import failing.

    File module1.js:

    let projectId = 99;
    let projectName = 'BuildIt';
    export { projectId, projectName };

    File base.js:

    import someValue from 'module1.js';
    console.log(someValue); // outputs "undefined"

You can export multiple values, and define a default, with a single export.

    File module1.js:

    let projectId = 99;
    let projectName = 'BuildIt';
    export { projectId as default, projectName };

    File base.js:

    import someValue from 'module1.js';
    console.log(someValue); // outputs "BuildIt"

You can import 'all'. When you do this, you should assign an alias.

    File module1.js:

    let projectId = 99;
    let projectName = 'BuildIt';
    export { projectId, projectName };

    File base.js:

    import * as values from 'module1.js';
    console.log(values); 
    // outputs
    // "{projectId: 99, projectName: 'BuildIt'}"

## Named Exports in Modules

Named exports are read-only.

    File module1.js:

    export let projectId = 99;

    File base.js:

    import {projectId} from 'module1.js';
    projectId = 8000; // Runtime Error: projectId is read-only
    console.log(projectId); 

but not if the property is inside an object...

    File module1.js:

    export let project = {
      projectId: 99
    };

    File base.js:

    import {project} from 'module1.js';
    project.projectId = 8000;
    console.log(project.projectId); // outputs "8000"

If you change one object from inside another module, both modules stay in-sync.

    File module1.js:

    export let project = {
      projectId: 99
    };
    export function showProject() {
      console.log(project.projectId);
    }

    File base.js:

    import {project, showProject} from 'module1.js';
    project.projectId = 8000;
    showProject(); // outputs "8000"
    console.log(project.projectId); // outputs "8000"

Functions also stay in-sync.

    File module1.js:

    export function showProject() { console.log('in original'); };
    export function updateFunction() {
      showProject = function () { console.log('in updated'); };
    };

    File base.js:

    import { showProject, updateFunction } from 'module1.js';
    showProject(); // outputs "in original"
    updateFunction();
    showProject(); // outputs "in updated"

## Class Fundamentals

Classes are actually just typed functions.

    class Task {

    }

    console.log(typeof Task); // outputs "function"

Instantiated classes are objects.

    class Task {

    }
    let task = new Task();
    console.log(typeof task); // outputs "object"

You can use instanceof.

    class Task {

    }
    let task = new Task();
    console.log(task instanceof Task); // outputs "true"

Classes have shorthand notation for properties and functions.

    class Task {
      showId() {
        console.log('99');
      }
    }
    let task = new Task();
    task.showId(); // outputs "99"

Adding a method to a class is similar to adding that method to the object's prototype.

    class Task {
      showId() {
        console.log('99');
      }
    }
    let task = new Task();
    console.log(task.showId === Task.prototype.showId); // outputs "true"

You can define constructors.

    class Task {
      constructor() {
        console.log('constructing Task');
      }
      showId() {
        console.log('99');
      }
    }
    let task = new Task(); // outputs "constructing Task"

No commas inside class definitions.

    class Task {
      constructor() {
        console.log('constructing Task');
      }, // <-- there's a comma here
      showId() {
        console.log('99');
      }
    }
    let task = new Task();
    // Syntax error

You cannot declare variables inside class bodies.

    class Task {
      let taskId = 9000; // Syntax error
      constructor() {
        console.log('constructing Task');
      }
      showId() {
        console.log('99');
      }
    }
    let task = new Task();

Class definitions are not hoisted.

    let task = new Task(); // Error: use before declaration

    class Task {
      constructor() {
        console.log('constructing Task');
      }
    }

You can use classes as expressions similar to constructor functions in es5.

    let newClass = class Task {
      constructor() {
        console.log('constructing Task');
      }
    };

    new newClass(); // outputs "constructing Task"

You can't call `call` to change the context.

    class Task {
      constructor() {
        console.log('constructing Task');
      }
    }
    let task = {};
    Task.call(task); // Error: class constructor cannot be called with the new keyword

Classes don't get put on the global namespace.

    class Task { };

    console.log(window.Task === Task); // outputs "false"

## Keywords extends and super

The `extends` keyword enables prototypal inheritance.

    class Project {
      constructor(name) {
        console.log('constructing Project: ' + name);
      }
    }

    class SoftwareProject extends Project {}

    let p = new SoftwareProject('test'); // outputs "constructing Project: test"

the `super` keyword allows you to reference a class' parent.

    class Project {
      constructor() {
        console.log('constructing Project');
      }
    }

    class SoftwareProject extends Project {
      constructor() {
        super();
        console.log('constructing SoftwareProject');
      }
    }

    let p = new SoftwareProject();

    // outputs
    // "constructing Project"
    // "constructing SoftwareProject"

You cannot 'override' a prototypal constructor. The constructor needs to know when to invoke the upstream constructor.

    class Project {
      constructor() {
        console.log('constructing Project');
      }
    }

    class SoftwareProject extends Project {
      constructor() {
        //super();
        console.log('constructing SoftwareProject');
      }
    }

    let p = new SoftwareProject();

    // ReferenceError: this is not defined

Likewise, you must have constructors on the entire prototype chain.

    class Project {
      // constructor() {
      //   console.log('constructing Project');
      // }
    }

    class SoftwareProject extends Project {
      constructor() {
        //super();
        console.log('constructing SoftwareProject');
      }
    }

    let p = new SoftwareProject();

    // ReferenceError: this is not defined

Functions behave differently. You can override functions in children.

    class Project {
      getTaskCount() {
        return 50;
      }
    }
    class SoftwareProject extends Project {
      getTaskCount() {
        return 66;
      }
    }
    let p = new SoftwareProject();
    console.log(p.getTaskCount()); // outputs "66"

`super` can also be used in functions.

    class Project {
      getTaskCount() {
        return 50;
      }
    }
    class SoftwareProject extends Project {
      getTaskCount() {
        return super.getTaskCount() + 6;
      }
    }
    let p = new SoftwareProject();
    console.log(p.getTaskCount()); // outputs "56"

Using object literals.

    let project = {
      getTaskCount() { return 50; }
    };
    let softwareProject = {
      getTaskCount() {
        return super.getTaskCount() + 7;
      }
    };
    Object.setPrototypeOf(softwareProject, project);
    console.log(softwareProject.getTaskCount()); // outputs "57"

## Properties for Class Instances

You initialize instance variables using class constructors.

    class Project {
      constructor() { this.location = "France"; }
    }
    class SoftwareProject extends Project {
      constructor() {
        super();
      }
    }
    let p = new SoftwareProject();
    console.log(p.location); // outputs "France"

Objects can go out of scope if you use let, const or var in the constructor.

    class Project {
      constructor() { let location = "France"; }
    }
    class SoftwareProject extends Project {
      constructor() {
        super();
      }
    }
    let p = new SoftwareProject();
    console.log(p.location); // outputs "undefined"

Attached instance variables can be used across constructors.

    class Project {
      constructor() { this.location = 'Paris'; }
    }
    class SoftwareProject extends Project {
      constructor() {
        super();
        this.location = this.location + ', France';
      }
    }
    let p = new SoftwareProject();
    console.log(p.location); // outputs "Paris, France"

## Static Members

Static members are attached to the class as a constructor function. You don't have to use the `new` keyword.

    class Project {
      static getDefaultId() {
        return 0;
      }
    }
    console.log(Project.getDefaultId()); // outputs "0"

Static members can't be called using class instances.

    class Project {
      static getDefaultId() {
        return 0;
      }
    }
    var p = new Project();
    console.log(p.getDefaultId());
    // Error: Object doesn't support property or method getDefaultId

You can't have static properties.

    class Project {
      static let id = 0;
    }
    console.log(Project.Id); // Syntax Error: ( expected

You CAN attach properties directly to the class or constructor function.

    class Project {

    }
    Project.id = 99;
    console.log(Project.id);

## new.target

`new.target` is mainly used in a constructor, and is of type `function`.

    class Project {
      constructor() {
        console.log(typeof new.target);
      }
    }
    var p = new Project();
    // outputs "function"

`new.target` points to the constructor of the enclosing class.

    class Project {
      constructor() {
        console.log(new.target);
      }
    }
    var p = new Project();
    // outputs
    // constructor() {
    //   console.log(new.target);  
    // }

When extending an object, `new.target` points to the constructor initially called.

    class Project {
      constructor() {
        console.log(new.target);
      }
    }
    class SoftwareProject extends Project {
      constructor() {
        super();
      }
    }
    var p = new SoftwareProject();
    // outputs
    // constructor() {
    //   super();  
    // }

... even when the initial constructor is a default constructor.

    class Project {
      constructor() {
        console.log(new.target);
      }
    }
    class SoftwareProject extends Project {}
    var p = new SoftwareProject();
    // outputs
    // constructor(...args) {
    //   super(...args);  
    // }

`new.target` is really useful if you need access to static methods on the prototype chain. This only works on static methods.

    class Project {
      constructor() {
        console.log(new.target.getDefaultId());
      }
    }
    class SoftwareProject extends Project {
      static getDefaultId() { return 99; }
    }
    var p = new SoftwareProject();
    // outputs "99"

# Symbols

The main new type is the `Symbol` type.

The purpose of a symbol is to generate a unique identifier. The actual ID is hidden.

    let eventSymbol = Symbol('resize event');
    console.log(typeof eventSymbol);
    // outputs "symbol"
    console.log(eventSymbol.toString());
    // outputs "Symbol(resize event)"

Symbols can be used with constants.

    const CALCULATE_EVENT_SYMBOL = Symbol('calculate event');
    console.log(CALCULATE_EVENT_SYMBOL.toString());
    // outputs Symbol(calculate event)

Each symbol created is unique, even if the constructor argument is reused.

    let s = Symbol('event');
    let s2 = Symbol('event');
    console.log(s === s2); // outputs "false"

You can access the Symbol registry using `Symbol.for`. You will either get a reference to an existing symbol, or a new one if it doesn't already exist.

    let s = Symbol.for('event');
    let s2 = Symbol.for('event');
    console.log(s === s2); // returns "true"

`Symbol.keyFor` allows you to "dereference" a symbol and get the string used to create the symbol.

    let s = Symbol.for('event');
    let description = Symbol.keyFor(s);
    console.log(description); // outputs "event"

Symbols are useful when you need to access a property on an object without knowing the key of the property.

    let article = {
      title: 'Whiteface Mountain',
      [Symbol.for('article')]: 'My Article'
    };
    
    let value = article[Symbol.for('article')];
    console.log(value); // outputs "My Article"

Symbols used as property  of objects aren't like normal properties.

    let article = {
      title: 'Whiteface Mountain',
      [Symbol.for('article')]: 'My Article'
    };
    console.log( Object.getOwnPropertyNames(article));
    // outputs ['title']
    console.log( Object.getOwnPropertySymbols(article));
    // outputs [Symbol(article)]    

## Well-known Symbols

There are several well-known symbols for meta programming.

### MDN Reference

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol

### toStringTag

Normally, calling `toString()` isn't very helpful.

    let Blog = function() {};
    let blog = new Blog();
    console.log( blog.toString() ); // outputs [object object]

The `toStringTag` is added directly to the prototype.

    let Blog = function() {};
    Blog.prototype[Symbol.toStringTag] = 'Blog Class';
    let blog = new Blog();
    console.log( blog.toString() ); // outputs "[object Blog Class]"

### isConcatSpreadable

Normally, `Array.concat` spreads out array values.

    let values = [8, 12, 16];
    console.log([].concat(values));
    // outputs "[8, 12, 16]"

Setting `isConcatSpreadable` prevents this behavior.

    let values = [8, 12, 16];
    values[Symbol.isConcatSpreadable] = false;
    console.log([].concat(values));
    // outputs "[ [8, 12, 16] ]"

### toPrimitive

    let values = [8, 12, 16];
    let sum = values + 100;
    console.log(sum);
    // outputs 8, 12, 16100

`toPrimitive` lets you define a function that is called when an object's value is evaluated.

    let values = [8, 12, 16];
    values[Symbol.toPrimitive] = function (hint) {
      console.log(hint);
      return 42;
    }
    let sum = values + 100;
    console.log(sum);
    // outputs
    // default
    // 142

# Object Extensions

## Object.setPrototypeOf

`setPrototypeOf` assigns the prototype of an object to that of the second object.

    let a = {
      x: 1
    };

    let b = {
      y: 2
    }

    Object.setPrototypeOf(a, b);
    console.log(a.y);
    // outputs "2"

## Object.assign

`assign` populates a target with the properties of other arguments passed as parameters.

    let a = { a: 1 }, b = { b: 2 };

    let target = {};
    Object.assign(target, a, b);
    console.log(target);
    // outputs "{a: 1, b: 2}"

Properties with the same key will be overridden by subsequent properties.

    let a = { a: 1 }, b = { a: 5, b: 2 };

    let target = {};
    Object.assign(target, a, b);
    console.log(target);
    // outputs "{a: 5, b: 2}"

`Object.assign` requires properties on objects to be enumerable.

    let a = { a: 1 }, b = { a: 5, b: 2 };

    Object.defineProperty(b, 'c', {
      value: 10,
      enumerable: false
    });

    let target = {};
    Object.assign(target, a, b);
    console.log(target);
    // outputs "{a: 5, b: 2}"

`Object.assign` doesn't walk up the prototype chain.

    let a = { a: 1 }, b = { a: 5, b: 2 }, c = { c: 20 };

    Object.setPrototypeOf(b, c);

    let target = {};
    Object.assign(target, a, b);
    console.log(target);
    // outputs "{a: 5, b: 2}"

## Object.is

`Object.is` compares if two objects are exactly equal to each other.

    let amount = NaN;
    console.log(amount === amount);
    // outputs "false"
    console.log(Object.is(amount, amount));
    // outputs "true"

`Object.is` fixes some of the more confusing rules of `===` comparison.

    let amount = 0, total = -0;
    console.log(amount === total);
    // outputs "true"
    console.log(Object.is(amount, total));
    // outputs "false"

## Object.getOwnPropertySymbols

`Object.getOwnPropertySymbols` is a static function that returns an array of property symbols on an object.

    let article = {
      title: 'Whiteface Mountain',
      [Symbol.for('article')]: 'My Article'
    }

    console.log(Object.getOwnPropertySymbols(article));
    // outputs "[ Symbol(article) ]"

# String Extensions

## startsWith

`startsWith` returns true if a string starts with a given string.

    let title = 'Santa Barbara Surf Riders';
    console.log(title.startsWith('Santa'));
    // outputs "true"

## endsWith

`endsWith` returns true if a string ends with a given string.

    let title = 'Santa Barbara Surf Riders';
    console.log(title.endsWith('Surf'));
    // outputs "false"

## includes

`includes` returns true if a string contains the given string.

    let title = 'Santa Barbara Surf Riders';
    console.log(title.includes('ba'));
    // outputs "true"

## Unicode Code Points

Unicode code points now support "Astral Plane Values" (unicode characters with more than 4 hex values).

    var title = "Surfer's \u{1f3c4} Blog";
    console.log(title);
    // outputs "Surfer's 🏄 Blog"

## normalize

Using unicode code points can sometimes lead to unusual length values assigned to strings. `normalize` will help correct this.

    var title = "Mazatla\u0301n";
    console.log(title + " " + title.length);
    // outputs "Mazatlán 9"
    console.log(title + " " + title.normalize().length);
    // outputs "Mazatlán 8"

## codePointAt

    var title = "Mazatla\u0301n";
    console.log(title.normalize().codePointAt(7).toString(16));
    // outputs "6e" (the hex value for á)

## fromCodePoint

`fromCodePoint` will set a string from a hex value.

    console.log(String.fromCodePoint(0x1f3c4));
    // outputs "🏄"

## raw

`String.raw` performs interpolation without processing any other characters (such as code points and new lines) in the string.

    let title = 'Surfer';
    let output = String.raw`${title} \u{1f3c4}\n`
    // outputs "Surfer \u{1f3c4}\n"

## repeat

`String.repeat` repeats a string some number of times.

    let wave = '\u{1f30a}';
    console.log(wave.repeat(10));
    // outputs "🌊🌊🌊🌊🌊🌊🌊🌊🌊🌊"

# Number Extensions

## parseInt

`parseInt` has been moved to a function on the Number type. While it still exists on the global namespace, you should switch to the Number variant as they are identical.

    console.log(Number.parseInt === parseInt); // outputs "true"

## parseFloat

`parseFloat` has been moved as well.

    console.log(Number.parseFloat === parseFloat); // outputs "true"

## isNaN

The behavior of `isNaN` is slightly different now, and has been moved to the Number type.

    let s = 'NaN'; // this is a string, not a true NaN
    console.log(isNaN(s)); // outputs "true"
    console.log(Number.isNaN(s)); // outputs "false"

The global version of the function will convert the string prior to evaluation.

## isFinite

The behavior of `isFinite` is slightly different now, and has been moved to the Number type.

    let s = '8000'; // this is a string, not a number
    console.log(isFinite(s)); // outputs "true"
    console.log(Number.isFinite(s)); // outputs "false"

The global version of the function will convert the string prior to evaluation.

## isInteger

`Number.isInteger` evaluates numbers and returns a boolean value if they are integers.

    let sum = 408.2;
    console.log(Number.isInteger(sum)); // returns "false"

Additional test cases:

    console.log(Number.isInteger(3)); // returns "true"
    console.log(Number.isInteger(NaN)); // returns "false"
    console.log(Number.isInteger(Infinity)); // returns "false"
    console.log(Number.isInteger(undefined)); // returns "false"

## isSafeInteger

`Number.isSafeInteger` evaluates a number and returns a boolean value if the number can be accurately represented using floating point notation. Remember, floating point values lose precision when very large.

    let a = Math.pow(2, 53) - 1;
    console.log(Number.isSafeInteger(a)); // outputs "true"
    a = Math.pow(2, 53);
    console.log(Number.isSafeInteger(a)); // outputs "false"

This implies that the largest safe integer that can be stored is (2 ^ 53) - 1 or 9,007,199,254,740,991. This also happens to be the value of `Number.MAX_SAFE_INTEGER` static property (see below).

## Number Constants

There are three new constants added to the Number type.

    console.log(Number.EPSILON); // outputs "2.220446049250313e-16"
    console.log(Number.MAX_SAFE_INTEGER); // outputs "9007199254740991"
    console.log(Number.MIN_SAFE_INTEGER); // outputs "-9007199254740991"

# Math Extensions

## Hyperbolic Functions

New hyperbolic functions:

    cosh()
    acosh()
    sinh()
    asinh()
    tanh()
    atanh()
    hypot()

## Artithmetic Functions

New arithmetic functions:

    cbrt()    cube root
    clz32()   count leading zeros (32 bit integers)
    expm1()   equal to exp(x) - 1
    log2()    log base 2
    log10()   log base 10
    log1p()   equal to log(x+1)
    imul()    32 bit integer multiplication

### cbrt

`cbrt` examples:

    console.log(Math.cbrt(27));  // outputs "3"

## Miscellaneous Functions

Other functions:

    sign()    the number's sigh: 1, -1, 0, -0, NaN
    trunc()   the integer part of a number, similar to floor
    fround()  round to the nearest 32 bit floating point value

### sign

`sign` examples:

    console.log(sign(0));  // outputs "0"
    console.log(sign(-0));  // outputs "-0 (0 in Microsoft's Edge browser)"
    console.log(sign(-20));  // outputs "-1"
    console.log(sign(20));  // outputs "1"
    console.log(sign(NaN));  // outputs "NaN"

### trunc

`truncate` examples:

    console.log(Math.trunc(27.1));  // outputs 27
    console.log(Math.trunc(-27.9));  // outputs -27

# RegExp Extensions

## Astral Plane Unicode Characters

Regular expressions now work with the new Astral Plane unicode characters. You need to pass additional arguments.

    let pattern = /\u{1f3c4}/;
    console.log(pattern.test('🏄')); // outputs "false"

Append `u` to the end of the pattern to explicitly specify astral plane unicode characters.

    let pattern = /\u{1f3c4}/u;
    console.log(pattern.test('🏄')); // outputs "true"

Another example:

    let pattern = /^.Surfer/;
    console.log(pattern.test('🏄Surfer')); // outputs "false"
    
In es5, astral plane characters are actually length 2:

    let pattern = /^.Surfer/u;
    console.log(pattern.test('🏄Surfer')); // outputs "true"
    
## The "sticky" property

The `sticky` property reflects whether or not the search is sticky, meaning that searches start at the index specified by the `lastIndex` property. By default, `pattern.lastIndex` is `0`.

    let pattern = /900/y;
    console.log(pattern.lastIndex);  // outputs "0"
    console.log(pattern.test('800900'));  // outputs "false"

You can manually set `property.lastIndex`:

    let pattern = /900/y;
    pattern.lastIndex = 3;
    console.log(pattern.lastIndex);  // outputs "3"
    console.log(pattern.test('800900'));  // outputs "true"

## Flags

There's a new property on the RegExp object called `flags`, which holds the value of all flags added to the regular expression.

    let pattern = /900/yg;
    console.log(pattern.flags); // outputs "gy"

The order here is important. `pattern.flags` will always return flags in the order "gimuy".

# Function Extensions

## Function.name

`Function.name` is a new property that holds the name of the function:

    let fn = function calc() {
      return 0;
    };
    console.log(fn.name); // outputs "calc"

In the case of anonymous function expressions, the name is assigned as the name of the variable the function is assigned to.

    let fn = function() {
      return 0;
    };
    console.log(fn.name); // outputs "fn"

Another example:

    let fn = function() {
      return 0;
    };
    let newFn = fn;
    console.log(newFn.name); // outputs "fn"

This works on classes and class methods, too:

    class Calculator {
      constructor() {
      }
      add() {
      }
    }
    let c = new Calculator();
    console.log(Calculator.name); // outputs "Calculator"
    console.log(c.add.name); // outputs "add"

`Function.name` is not writable, but is configurable with the `Object.defineProperty()` method.

