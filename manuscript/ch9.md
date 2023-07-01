# Epilogue

In this book's epilogue, I want to take a moment to reflect on the journey we've embarked upon. It's a journey that began with constructing a simple static page, gradually introducing various tools and technologies, and finally creating a fully automated full-stack application capable of continuous integration and delivery.

Our journey began with a humble static page. While the simplicity and comprehensibility of a static page are its strengths, the limitations also become apparent as the application grows. Hence, we introduced `package.json` and `scripts`, making our build process more configurable and flexible. This introduction marked our first encounter with automation - executing a series of tasks through running simple scripts, saving time on manual operations.

Subsequently, we introduced a static server. Through it, we could better simulate a real server environment, helping us discover and resolve potential production environment issues early. Simultaneously, the static server provided us with a development environment, enabling us to test our application without the need to deploy it to production each time.

Next, we introduced React and a bundler. React, being one of the most popular frontend frameworks, offers a modular way of constructing complex user interfaces. The bundler assists us in packaging multiple files and modules into one or more optimized files, enabling our application to run in various environments.

To ensure the quality of our code, we proceeded to incorporate static checking tools. Through static checking, we could spot potential errors and issues before the code runs, which is critical for large projects.

We continued by adopting Cypress for acceptance testing. Acceptance testing is a powerful method of automated testing, ensuring all parts of our application work as expected. In addition, we utilized file watching, enabling us to see the results immediately after modifying code, enhancing our development efficiency.

Then, we started integrating backend API interactions, a pivotal step in transitioning our application from a static page to a truly dynamic application. For more stable testing, we introduced stub data, simulating API responses. Consequently, our tests no longer relied on the actual backend server, making the results more reliable and predictable.

In the final stages of our journey, we integrated continuous integration and continuous delivery, along with GitHub Actions. Through continuous integration and delivery, we could run tests immediately after code submission and then deploy the code to production, making our development process more automated and efficient. GitHub Actions, a powerful tool, allows us to set up and run automation workflows directly in the GitHub repository.

Finally, we deployed our application to GitHub Pages, a free static site hosting service, making it easy to publish our application online for others to access and use.

Throughout this journey, we transitioned from a simple static page to a complex full-stack application, learning and implementing numerous tools and technologies, including React, bundlers, static checking, automated testing, file watching, backend API interactions, stub data, continuous integration and delivery, GitHub Actions, and application deployment. Each step enhanced our application and made our development process more automated and efficient.

So, that's our journey: from nothing to something, simple to complex, manual to automated. The process was challenging yet rewarding, but most importantly, it offered us a deep understanding of various aspects of modern frontend development, thereby making us better developers. I hope this book will be a helpful companion on your own development journey, regardless of whether you're a novice or an experienced developer, and that you will find it enlightening.