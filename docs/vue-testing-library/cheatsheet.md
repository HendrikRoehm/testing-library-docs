---
id: cheatsheet
title: Cheatsheet
---

A short guide to all the exported functions in `Vue Testing Library`.

## Queries

> **Difference from DOM Testing Library**
>
> The queries returned from `render` in `Vue Testing Library` are the same as
> `DOM Testing Library`. Yet, they have the first argument bound to the
> document, so instead of `getByText(node, 'text')` you write
> `getByText('text')`.

### Search variants

|                   | Return if no match | Return if 1 match | Return if 1+ match | Await? |
| ----------------- | ------------------ | ----------------- | ------------------ | ------ |
| **getBy...**      | throw              | return            | throw              | No     |
| **findBy...**     | throw              | return            | throw              | Yes    |
| **queryBy...**    | `null`             | return            | throw              | No     |
| **getAllBy...**   | throw              | array             | array              | No     |
| **findAllBy...**  | throw              | array             | array              | Yes    |
| **queryAllBy...** | `[]`               | array             | array              | No     |

### Search types

|                          | finds by...                      | DOM example                           |
| ------------------------ | -------------------------------- | ------------------------------------- |
| **...ByLabelText**       | label or aria-label content      | `<label for="element" />`             |
| **...ByPlaceholderText** | input placeholder value          | `<input placeholder="name" />`        |
| **...ByText**            | element text content             | `<p>Lorem ipsum</p>`                  |
| **...ByDisplayValue**    | form element current value       | Current value of input element        |
| **...ByAltText**         | img alt attribute                | `<img alt="movie poster" />`          |
| **...ByTitle**           | title attribute or svg title tag | `<span title="Add" />` or `<title />` |
| **...ByRole**            | ARIA role                        | `<div role="dialog" />`               |
| **...ByTestId**          | data-testid attribute            | `<div data-testid="some-message" />`  |

> You can write any combination of Search variants and Search types.

### An example

`getByLabelText('Username')` will search for a `<label>` element that contains
the string "Username", and will return the associated `input`. In case of not
finding any, or finding more than one, it will throw an error.

`queryAllByRole('nav')` will synchronously look for all elements with a
`role="nav"` attribute, and return an array with the results (or an empty array
if no results were found).

For more information, see [Which query should I use?](guide-which-query.md).

## Async utilities

- **wait** (Promise) retry function within until it stops throwing or times out.
- **waitForElement** (Promise) retry function or array of functions and return
  the result.
- `findBy` and `findAllBy` queries are async and retry until either a timeout or
  if the query returns successfully; they wrap `waitForElement`.

For more information, see
[DOM Testing Library Async API](dom-testing-library/api-async.md).

> Remember to `await` or `.then()` the result of async functions in your tests!

## Firing events

- **fireEvent()** trigger DOM event: `fireEvent(node, event)`
- **fireEvent.\*** helpers for default event types
  - **click** `fireEvent.click(node)`
  - **input** `fireEvent.input(node, event)`
  - [See all supported events](https://github.com/testing-library/dom-testing-library/blob/master/src/events.js)

For more information, see [Events API](dom-testing-library/api-events.md)

> **Difference from DOM Testing Library**
>
> The events returned from `Vue Testing Library` are all async, so you should
> `await` or `then()` the result.
>
> VTL also exposes `fireEvent.update(node, value)` event to deal with `v-model`.
> See [the API](vue-testing-library/api#updateelem-value) for more details.

## Other

- **within(node)** takes a node and returns an object with all the queries bound
  to it: `within(getByTestId("global-header")).getByText("hello")`.
- **configure(config)** change global options:
  `configure({testIdAttribute: 'my-test-id'})`.
- **cleanup()** clears the DOM ([use with `afterEach`](setup.md#cleanup) to
  reset DOM between tests).

For more information, see [Helpers API](dom-testing-library/api-helpers.md) and
[Config API](dom-testing-library/api-configuration.md).

## Text Match Options

Given the following HTML:

```html
<div>Hello World</div>
```

All these matchers **will find the element:**

```javascript
// Matching a string:
getByText('Hello World') // full string match
getByText('llo Worl', { exact: false }) // substring match
getByText('hello world', { exact: false }) // ignore case

// Matching a regex:
getByText(/World/) // substring match
getByText(/world/i) // substring match, ignore case
getByText(/^hello world$/i) // full string match, ignore case
getByText(/Hello W?oRlD/i) // advanced regex

// Matching with a custom function:
getByText((content, element) => content.startsWith('Hello'))
```
