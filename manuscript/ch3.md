# Static Page Implementation

In this chapter, we'll develop the first version of our "Quote of the Day" app. This version is straightforward, merely displaying a static inspirational quote on the webpage that can be run in a browser.

We can start by creating a directory named `quote-of-the-day`:

```bash
mkdir -p quote-of-the-day
cd quote-of-the-day
```

The two command lines above do the following:

1. `mkdir -p quote-of-the-day`: This creates a directory named "quote-of-the-day". Here, `mkdir` is a command for creating a directory, and `-p` is an option that suppresses error messages if the directory already exists. This command creates a directory named "quote-of-the-day" in the current working directory.
2. `cd quote-of-the-day`: This moves into the "quote-of-the-day" directory. `cd` is a command for changing the current working directory, and "quote-of-the-day" is the target directory name. This command switches the current working directory to the "quote-of-the-day" directory.

Inside this empty directory, we can initialize it as a Node.js project by executing `npm init`:

```bash
$ npm init -y
```

The `-y` option tells the `npm` command to skip the interactive interface and generate the Node.js project with default values. Upon completion, npm will write a package.json file in the current directory. This file is our project's descriptor file, and our future build scripts will revolve around it.

```bash
Wrote to /Users/juntao/icodeit/ideas/quote-of-the-day/package.json:

{
  "name": "quote-of-the-day",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}
```

Now, if we execute `npm test` in the command line, we get the following output:

```bash
> quote-of-the-day@1.0.0 test
> echo "Error: no test specified" && exit 1

Error: no test specified
```

This is because, in the `scripts` section of package.json, we've defined a command that gets executed when `test` is run:

```bash
echo "Error: no test specified" && exit 1
```

This line actually consists of two separate commands, `echo "Error: no test specified"` and `exit 1`, connected by the logical operator `&&`. Like in programming languages, the second command (`exit 1`) only executes if the first command (`echo`) executes successfully. If we change the `test` in package.json to:

```bash
"scripts": {
  "test": "echo \"⛔️ I don't know what to run 🤷\" && exit 1"
},
```

When we run `npm test` again, the command-line output changes:

![npm test](ch3/npm-test.png)

Additionally, we can define new tasks in scripts. For example, we can define a task named `start` that prints the current time when executed:

```bash
"scripts": {
  "test": "echo \"⛔️ I don't know what to run 🤷\" && exit 1",
  "start": "echo 🕞: `date`"
},
```

When `npm start` is executed, the command line displays:

![npm start](ch3/npm-start.png)

Alright, now that we have a rough understanding of the `scripts` section in `package.json`, we can start the development of **Quote of the day**.

## Initializing the Project

First, we need to define two directories: `src` for storing the source code and `dist` to act as the server root directory. Since our application will run on a local port, we need an HTTP server, so when users access this port via the browser, they will reach the content in the `dist` directory.

```bash
mkdir -p src dist
touch src/index.html src/style.css
```

The above two commands:

1. `mkdir -p src dist`: This creates two directories named "src" and "dist". `mkdir` is a command to create directories, and `-p` is an option which means it won't throw an error and will continue to execute if the directories already exist. This command creates the "src" and "dist" directories in the current working directory.
2. `touch src/index.html src/style.css`: This creates two files, "src/index.html" and "src/style.css". `touch` is a command used to create files, followed by the path and name of the file. This command creates an "index.html" file and a "style.css" file in the "src" directory.

In the first version, we just need to display a static quote in index.html:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Quote of the day</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
<div>
  <p class="content">If you want to lift yourself up, lift up someone else.</p>
  <p><span class="author">&mdash; Booker T. Washington</span></p>
</div>
</body>
</html>
```

In the corresponding CSS file, we defined a text alignment style:

```css
body {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  margin: 0;
}
```

This code aligns the contents of the `<body>` element in the center of the viewport, and ensures that the height of the entire page fills the whole viewport height. This is common in many web layouts, helping to align the content of the page in the center and fill the screen.

We've now completed the code changes needed for the first version. Next, we need to install an HTTP server, so we can access our application via a browser.

## Static Server: http-server

The http-server in Node.js is a simple command-line tool for creating a basic HTTP server locally. It allows you to quickly start a static file server during development for testing and debugging web pages, applications, or other static resources.

The http-server provides a lightweight development server that runs on the local host and listens to a specified port. You can use the http-server command-line tool to start the server, specifying the file directory and port number to be served. Once the server is started, you can view and access your files by visiting the corresponding URL in the browser.

The http-server is a popular development tool because it's easy to use, requires no additional configuration, and provides basic HTTP server features such as static file access, cache control, and CORS support. It's one of the preferred tools for many developers in the Node.js ecosystem during local development.

We can install this tool by executing the following command in the Terminal:

```sh
npm install http-server --save-dev
```

After installing `http-server`, we can use the `http-server` command in the Shell. Usually, we specify a directory as the root of the HTTP server and then specify a port:

```sh
http-server dist -p 3000
```

This command starts an HTTP server in the `dist` folder under the current working directory and listens on port 3000. This way, we can view and access the static files in the `dist` folder by visiting `http://localhost:3000` in the browser.

However, the current problem is that there is no content in the `dist` directory. We need to copy the HTML and CSS files from `src` to `dist`. Although this step may seem redundant, it's necessary for isolating the source code from the production code. In our first version, the source code is simple, and we don't need to make conversions or complex developments, so the source code and the production code are the same. In actual scenarios, the source code is usually formatted for easy writing and debugging by developers, while the production code doesn't need to consider readability and is often compressed or obfuscated to prevent reverse engineering. Therefore, the two are not usually the same. We will discuss this in more detail in the next chapter.

We can define a new task `build` in `package.json`. The content of this task is to copy the HTML and CSS from `src` to `dist`:

```json
"scripts": {
  "build": "cp src/* dist/"
},
```

Finally, we need to modify the `start` task as follows:

```json
"scripts": {
  "build": "cp src/* dist/",
  "start": "http-server dist -p 3000"
},
```

We first need to execute `npm run build` to copy the source files to the production code. Then when we execute `npm start`, `http-server` will run on port 3000.

```sh
npm run build
npm start
```

Enter `[http://localhost:3000](http://localhost:3000)` in the browser, and you can see our first version of **Today's Proverb**!

![Running in localhost](ch3/localhost.png)

In this chapter, we first introduced the use of the Shell and the command line interface, including how to enter commands in the command line, how to view the output of commands, and how to use the command line for quick and efficient development.

Then, we spent some time interpreting the `scripts` section in Node.js's `package.json` file. We learned how to define our own tasks, how to use the `npm run` command to run these tasks, and the importance of these tasks in the project development and build process.

In addition, we also detailed the use of `http-server`. We explained how to use this simple HTTP server to preview and test our project, giving everyone a deeper understanding of this common development tool.

Lastly, we successfully built and ran the first version of the "Today's Proverb" page using our newfound knowledge. Through this practical project, readers can better understand and master the above theoretical knowledge, and we also demonstrated how to run an HTTP server locally and deploy and preview our project in the server.

In the next chapter, we will use React to extend the content of the first chapter and learn more about build scripts.