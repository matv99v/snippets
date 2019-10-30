# Prototypal inheritance in javascript, OOP approaches

### What would be the best way to store data and functionality?

```javascript
const user1 = {
  name: "Will",
  score: 3,
  increment: function() {
    user1.score++;
  },
};
user1.increment(); // user1.score => 4
```

### Solution 1. Generate objects using a function
Each time we create a new user we make space in our computer's memory for all our data and functions. But our functions are just copies.

```javascript
function userCreator(name, score) {
  let newUser = {};
  newUser.name = name;
  newUser.score = score;
  newUser.increment = function() {
    newUser.score++;
  };
  return newUser;
};

let user1 = userCreator("Will", 3);
let user2 = userCreator("Tim", 5);
user1.increment();
user2.increment();
```


### Solution 2. Using the prototypal nature of JavaScript
Store the increment function in just one object and have the interpreter, if it doesn't find the function on user1, look up to that object to check if it's there.

```javascript
function userCreator (name, score) {
  let newUser = Object.create(userFunctionStore);
  newUser.name = name;
  newUser.score = score;
  return newUser;
};

const userFunctionStore = {
  increment: function() { this.score++; },
  login: function() { console.log("You're loggedin"); }
};

const user1 = userCreator("Will", 3);
const user2 = userCreator("Tim", 5);
user1.increment();
```

### Solution 3. Introduce magic keyword `new`
When we call the constructor function with new in front we automate 2 things:
1. Create a new user object
2. Return the new user object

```javascript
function User(name, score) {
  this.name = name;
  this.score = score;
}

User.prototype.increment = function() {
  this.score++;
};
User.prototype.login = function() {
  console.log("login");
};

const user1 = new User(“Eva”, 9);
user1.increment();
```

### Solution 4. The class `syntactic sugar`
We’re writing our shared methods separately from our object `constructor`  itself (off in the User.prototype object). Other languages let us do this all  in one place. ES2015 lets us do so too.

```javascript
class User {
  constructor (name, score){
    this.name = name;
    this.score = score;
  }
  increment (){
    this.score++;
  }
  login (){
    console.log("login");
  }
}

const user1 = new User("Eva", 9);
user1.increment();
```
