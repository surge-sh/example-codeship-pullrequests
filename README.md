# Codeship pull requests example [![Codeship Status for surge-sh/example-codeship](https://codeship.com/projects/86b9dbb0-eb83-0132-4c31-26192dc48311/status?branch=master)](https://codeship.com/projects/83463)

An example using [Codeship](https://codeship.com), for continuously previewing pull requests by publishing them on [Surge.sh](https://surge.sh).

## Getting started

Continuous Deployment services like [Codeship](https://codeship.com) make it possible to run publish your project on Surge each time you push to a GitHub or Bitbucket repository.

This guide will walk you through:

1. Getting your Surge token, so Codeship can login to Surge on your behalf
2. Adding Surge as a `devDependency`, so Codeship can install Surge
3. Setting up your project on Codeship, so you can publish each time you push to GitHub or Bitbucket

First, you’ll need your token from the Surge CLI. This secret key lets services like Codeship login and publish projects on your behalf. Get your Surge token by running the following command in your terminal:

```
surge token
```

You’ll be asked to login again, and afterwards your token will be displayed like this:

```
token: a142e09a6b13a21be512b141241c7123
```

![Getting your token from the Surge CLI.](https://surge.sh/images/help/integrating-with-codeship.gif)

### Adding Surge as a `devDependency` to a `package.json` file

Codeship’s machines won’t have Surge installed by default, so you also need to save Surge as a development dependency in your project. This lets Codeship know it needs to install Surge to publish your project.

You can do this by creating a `package.json` file if you don’t have one already. Run the following command in your terminal to be walked through making this file (you can hit enter to accept the defaults):

```sh
npm init
```

You will end up with a file that looks [something like this one](package.json).

Next, run this command to save Surge as a `devDependency`, so Codeship will install it:

```sh
npm install --save-dev surge
```

### Add your project’s repository to Codeship

Now you can login and setup a new project on Codeship. Add your project’s GitHub or Bitbucket repository to your Codeship projects:

![Add the GitHub or Bitbucket repository your project is stored in. This example is using the surge-sh/example-codeship repo.](https://surge.sh/images/help/integrating-with-codeship-2.png)

### Define your Setup and Test Commands

Now you’re ready to run `surge` on Codeship. Your Codeship setup commands run before the deployment command.

![](https://surge.sh/images/help/integrating-with-codeship-3.png)

Your setup commands should look like this:

```sh
# Install the latest version of Node.js
nvm install stable
nvm use stable

# Install Surge as a devDependency
npm install
```

After you push you successfully to your repository, Codeship will run your Test Pipeline commands, which is when you can publish your project with Surge:

```sh
# Run your tests (if you have any)
npm test

# Run Surge
surge --project ./ --domain example-codeship.surge.sh
```

### Add Environment Variables

Press _Environment Variables_ next, and you’ll be able to secretly add your email address and token so Codeship can login to Surge for you:

![Environment Variables is listed under your project settings.](https://surge.sh/images/help/integrating-with-codeship-4.png)

Create one environment variable called:

```
SURGE_LOGIN
```

…and set it to the email address you use with Surge. Next, add another environment variable called:

```
SURGE_TOKEN
```

…and set it to your Surge token.

![Adding `SURGE_LOGIN` and `SURGE_TOKEN` as environment variables.](https://surge.sh/images/help/integrating-with-codeship-5.png)

### Add a deployment script

Push to your repository, and your Setup and Test commands should run, triggering your tests to run on Codeship. Now, Codeship will let you move onto adding a script for Continuous Deployment. You can press the button labeled _Set up Continuous Deployment_ or access this section under your project settings:

![Adding a custom script for continuous deployment.](https://surge.sh/images/help/integrating-with-codeship-6.png)

Add a _Custom Script_ and enter the command you want to run with Surge, for example:

```sh
surge --project ./ example-codeship.surge.sh
```

Now, when you push your poject to GitHub or Bitbucket again, this command will be run and your project will get published automatically.

## License

[The MIT License (MIT)](LICENSE.md)

Copyright © 2015 [Chloi Inc.](http://chloi.io)
