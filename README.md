# Experiences and notes during my programming career
## React
* Always set default value for state parameter of a reducer, otherwise, the state type will be "never" in app state (generated according to rootReducer)
* Follow [this article](https://www.robinwieruch.de/react-hooks-fetch-data) to optimize data fetching with `useEffect` hooks
## NodeJS
* Use [express-generator-typescript](https://www.npmjs.com/package/express-generator-typescript) to generate a basic NodeJS template with TypeScript.
* Use [lodash](https://lodash.com/docs/4.17.15) to manipulate arrays and collections
## Testing
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
