# Communicating with Backend API

In this chapter, we'll learn how to extend our "Quote of the Day" application to fetch quotes from a remote server rather than hardcoding them into our source code.

Making network requests to fetch data is a common pattern for most applications. The frontend initiates requests using the HTTP protocol, the backend application analyzes and processes the request (determining the format of the request and the preferred format of the frontend), and then returns the corresponding data. Common HTTP-based APIs include RESTful API and GraphQL.

## Quotable API

We'll use [Quotable](https://github.com/lukePeavey/quotable#list-quotes) as our backend API. The Quotable API is a free RESTful API that provides a vast number of famous quotes and sayings. Each quote includes the author, content, and their associated tags or categories.

Here are its main features:

- Retrieve a random quote: You can call the API to get a random quote, which includes the content and the author's information.
- Get quotes from a specific author: You can get all the quotes from an author by searching for their name.
- Search for quotes: The API provides a search feature, allowing you to find relevant quotes based on keywords.
- Retrieve quotes based on tags: Each quote is tagged with relevant categories, such as "love", "life", "friendship", etc. You can fetch related quotes based on these tags.

The Quotable API returns data in JSON format, making it easy for developers to process. With this API, developers can enrich their projects with a diverse collection of quotes without worrying about data storage and management.

We can access the Quotable API through a command-line interface to see the data format:

```bash
curl https://api.quotable.io/quotes/random
```

This command uses `curl` with the Quotable URL as a parameter to fetch a random quote. The server returns the following format:

```json
[
  {
    "_id": "N43bqeXRBeqI",
    "content": "Just trust yourself, then you will know how to live.",
    "author": "Johann Wolfgang von Goethe",
    "tags": [
      "Famous Quotes"
    ],
    "authorSlug": "johann-wolfgang-von-goethe",
    "length": 52,
    "dateAdded": "2021-05-07",
    "dateModified": "2023-04-14"
  }
]
```

As you can see, the server returns an array with one element, containing data such as content, author, and some metadata such as length, date added, and tags.

Quotable also supports searches by tag or author. For instance, we can retrieve all quotes related to happiness:

```bash
curl https://api.quotable.io/quotes?tags=happiness&limit=3
```

The command above limits the results to three quotes, thus preventing excessive backend API calls. Quotable also supports sorting, pagination, and keyword search. However, for the purposes of this book, we only need the random quote interface.

## Frontend Code

On the frontend, we can use `fetch` to send the request and then render the data as before. We need to use `useEffect` in our App.jsx:

```jsx
const App = () => {
  const [quotes, setQuotes] = useState([]);

  useEffect(() => {
    const fetchQuotes = async () => {
      fetch("https://api.quotable.io/quotes/random?limit=3")
        .then((r) => r.json())
        .then(quotes => setQuotes(quotes))


    }

    fetchQuotes();
  }, []);

  return (
		//...
  );
};
```

First, we replace the hardcoded quotes. Then, inside `useEffect`, we use fetch to get three random quotes. Finally, once the data is fetched, the page will render these three quotes.

When we refresh the page, we surprisingly find the page blank. If you open the browser's debugger (inspect the page), you'll find many errors in the console.

![console error](ch6/console.png)

This happens because `fetch` is an asynchronous operation. When the request is sent, the `quotes` array is still empty. However, as we used an expression like `quotes[index].content` to render, we get an error when trying to access the `content` property of an undefined object.

We can add a guard clause to this expression to solve the problem:

```jsx
return (
  <>
    <div>
      <p className="content">{quotes[index] && quotes[index].content}</p>
      <p>
        <span className="author">&mdash; {quotes[index] && quotes[index].author}</span>
      </p>
    </div>
    <button onClick={clickHandler}>next</button>
  </>
);
```

The code above ensures that `quotes[index]` exists (is not undefined) before accessing its `content` and `author` properties. With this change, the page works as expected again.

## Fixing Tests

If we run the tests now using `npm run e2e`, we'll find that all tests have failed. This is because the test still expects the data to be the hard-coded values we initially used in the source code. 🤔️, This could be a bit tricky. On one hand, we want these quotes to be random, but on the other, we need to test the quotes on the page. When running tests, how does the test code know what "random" data the backend will return?

This introduces the concept of stubbing. In testing, we can intercept network requests and pretend to be the server, returning a set of pre-defined data. This allows the tests to pass. In the actual execution of the application, i.e., in the end user's browser, this interception code does not exist, so the user sees real random data.

In Cypress, we can use the `cy.intercept()` method to intercept network requests and return stub data. This allows our front-end tests to no longer depend on the actual returns from the backend API, but rather verify our code based on our own preset data.

For example:

```js
cy.intercept('GET', '/quotes/1', {
  statusCode: 200,
  body: {
    id: 1,
    quote: "Truth can only be found in one place: the code.",
    author: "Robert C. Martin"
  }
});
```

In the code above, the `cy.intercept()` method intercepts all GET requests to `/quotes/1` and returns our pre-set JSON data. After this, all GET requests to `/quotes/1` will return our preset data, rather than the actual return from the backend API.

