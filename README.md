# Experiences and notes during my programming career

## Thinking

* If you are stuck, rest.
* Losing your sanity is almost guaranteed if you leave the application in a completely broken state.
* Always hard code and brute force first. Never generalize too soon, never optimize too soon.

* If you feel your solution is complicated, it means that your solution is wrong.

* Whenever you want to manipulate (find, judge, extract, edit) patterns in a string, use [regex](https://github.com/ziishaned/learn-regex). Test your regex using this [tool](https://regex101.com/r/dmRygT/1).

## Refactor

* Refactoring of existing code is a common reality of extending existing applications. Refactoring is an important and necessary skill even if it may feel difficult and unpleasant at times.
* Always take baby steps. A baby step is a small change that doesn't break app functionality.

## Working with an existing codebase

* You can start your research by reading the README.md in the root of the repository. Usually, the README contains a brief description of the application and the requirements for using it, as well as how to start it for development.
* Take a peek at `package.json`.
* Start the application and click around to verify you have a functional development environment.
* If the project has unit, integration or end-to-end tests, reading those is most likely beneficial.
* You can also browse the folder structure to get some insight into the application's functionality and/or the architecture used. These are not always clear, and the developers might have chosen a way to organize code that is not familiar to you.
* Do remember that reading code is a skill in itself, and don't worry if you don't understand the code on your first readthrough.

## Troubleshoot

### Search error message

* Search for the error message in Google and Stack Overflow, it's likely you're not the first person to ever run into this
* Isolate the code that's throwing the error (next section). This should narrow down the possible sources of the error, and provide you with more information to search the internet for others who have had the same problem.

### Isolate the code that's throwing the error

* Revert back to a working version of your code
* Apply your recent changes piece by piece, until it breaks
* If your app is too large and complex to do that, try and extract the functionality you're trying to add to its own blank project, and go from there.

## Javascript

* At least, learn Javascript properly. Read the following material from left to right: [Javascript Express](https://www.javascript.express), [A re-introduction to JavaScript (JS tutorial)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/A_re-introduction_to_JavaScript), [You-Dont-Know-JS](https://github.com/getify/You-Dont-Know-JS)

* Use [lodash](https://lodash.com) to manipulate arrays and collections

* In order to avoid time-eating bugs, it is recommended that within a flat directory, each file has a unique filename regardless of its extension. For example, if there is a `myFile.ts`, there should not be a `myFile.json`.

* Spread operator cannot fully copy nested arrays or nested objects. Follow this [instruction](https://stackoverflow.com/questions/122102/what-is-the-most-efficient-way-to-deep-clone-an-object-in-javascript) for deep cloning.

## Typescript

* Usually, types for existing packages can be found from the @types organization within npm, and you can add the relevant types to your project by installing an npm package with the name of your package with a @types/prefix. For example: `npm install --save-dev @types/react`. Since the typings are only used before compilation, the typings are not needed in the production build and they should always be in the `devDependencies` of the `package.json`.

* TypeScript only checks whether we have all of the required fields or not, but excess fields are not prohibited. Unfortunately, this can lead to unwanted behaviour if you are not aware of what you are doing; the situation is valid as far as TypeScript is concerned, but you are most likely allowing use that is not wanted.

* Below is recommended TSConfig. More details can be found on [Intro to TSConfig Reference](https://www.staging-typescript.org/tsconfig#strict)

```
{
  "compilerOptions": {
    "target": "ES6",
    "outDir": "./build/",
    "module": "commonjs",
    "strict": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "noImplicitReturns": true,
    "noFallthroughCasesInSwitch": true,
    "esModuleInterop": true
  }
}
```

* Install ESLint and its TypeScript extention

```
npm install --save-dev eslint @typescript-eslint/eslint-plugin @typescript-eslint/parser
```
* We should never use type assertion unless there is no other way to proceed, as there is always the danger we assert an unfit type to an object and cause a nasty runtime error.

* Passing a type parameter to Axios will not validate any data. It is quite dangerous especially if you are using external APIs. You can create custom validation functions which take in the whole payload and return the correct type, or you can use a type guard.

* At the moment of writing, `Omit` does not work well with union type. For example, let say `Entry` is a union type of `LowEntry`, `HighEntry` and `ImportantEntry`. We want to remove `id` from `Entry` by using `Omit<Entry, 'id'>`, but it would not work as expected. In fact, `Omit<Entry, 'id'>` only contains properties that are shared among the union members. More details about this bug [here](https://github.com/microsoft/TypeScript/issues/42680).

* Follow this [blog](https://kentcdodds.com/blog/get-a-catch-block-error-message-with-typescript) to get a catch block error message.

## Cypress

* Cypress has adopted Mocha's bdd syntax.

* Passing arrow functions to Mocha and Cypress is [discouraged](https://mochajs.org/#arrow-functions).

## React

* Always set default value for state parameter of a reducer, otherwise, the state type will be "never" in app state (generated according to rootReducer)

* Follow [this article](https://www.robinwieruch.de/react-hooks-fetch-data) to optimize data fetching with `useEffect` hooks.
* The `npm audit` command can be used to check the security of dependencies. It compares the version numbers of the dependencies in your application to a list of the version numbers of dependencies containing known security threats in a centralized error database.

### Don't mutate state

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
### Unit Testing

* [How to Write Unit Tests for Asynchronous Redux Thunk](https://decembersoft.com/posts/how-to-unit-test-redux-thunks/)
* Use [deep-freeze](https://github.com/substack/deep-freeze) to ensure that the reducer doesn't change state internally.
```javascript
const state = []
const action = {
  type: 'NEW_NOTE',
  data: {
    content: 'the app state is in redux store',
    important: true,
    id: 1
  }
}

deepFreeze(state)
const newState = noteReducer(state, action)

expect(newState).toHaveLength(1)
expect(newState).toContainEqual(action.data)
```
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

### Hooks

Below are some usefull custom hooks:

* `useField` hook simplifies input management. Use spread syntax and `getInput` method to pass properties to `<input/>`. The `getInput` method extracts `reset` property from returned object of `userField`. Without `getInput` method, a warning is thrown: `Invalid value for prop 'reset' on  <input/> tag`

```javascript
const useField = (name, type) => {
        const [value, setValue] = useState('')

        const onChange = (event) => {
                setValue(event.target.value)
        }

        const reset = () => setValue('')
        
        const inputProps = {
          name, value, type, onChange
        }

        return { name, value, onChange, type, reset, inputProps }
}

const content = useField('content', 'text')
<form onSubmit={handleSubmit}>
  <div>
    content
    <input { ...content.inputProps } />
  </div>
  <button type='submit'>create</button>
</form>
```

## NodeJS

* Use [express-generator-typescript](https://www.npmjs.com/package/express-generator-typescript) to generate a basic NodeJS template with TypeScript.

* Use [bcrypt](https://github.com/kelektiv/node.bcrypt.js) to generate password hashes.

* Use [express-async-error](https://github.com/davidbanham/express-async-errors) to eliminate try/catch blocks. Because of the library, we do not need the next(exception) call anymore. The library handles everything under the hood. If an exception occurs in an async route, the execution is automatically passed to the error handling middleware.

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

## Android

### Jetpack Compose

* If an item is out of the viewport upon scrolling, any state inside that item will be lost. Use `rememberSavable` to remember the state.
* The `remember` function works only as long as the composable is kept in the Composition. When you rotate, the whole activity is restarted so all state is lost. This also happens with any configuration change and on process death.
* `LazyColumn` doesn't recycle its children like `RecyclerView`. It emits new Composables as you scroll through it and is still performant, as emitting Composables is relatively cheap compared to instantiating Android Views.
* Always use `Column()` or `Row()` to wrap around `Icon()` for larger clickable area
```
Column(
    modifier = Modifier
        .align(Alignment.CenterVertically)
        .clickable {
            handleClickEvent()
        }
) {
    Icon(
        imageVector = Icons.Filled.MoreVert,
        contentDescription = "Filter Icon"
    )
}
```
