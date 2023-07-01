# Preface

Software development is a complex process. A seemingly simple application often hides a multitude of factors that can lead to its failure. Unstable networks, erroneous connection strings, inconsistent data formats, forgotten memory cleanups, unreleased log spaces, and so on, all contribute to uncertainties that force developers to tread carefully. In a typical software project, especially one with some scale that involves multiple teams, we see a process like this happening daily: modify code, transfer to testing, reject and revise, re-test, integrate, jointly debug, discover new issues, reject and revise again, re-test again, and so forth.

Software development shouldn't be like this, and it **can** be different. The crux of the matter lies in gaining confidence in software behavior before release, and in the efficiency of rolling back to a previous working version when a problem is discovered. In other words, if we are 90% confident in new changes at the time of release, we will release it. Additionally, if an issue arises online, we can switch back to a previous version in a very short time. If we can achieve these two points, we probably don't need to worry about things like transferring to testing, joint debugging, etc., after all, we can always fall back to the previous version.

However, achieving this is not easy. We need to build many protective mechanisms and infrastructure in advance. Each code change triggers rigorous automated tests, each file modification adheres to predefined rules, each merge triggers all automated regression tests, acceptance tests, and end-to-end tests, and eventually forms a deployable software package (sometimes an image file). That is, we eliminate error-prone issues with automation from the beginning, and if they can't be eliminated, we can promptly remediate.

This is the essence of this book: through writing automated build scripts and implementing continuous delivery practices, we can achieve a more efficient and developer-friendly delivery method. The main purpose of this book is to teach you through a real example how to define build scripts, how to use these scripts in local and continuous integration environments, and how to perform common development tasks such as translation (translation+compiling), static checking, packaging, automated testing, automated deployment, etc., to achieve continuous delivery, improve efficiency, and ensure product quality.

## Before We Start

In this book, I assume that you have some understanding of programming, you need to understand the Node environment, and the current version of [Node.js](https://nodejs.org/en) has been installed locally. If you execute the command

```bash
npm --version
```

in your local terminal (assuming you are in the Mac OSX environment) and get a version number similar to `9.5.0`, it means you already have everything set up. Besides, you need to know some basic React knowledge. This is not the focus, all you need is to have gone through this [Quick Guide](https://react.dev/learn), it's enough to continue reading, after all, the purpose of this book is the command line and continuous integration environment.

Lastly, you need a [Github](https://github.com/) account and properly configured SSH public and private keys. To confirm that your local SSH key can connect to GitHub, you can follow the steps below:

1. Run the following command in your local command line:

    ```bash
    ssh -T git@github.com
    ```
    
    This command will try to connect to GitHub via SSH. The `-T` option prevents ssh from requesting a shell, allowing us to simply test connectivity instead of trying to create a shell.

2. The first time you connect to GitHub, you may see a warning message like this:

    ```
    The authenticity of host 'github.com (IP ADDRESS)'

 can't be established.
    RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
    Are you sure you want to continue connecting (yes/no)?
    ```

    In this case, you should type "yes" and press enter.

3. If your SSH key is set up correctly, you should see a message like this:

    ```
    Hi USERNAME! You've successfully authenticated, but GitHub does not provide shell access.
    ```

    Here "USERNAME" is your GitHub username. This means that SSH has successfully connected to GitHub. If you see this message, your local SSH key can successfully connect to GitHub.

4. If your SSH key is not set up correctly, you may see a "Permission denied" message. In this case, you may need to regenerate your SSH key or confirm whether your public key has been added to your GitHub account. You can check GitHub's help documentation for more information on setting up SSH.

## The Quote of the Day Application

The application we're going to build is called *Quote of the Day*. It's a simple program that randomly selects a **famous quote** and displays it on a card. There are two buttons at the bottom of the card, one to download the card as an image, and the other to refresh the card and randomly display another famous quote.

![Quote of the day](ch1/quote-of-the-day.png)

In the process of building this application, we will cover the following content:

- Learn the basics of command line tools and Shell scripts: Master the basic operations of the command line and understand the functions and usage scenarios of Shell scripts.
- Automate your development process with npm scripts: Detailed explanation of how to use npm scripts to define and execute build scripts, automating tedious development tasks.
- Compile React code into browser-runnable JavaScript: Explain how to compile React code into JavaScript, further understanding the operating principles of React applications.
- Write user acceptance tests to ensure product quality: Introduce how to write user acceptance tests, discover and fix problems in advance, ensuring the correctness and stability of product features.
- Call RESTful API for seamless backend data integration: Teach how to interact and display data by calling RESTful API, realizing the interaction between frontend and backend data.
- Use Github Actions to build an automated continuous integration/delivery process: Detailed explanation of how to use Github Actions to achieve continuous integration and delivery, improving the efficiency and quality of software development.

Through this book, you will not only master these skills but also understand how they work in real projects and how to integrate them into your development process.