# Experiences and notes during my programming career

## Thinking

* If you are stuck, rest.

* Always hard code and brute force first. Never generalize too soon, never optimize too soon.

* If you feel your solution is complicated, it means that your solution is wrong.

* Whenever you want to manipulate (find, judge, extract, edit) patterns in a string, use [regex](https://github.com/ziishaned/learn-regex). Test your regex using this [tool](https://regex101.com/r/dmRygT/1).

## Javascript

* At least, learn Javascript properly. Read the following material from left to right: [Javascript Express](https://www.javascript.express), [A re-introduction to JavaScript (JS tutorial)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/A_re-introduction_to_JavaScript), [You-Dont-Know-JS](https://github.com/getify/You-Dont-Know-JS)

* Use [lodash](https://lodash.com/docs/4.17.15) to manipulate arrays and collections

* Spread operator cannot fully copy nested arrays or nested objects. Follow this [instruction](https://stackoverflow.com/questions/122102/what-is-the-most-efficient-way-to-deep-clone-an-object-in-javascript) for deep cloning.

## Cypress

* Cypress has adopted Mocha's bdd syntax.

* Passing arrow functions to Mocha and Cypress is [discouraged](https://mochajs.org/#arrow-functions).

## React

* Always set default value for state parameter of a reducer, otherwise, the state type will be "never" in app state (generated according to rootReducer)

* Follow [this article](https://www.robinwieruch.de/react-hooks-fetch-data) to optimize data fetching with `useEffect` hooks.

### Don't mutate state, replace it

* Use `Array.concat()` to add an item to an array rather than `Array.push()`, because `Array.concat()` does not mutate the array, but creates a new array in which the content of the old array and the new item are both included.

* Here is how you should update an item in an array (item is identified by id):

```javascript
const updateNoteContentById = (id, newContent) => {
  const note = notes.find(n => n.id === id)
  const changedNote = { ...note, content: newContent }
  setNotes(notes.map(note => note.id !== id ? note : changedNote))
}
```

* You should use `Array.filter()` to delete an item in an array. Never ever use `pop()`.

```javascript
const deleteNoteById = id => {
    const note = notes.find(n => n.id === id)
    setNotes(notes.filter(n => n.id !== id))
}
```

## NodeJS

* Use [express-generator-typescript](https://www.npmjs.com/package/express-generator-typescript) to generate a basic NodeJS template with TypeScript.

* Use [bcrypt](https://github.com/kelektiv/node.bcrypt.js) to generate password hashes.

* Use [express-async-error](https://github.com/davidbanham/express-async-errors) to eliminate try/catch blocks. Because of the library, we do not need the next(exception) call anymore. The library handles everything under the hood. If an exception occurs in an async route, the execution is automatically passed to the error handling middleware.

## React Unit Testing

* [How to Write Unit Tests for Asynchronous Redux Thunk](https://decembersoft.com/posts/how-to-unit-test-redux-thunks/)

* Use `useEvent` instead of `fireEvent`. Install `useEvent` [here](https://testing-library.com/docs/ecosystem-user-event/).

* [Here](https://kentcdodds.com/blog/fix-the-not-wrapped-in-act-warning) and [here](https://testing-library.com/docs/guide-disappearance/) is how to fix the "not wrapped in act(...)" warning.

* Prefer `testing-library` over `enzyme`.

* Get image by role `img` and alt text. For example:
```javascript
const toppingImages = screen.getByRole("img", {
    // text match: "topping" should be the end of the alt text
    name: /topping$/i,
});
```
* Get an alert by its role only. Don't find alerts by name.
```javascript
// This works
const alert = await screen.findByRole("alert");
// This doesn't work
const alert = await screen.findByRole("alert", {
    name: /an unexpected error/i
});
```
## C programming
### Memory leak
* Memory leak means that you lose the location of the allocated memory and you'll never able to free it anymore. The best way to combat memory leaks is to make sure that you always free allocated memory at the end of the pointer's scope, for example the end of a function.
* A dangling pointer is a pointer that points to a memory region that was previously deallocated. Unfortunately, having a pointer that points to a now freed memory region will surely cause problems, if used. To avoid problems from dangling pointer, check every single pointer in your program against NULL.
### Pointers
* You can assign any address to void pointer `void *ptr`. However, you cannot dereference a void pointer.
* When you pass `a` as a parameter to the function `fct`, function `fct` receives a **clone** of `a`. Turns out, `fct` only modify a clone of `a`, but not `a` itself. In other words, the following code will print `42`.
```c
#include <stdio.h>
void    fct(int num)
{
        num = 3;
}
int     main(void)
{
        int a;
        
        a = 42;
        fct(a);
        printf("a = %d", a);
        return (0);
}
```
If you want to modify value of a variable inside a function, use pointer. In the following code, `fct` receives a clone of the address of `a`. Inside `fct`, we deference to the memory address of `a` and then modify the value inside that memory address. So in the end, we modify the value of `a`.
```c
#include <stdio.h>
void    fct(int *ptr)
{
        *ptr = 3;
}
int     main(void)
{
        int a;
        
        a = 42;
        fct(&a);
        printf("a = %d", a);
        return (0);
}
```
* Given `int tab[10]`, `tab` is the pointer to the first element in the array. 
* A string is a series of bytes. Each byte contains a character. The last byte must be a null character `\0`.
* When creating a string MANUALLY, remember to put `\0` at the end.
* With `cat -e` linux command, the `\0` character will be displayed as `@^`.
* The following code will either not compile at all, or compile but crash. Declaring this way means that the memory area of the string (char array) is immutable
```c
char *str;

str = "toto";
str[0] = 'p';
```
* The following code will compile normally.
```c
char str[] = "toto";

str[0] = 'p';
```
