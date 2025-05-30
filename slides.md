---
# You can also start simply with 'default'
theme: default
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: /assets/intro.jpg
# some information about your slides (markdown enabled)
title: Software Development | Foundations
info: |
  ## Software Development | Foundations
# apply unocss classes to the current slide
class: text-left
drawings:
  persist: false
transition: slide-left
mdc: true
---

# Unit Testing

Full-Stack Development - part 6/8

- [ ] Intro to Testing
- [ ] Writing Unit Tests
- [ ] Automated Test `npm-test`

<div class="abs-br m-6 text-xl">
  <a href="https://github.com/slidevjs/slidev" target="_blank" class="slidev-icon-btn">
    <carbon:logo-github />
  </a>
</div>

<!--
-->

---

## transition: slide-left

# Intro to Unit Testing

What is testing and why it's important in software development

- Can you think of something where we test the product before we buy the product?
- Have you ever coded a new feature, only to have something that previously worked, break?
- Have you ever spent hours debugging something, only to find it was a small typo or logic error?
- Have you ever been afraid to refactor code because you weren't sure what might break?
- If your project grew 10x in size, how would you make sure everything still worked?
- What would happen if your app broke and users noticed before you did?

What is the role of a developer when we think about testing?

- [QA Job Postings](https://ca.indeed.com/jobs?q=qa+engineer&l=Remote&from=searchOnDesktopSerp&vjk=81b75ba803f6d8e7)

---

## transition: slide-left

# Definition

- Software Testing: A process to evaluate and ensure that software works as intended
- Why?
  - Reduces bugs
  - Improves code quality
  - Supports maintainability
- Your role as a developer: Deveopers are responsible for writing reliable code, which includees creating and running tests

What are some consequences of not testing properly?

---

## transition: slide-left

# Types of Testing

- Unit Tests: Tests the smallest parts of code (ex: functions) in isolation
  - competing tools: Jest, Mocha/Chai, Vitest
- Integration Tests: Ensures that combined parts of an app work together
  - tools: Jest, Mocha/Chai, Vitest, react-testing-library, Playwright
- End-to-End Tests: Simulates real user scenarios from start to finish
  - tools: Playwright, Cypress
- Ask ChatGPT:
  - What is the most popular testing library for node apps?
  - List developer testing tools in chronological order (ex: jest, mocha chai)
- What do you think would change if you had a way to automatically test parts of your app every time you updated it?

---

## transition: slide-left

# Debugging Tools

- Debugging Express Routes:
  - use of `console.log()` for basic tracing
  - use of VS Code breakpoints within Node.js
  - understanding of request/response cycle and basic status codes
- Testing Express Routes:
  - Manually testing via Postman (simulates API calls)
  - Use of browser dev tools to inspect network calls and responses

---

## transition: slide-left

# Jest

https://jestjs.io/ - Jest is an all-in-one test runner and assertion library

- Test Runner: a tool that finds your test files, executes the test code, reports results. Usually includes functions like `describe`, `test` or `it`, `beforeEach` etc.
  ```js
  describe("feature name", () => {
    it("should add numbers correctly", () => {
      // put assertion code here
    });
  });
  ```
- Other competing software: Mocha, Jasmine, [Vitest](https://2024.stateofjs.com/en-US/awards/),
- Assertion Library: a tool used to check whether your code behaves as expected. Usually includes functions like `expect`, `assert`, `toBe`
  ```js
  it("should add numbers correctly", () => {
    expect(1 + 1).toBe(2);
  });
  ```

---

## transition: slide-left

# Setup Jest & [supertest](https://github.com/ladjs/supertest#readme)

- `npm init -y`
- `npm i -D jest @babel/core @babel/preset-env babel-jest supertest`
- Change package.json:
  ```json
   "type": "module",
   "scripts": {
     "test": "jest --silent=false", // silent=false allows me to view console.log() output
     "test:watch": "jest --watch"
   },
  ```
  - run `git init` otherwise "watch" won't work
- Create `.babelrc`
  ```json
  {
    "presets": ["@babel/preset-env"]
  }
  ```

---

## transition: slide-left

# Writing Unit Tests (pg.1)

```js
// first.test.js
describe("first test", () => {
  it("should work", () => {
    expect(true).toBe(true);
  });

  it("should be 4", () => {
    expect(1 + 1).toBe(2);
  });
});
```

- `npm run test` or `npm run test:watch`
- try breaking a test to see what happens (ex: `expect(2 + 2).toBe(5)`)
- What does [describe](https://jestjs.io/docs/api#describename-fn) do?
- What does [it](https://jestjs.io/docs/api#testname-fn-timeout) do?
- What does [expect](https://jestjs.io/docs/expect) do?
- What does [toBe](https://jestjs.io/docs/expect#tobevalue) do?
  - compare objects using toBe vs [toEqual](https://jestjs.io/docs/expect#toequalvalue)

---

## transition: slide-left

# Writing Unit Tests (pg.2)

Test Driven Development (i.e. red 🔴, green 🟢, refactor 🟢) is one style of writing unit tests.

1. Write the unit test first = `RED`

```js
// math.js
export const add = () => {};

// /tests/math.test.js
import { add } from "../math.js";

describe("add", () => {
  it("should add two numbers", () => {
    expect(add(2, 3)).toBe(5);
  });
});
```

- Step 1) 🔴 `RED`
- Step 2) 🟢 `GREEN` - implement function now to make it pass
- Step 3) 🟢 `REFACTOR` function (if needed) yet still pass
- Now you have a way to add/change features in your codebase and getting instant feedback

---

## transition: slide-left

# Writing Unit Tests (pg.3)

2. Write the function then incorporate it into test so it passes = `GREEN`

   ```js
   // math.js
   export function add(a, b) {
     return ???;
   }
   ```

   ```js
   // tests/math.test.js
   import { add } from "../math.js";

   describe("add", () => {
     it("should add two numbers", () => {
       expect(add(2, 3)).toBe(5);
     });
   });
   ```

- Tip: _Your tests pass because they didn't fail. Not because your code works_ (ex: `return 4`)

1. Refactor function if needed so it's cleaner yet stays `GREEN`

- Q: When should you write unit tests?

---

## transition: slide-left

# Exercise: Writing Unit Tests

Practice Test Driven Development

- Tip: Install VS Code extension "Jest"

1. Make functions and unit tests for:
   - `subtract()`
   - `multiple()`
   - `divide()`

---

## transition: slide-left

# Testing for Invalid Input (pg.1)

- What happens if your functions are called with arguments that invalid? ex: add('1', '2')

  ```js
  // math.js
  export const add = (a, b) => {
    if (typeof a === "string") a = Number(a);
    if (typeof b === "string") b = Number(b);
    return a + b;
  };

  // /tests/math.test.js
  it("should cast string numerals into numbers", () => {
    expect(add("1", "2")).toBe(3);
  });
  ```

  - But what happens if you call it with `(add(2, 'potato')`?

---

## transition: slide-left

# Testing for Invalid Input (pg.2)

- What happens if you call it with `(add(2, 'potato')`?
- How would you write the test?
- Would you expect it to throw an error or not? (depends on what you want to accomplish. No right answer here)

```js
// math.js
export const add = (a, b) => {
  if (typeof a === "string") a = Number(a);
  if (typeof b === "string") b = Number(b);

  if (isNaN(a)) throw new Error("the first argument is not a number");
  if (isNaN(b)) throw new Error("the second argument is not a number");

  return a + b;
};

// /tests/math.test.js
it("should cast string numerals into numbers", () => {
  expect(() => add(2, "potato").toThrow()); // note we had to wrap with expect()
});
```

---

## transition: slide-left

# Exercise: Writing Unit Tests for Edge Cases

- Refactor your unit tests to take into account some of these edge cases
- Here are some examples of invalid inputs that you may want to take into account in your unit tests:
  - `boolean`, `string`, `number`
  - `undefined`, `null`, `NaN`
  - array, object, function
  - Promise

---

layout: image-right
transition: slide-left
image: /assets/kent.png
backgroundSize: 400px 250px
class: text-left

---

# 10 minute break

🍦 Cool Tips, Trends and Resources:

- 🏗️ [Load Balancing](https://samwho.dev/load-balancing/)
- 💾 [Tech Documentaries](https://www.techdocumentaries.com/#2)
- 🎨 [Interactive SVG Reference](https://www.fffuel.co/sssvg/)
- 🧑‍🎨 [Toools Design Resources](https://www.toools.design/?ref=dailydev)
- ✋ [Stop Lying to Your Users](https://www.epicweb.dev/stop-lying-to-your-users)

<br>
<hr>
<br>

- 🧪 [Enter anonymous lab questions](https://docs.google.com/forms/d/e/1FAIpQLSevvGARdHQikso-uLqFCO481MABKE5HofuSrlzEPMNQ2ZLykw/viewform?usp=dialog)
- ℹ️ [Course feedback survey](https://circuitstream.typeform.com/to/ZoyYk7px#course_id=SoftwareAN&instructor=9514)

<!--
- take attendance
-->

---

## transition: slide-left

# Unit Testing Async Functions (pg.1)

- try the following, does this pass?:
  ```js
  it("should this async code pass?", () => {
    setTimeout(() => {
      expect(false).toBe(true);
    }, 1000);
  });
  ```
  - let's put this timeout in an async function, then write a unit test

---

## transition: slide-left

# Unit Testing Async Functions (pg.2)

```js
// in math.js
export const delayedAnswer = () => {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve(2);
    }, 1000);
  });
};

// in /tests/math.test.js
import { delayedAnswer } from "../math.js";

it("should pass even tho async", async () => {
  const answer = await delayedAnswer(2);
  expect(answer).toBe(2);
});
```

---

## transition: slide-left

# Unit Testing Async Functions (pg.3)

another simpler example

```js
import { describe, it, expect } from "@jest/globals";

// pretend we imported this function from a different file
export const fetchData = async () => {
  return "data received";
};

describe("fetchData", () => {
  it('resolves with "data received"', async () => {
    const data = await fetchData();
    expect(data).toBe("data received");
  });
});
```

---

## transition: slide-left

# Spies (pg.1)

- Spies give existing functions introspection abilities. It lets you watch or override a function
- Great for mocking small functions without mocking the whole module

```js
// services/logger.js
export const log = (msg) => console.log(msg);

// services/userService.js
import { log } from "./logger.js";

export const createUser = (name) => {
  log(`User created: ${name}`);
  return { name };
};
```

- Notice userService.js imports/uses another function elsewhere.
- What if we want to test if `log()` gets called with the correct arguments?
- something like: `expect(log).toHaveBeenCalledWith('User created: Al');`

---

## transition: slide-left

# Spies (pg.2)

We can spy on `log()` to see if it's been called with the correct arguments

```js
import * as logger from "../services/logger.js";
import { createUser } from "../services/userService.js";
import { describe, it, expect, vi, beforeEach } from "vitest";

describe("createUser()", () => {
  beforeEach(() => {
    jest.spyOn(logger, "log").mockImplementation(() => {});
    // ✅ You are spying on the real logger.log function.
    // ✅ You are replacing it with a no-op using .mockImplementation(() => {}).
    // ✅ This avoids running the real implementation (e.g., logging to the console).
    // ✅ But you're still importing the real module (not auto-mocking it).
  });

  it("calls logger with the correct message", () => {
    const user = createUser("Al");
    expect(logger.log).toHaveBeenCalledWith("User created: Al");
    expect(user).toEqual({ name: "Al" });
  });
});
```

---

## transition: slide-left

# Mocks (pg.1)

`jest.fn()` can return a single function (fine-grained control) and thus act as a function placeholder

- Intro to `jest.fn()` - what it does, why real functions are replaced in tests

```js
// tests/vi-fn-basic.test.js
import { describe, it, expect, jest } from "vitest";

describe("how jest.fn() works", () => {
  it("should track how many times a function was called", () => {
    // try logging out mockFn below. and mockFn.mock
    const mockFn = jest.fn();

    mockFn();
    mockFn("hello");

    // NOTE: The reason I can call `.toHaveBeenCalled...()` is because of `jest.fn()`
    expect(mockFn).toHaveBeenCalledTimes(2);
    expect(mockFn).toHaveBeenCalledWith("hello");
  });
});
```

---

## transition: slide-left

# Mocks (pg.2)

`jest.fn()` also allows you to return a value via `.mockReturnValue()`

```js
it("should return a value", () => {
  const mockFn = jest.fn().mockReturnValue(42);

  const result = mockFn();
  expect(result).toBe(42);
});
```

- or via `.mockResolvedValue()`

  ```js
  it("should return an async value", async () => {
    const asyncMock = jest.fn().mockResolvedValue("mock data");

    const result = await asyncMock();
    expect(result).toBe("mock data");
  });
  ```

- can also clear/reset mocks
  ```js
  // inside beforeEach()
  jest.clearAllMocks(); // clears call history
  jest.resetAllMocks(); // resets implementation too
  ```

---

## transition: slide-left

# Mocks (pg.3)

`jest.mock()` allows for module mocking and replaces all exports with mock functions

```js
// This code is almost an exactly duplicate of the spyOn slide earlier. So how is jest.mock() different?
// services/logger.js
export const log = (msg) => console.log(msg);

// services/userService.js
import { log } from "./logger.js";

export const createUser = (name) => {
  log(`User created: ${name}`);
  return { name };
};

// userService.test.js
import { createUser } from "../services/userService.js";
import * as logger from "../utils/logger.js";

// jest.mock('../utils/logger.js') automatically replaces log() with a mock function (jest.fn() under the hood).
jest.mock("../utils/logger.js");

test("logs when a user is created", () => {
  createUser("Alice");
  expect(logger.log).toHaveBeenCalledWith("User created: Alice");
});
```

---

## transition: slide-left

# Spies vs Mocks

What’s the Difference Between jest.mock() and jest.spyOn()?

| Feature          | `jest.mock()`                                                | `jest.spyOn()`                                                         |
| ---------------- | ------------------------------------------------------------ | ---------------------------------------------------------------------- |
| **Purpose**      | Automatically mocks all functions in a module                | Observes and optionally mocks a single method on an object             |
| **Scope**        | Mocks **the entire module**                                  | Mocks **a specific method**                                            |
| **Auto-mocking** | Yes, unless you override it                                  | No — you must provide a mock manually (or call `mockImplementation()`) |
| **Restoration**  | Needs manual `jest.resetModules()` or `jest.clearAllMocks()` | Easily restored with `mockRestore()`                                   |
| **Use case**     | External libraries, utility modules, database models         | Internal app code, one-off method replacements                         |

---

## transition: slide-left

# When to use spyOn vs mock

Summary

Use jest.spyOn() when:

- You want to observe a real function without replacing it.
- You need to partially mock — e.g., let other methods in the module behave normally.
- You're testing an internal method or object, and want precise control.

Use jest.mock() when:

- You want to mock an entire module (like a service, DB model, or utility).
- You're testing something that depends on an external file.
- You want auto-mocking and are OK overriding individual methods later.

---

transition: slide-left

---

# Mocks (pg.4)

`jest.fn()` can also mock actual logic by accepting a custom implementation.

```js
import { describe, it, expect, vi } from "vitest";

describe("jest.fn() with custom logic", () => {
  it("should act like a real function", () => {
    const mockGreet = jest.fn((name) => `Hello, ${name}!`);

    expect(mockGreet("Alice")).toBe("Hello, Alice!");
    expect(mockGreet).toHaveBeenCalledWith("Alice");
  });
});
```

---

transition: slide-left

---

# Mocks (pg.5)

ex: Mocking an Express-like Controller

```js
const mockReq = {};
const mockRes = {
  send: jest.fn(),
};

const mockController = jest.fn((req, res) => {
  res.send("Mock response!");
});

mockController(mockReq, mockRes);

// Test
expect(mockRes.send).toHaveBeenCalledWith("Mock response!");
```

---

## transition: slide-left

# Mocks (pg.6)

test route

```js
import request from "supertest";
import express from "express";
import router from "router.js";

// Mock controller
jest.mock("truckController.js", () => ({
  getTrucksBySlug: jest.fn((req, res) => res.send("mock response")),
}));

// Mock middleware
jest.mock("authController.js", () => ({
  isAuthenticated: (req, res, next) => next(),
}));

describe("GET /trucks", () => {
  const app = express();
  app.use(router);

  it("responds with mock controller data", async () => {
    const response = await request(app).get("/trucks");
    expect(response.text).toBe("mock response");
  });
});
```

---

## transition: slide-left

# Mocks (pg.7)

test controller

```js
import { describe, it, expect, vi } from "vitest";
import { getTrucksByAuthor } from "../controllers/truckController.js";
import * as truckHandler from "../handlers/truckHandler.js";

jest.mock("../handlers/truckHandler.js");

describe("truckController.getTrucksByAuthor()", () => {
  it("renders trucks view with trucks from handler", async () => {
    const mockTrucks = [{ name: "Mock Truck" }];
    truckHandler.getTrucksBySlug.mockResolvedValue(mockTrucks);

    const req = { user: { _id: "user123" } };
    const res = { render: jest.fn() };

    await getTrucksByAuthor(req, res);

    expect(truckHandler.getTrucksByAuthor).toHaveBeenCalledWith("user123");
    expect(res.render).toHaveBeenCalledWith("trucks", {
      title: "All Trucks",
      trucks: mockTrucks,
    });
  });
});
```

---

## transition: slide-left

# Mocks (pg.8)

test handler

```js
import { describe, it, expect, jest } from "@jest/globals";
import * as truckHandler from "../handlers/truckHandler.js";

// Mocking Truck model
jest.mock("../models/Truck.js", () => ({
  Truck: {
    find: jest.fn(() => ({
      lean: jest.fn(() => [{ name: "Truck 1" }, { name: "Truck 2" }]),
    })),
  },
}));

describe("getTrucksByAuthor()", () => {
  it("returns trucks for a given author", async () => {
    const trucks = await truckHandler.getTrucksByAuthor("authorId123");
    expect(trucks).toEqual([{ name: "Truck 1" }, { name: "Truck 2" }]);
  });
});
```

---

## transition: slide-left

# Homework

- Practice building unit tests
- Practice full-stack by building a [movie ratings and review application](https://courses.circuitstream.com/d2l/le/lessons/9514/topics/49843)
