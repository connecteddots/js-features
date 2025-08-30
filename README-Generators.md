

## Function vs Generators

### Functions
Like many other languages __functions__ are reusable code to perform some action and  avoid repeatition. 

```javascript
function greeting(name){
    const message = `hello ${name}!`;
    console.log(message);
}

greeting("shoaib");
```

When above function __greeting__ is called control is transferred to that function first statement and interpreter start executing statements until last statement and control is returned to caller.

### Generators
Generator is a different kind of function to handle situation when our function doesn't need to execute fully, but partially and return control to caller until caller call it again for next result. lets have a look on it.

```javascript
function* makeBreakfast(){
    yield "Make Omelate"
    yield "Make Tea"
    yield "Set the Table"
    yield "Serve Breakfast"
}

//Method 1
let breakfast = makeBreakfast();
console.log(breakfast.next()) // {value:"Make Omelate", done:false}
console.log(breakfast.next()) // {value:"Make Tea", done:false}
console.log(breakfast.next()) // {value:"Make Set the Table", done:false}
console.log(breakfast.next()) // {value:"Serve Breakfast", done:false}
console.log(breakfast.next()) // {value:undefined, done:true}

// Method 2
for(const step of makeBreakfast){
    console.log(step)
}
```
In above function makeBreakfast ${\color{yellow}*}$ is used to distinguished generators from normal functions. We use keyword ${\color{yellow}yield}$ to produce value for caller.

When caller use expression like ${\color{yellow}makeBreakfast()}$ javascript engine return a generator object whose state can not reset. This object expose some functions like __next__ but does not execute anything until we call ${\color{yellow}next()}$ on that object.

When we execute __next()__ first time generator function will execute function body until __yield__ is found or end of function reached.
* If Generator function has NO yield then function on first __next()__ call will execute fully and returns 
```json
{value:"function returned value", done:true}
``` 
* If Generator has some __yield__ keywords in its body then on first generator __next()__ call function body will start executing until first __yield__ keyword is reached. At this point function will produce value and return control to caller until __next()__ is called again. On first call we will see following result.
```json
{value:"make Omelate", done:false}
```
### Delegate Generators
Sometimes one generator contains other generators to produce values for that original generator. It has many beautiful usages. Among many examples one may be to produce alphanumeric code strings. e.g.

```javascript
// Demo String Generator (only for demo)
function* generateString(maxLength=6){
    yield* generateAlphabets(Math.floor(maxLength/2))
    yield* generateNumbers(Math.ceil(maxLength/2))
}

function* generateAlphabets(maxLength=3) {
    console.log(`alphabets maxLength=${maxLength}`);
    let alphabets = 'abcdefghijklmnopqrstuvwxyz'
    while((maxLength--) > 0){
        yield alphabets[Math.floor(Math.random()*alphabets.length+1)-1]
    }
}

function* generateNumbers(maxLength=3) {
    console.log(`numbers maxLength=${maxLength}`);
    let numbers = '0123456789'
    while((maxLength--) > 0){
        yield numbers[Math.floor(Math.random()*numbers.length+1)-1]
    }
}

let gen = generateString(5);
console.log([...gen].join(''))

```

In above example __Delegate Generator__ ${\color{yellow}yield}$ is replace with ${\color{yellow}yield*}$. After first inner generator execution completed second generator will execute.