For our test code, we need to make the following modifications in `quote-of-the-day.spec.cy.js`:

```js
describe("quote of the day spec", () => {
  beforeEach(() => {
    cy.intercept('GET', 'https://api.quotable.io/quotes/random*', {
      statusCode: 200,
      body: quotes
    });
  });

  it("displays a quote", () => {});

  it("clicks next button", () => {});
});
```

In the test code above, we used `beforeEach`: a Cypress hook function that runs before each test case. Within this function, we use the `cy.intercept()` method to intercept all GET requests pointing to `https://api.quotable.io/quotes/random*` and make them return preset `quotes` data.

This allows us to test our code in a stable environment, without worrying about the uncertainty that the API return data might bring.

## Refactoring Code

We all aim to write high-quality code that is easy to understand and modify. When we see unreasonable structures or less elegant expressions, as professional programmers, we can't help but make changes for the better. However, often even with this beautiful idea, it's hard to implement. The reason, to a large extent, is that the cost of doing so is too high.

The high cost can be described in a few parts: first, understanding existing poorly written code itself requires a lot of time and effort. If errors occur in the refactoring process, we need to spend extra time and effort retesting. So many times, programmers choose to delay refactoring. This in turn makes future understanding and rewriting more difficult, costing more time and effort, and so on.

Therefore, fundamentally, we should establish a mechanism that can protect us from the beginning, allowing us to verify our modifications in less time without worrying about breaking existing features. This mechanism is automated testing and automated build scripts.

With the protection of tests, we can make some structural adjustments to the current

 code without worrying about the final program being damaged. This is very important in programming.

We can move the part that fetches network data to a separate custom hook and store it in a new file `useFetchQuotes.js`:

```js
import { useEffect, useState } from "react";

const useFetchQuotes = () => {
  const [quotes, setQuotes] = useState([]);
  const [loading, setLoading] = useState(false);

  const fetchQuotes = () => {
    setLoading(true);
    fetch("https://api.quotable.io/quotes/random?limit=3")
      .then((r) => r.json())
      .then((data) => {
        setLoading(false);
        setQuotes(data);
      })
      .catch((e) => {
        setLoading(false);
      });
  };

  useEffect(() => {
    fetchQuotes();
  }, []);

  return { quotes, loading, fetchQuotes };
};

export { useFetchQuotes };
```

This code defines a custom React Hook named `useFetchQuotes`, which fetches three random quotes from the Quotable API. Here's a detailed explanation of this code:

- `useState` is used to define state within function components. `quotes` is a state variable used to store the quotes fetched from the API, `setQuotes` is the corresponding setter function. `loading` is a state variable used to indicate whether the data is being loaded, and `setLoading` is its corresponding setter function.
- The `fetchQuotes` function fetches random quotes from the Quotable API. When it starts fetching data, it sets `loading` to `true`. When data is successfully fetched, it stores the quotes in the `quotes` state variable and sets `loading` to `false`. If an error occurs during data fetching, it only sets `loading` to `false`.
- The `useEffect` hook calls the `fetchQuotes` function when the component first renders, hence the quotes are fetched when the component first renders. The `[]` as the dependency array for `useEffect` means that the `fetchQuotes` function is only called when the component first renders, not every time it renders.
- `useFetchQuotes` ultimately returns an object containing `quotes` and `loading` state variables representing the fetched quotes and the loading status respectively. This allows components using this custom Hook to destructure and use these two state variables.

Then in App.jsx, we can directly use the loading status and quotes array provided by this hook. For the JSX part, we can also extract a new component `Quote` to make the code clearer and more concise:

```jsx
import React from "react";

const Quote = ({ quote }) => {
  return (
    <div>
      <p className="content">{quote && quote.content}</p>
      <p>
        <span className="author">&mdash; {quote && quote.author}</span>
      </p>
    </div>
  );
};

export { Quote };
```

As such, the revised App.jsx will be simplified to:

```jsx
const App = () => {
  const [index, setIndex] = useState(0);
  const { loading, quotes } = useFetchQuotes();

  const clickHandler = () => {
    setIndex((index) => (index + 1) % 3);
  };

  return (
    <>
      {loading && <div>loading...</div>}
      {!loading && <Quote quote={quotes[index]} />}
      <button onClick={clickHandler}>next</button>
    </>
  );
};
```

In this way, each part's responsibility is clearer, the network is handled by the custom hook, the specific display is completed by the Quote component, and the App is responsible for specific coordination

 and calling. More importantly, while obtaining these cleaner codes, our automated tests are still passing.

![end to end](ch6/e2e-2.png)

That is to say, we don't need to worry about accidentally breaking the software that we deliver.

## Conclusion

In this chapter, we discussed the theme of building applications using React and APIs. We introduced how to use the Quotable API to get data and how to fetch and display data. After introducing network requests, we learned how to use the `cy.intercept()` method provided by Cypress to intercept and simulate API requests.

After fixing the tests, we refactored the current code, extracting the custom Hook `useFetchQuotes` and a display component Quote, thus making the code cleaner and easier to read. Through the development of a feature, we can clearly feel the important role of automated testing and build scripts in software development.