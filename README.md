# Experiences and notes during my programming career
## Thinking
* If you are stuck, rest.
* Always hard code and brute force first. Never generalize too soon, never optimize too soon.
* If you feel your solution is complicated, it means that your solution is wrong.
## Javascript
* Use [lodash](https://lodash.com/docs/4.17.15) to manipulate arrays and collections
* Spread operator cannot fully copy nested arrays or nested objects. Follow this [instruction](https://stackoverflow.com/questions/122102/what-is-the-most-efficient-way-to-deep-clone-an-object-in-javascript) for deep cloning.
* Here is how to update an object in an array by ID
```javascript
list.map(item => {
        if (item.id === id) {
            item.title = "new title";
            item.author = "new author";
        }
        return item;
    }
)
```
## React
* Always set default value for state parameter of a reducer, otherwise, the state type will be "never" in app state (generated according to rootReducer)
* Follow [this article](https://www.robinwieruch.de/react-hooks-fetch-data) to optimize data fetching with `useEffect` hooks
## NodeJS
* Use [express-generator-typescript](https://www.npmjs.com/package/express-generator-typescript) to generate a basic NodeJS template with TypeScript.

## React Testing
* [How to Write Unit Tests for Asynchronous Redux Thunk](https://decembersoft.com/posts/how-to-unit-test-redux-thunks/)
* Use `useEvent` instead of `fireEvent`. Install `useEvent` [here](https://testing-library.com/docs/ecosystem-user-event/).
* [Here](https://kentcdodds.com/blog/fix-the-not-wrapped-in-act-warning) and [here](https://testing-library.com/docs/guide-disappearance/) is how to fix the "not wrapped in act(...)" warning.
* Prefer `testing-library` over `enzyme`.
* Queries in `testing-library`:
    + `getBy...`: assert elements that are supposed to be present.
    + `queryBy...`: assert elements that are supposed NOT to be present.
    + `findBy...`: use `await findBy...` to assert elements that are async (elements that depend on network data, IO)
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
