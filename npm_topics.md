# Table of Contents

1. [Initialize Project](#initialize-project)
1. [Semantic Versioning](#semantic-versioning)
1. [Editor Config](#editor-config)
1. [Babel](#babel)
1. [Build](#build)
1. [Source File](#source-file)
1. [Linting NPM Packages](#linting-npm-packages)
1. [Publish NPM Package](#publish-npm-package)
1. [Test Package Locallly](#test-package-locallly)

## Initialize Project
Create main directory
```
mkdir npm101
```
Initialize git
```
git Init
```
Create .gitignore and add lib dir
```
vi .gitignore
echo 'lib/' >> .babelrc
```
Create .npmignore and add npm_modules
```
vi .npmignore
echo 'npm_modules/' >> .babelrc
```
Initialize npm. This command will create the package.json file
```
npm init -y
```
Modify package.json
```
add "description"
add "author"
modify "main"
```

## [Semantic Versioning](#https://semver.org/)

In this section, we are not going to change anything in our project. The focus here is to talk about how to label new releases of our package. In the NPM and Node.js landscape, the most used strategy is by far Semantic Versioning. What makes this strategy so special is that it has a well-defined schema that makes it easy to identify what versions are interoperable.

Semantic Versioning, also known as SemVer, uses the following schema: MAJOR.MINOR.PATCH. As we can see, any version is divided into three parts:

MAJOR: A number that we increment when we make incompatible API changes.
MINOR: A number that we increment when we add features in a backwards-compatible manner.
PATCH: A number that we increment when we make small bug fixes.
That is, if we have a problem with our code and fix it simply by changing an if statement, we have to increment the PATCH part: 1.0.0 => 1.0.1. However, if we need to add a new function (without changing anything else) to handle this new scenario, then we increment the MINOR part: 1.0.0 => 1.1.0. Lastly, if this bug is so big that requires a whole lot of refactoring and API changes, then we increment the MAJOR part: 1.0.0 => 2.0.0.

## [Editor Config](#http://editorconfig.org/)
```
root = true

[*]
charset = utf-8
indent_style = space
indent_size = 2
insert_final_newline = true
trim_trailing_whitespace = true
```

## Babel
#### ES6+: Developing with Modern JavaScript
* [Javascript Engines](#https://en.wikipedia.org/wiki/JavaScript_engine#Implementations)
* [ES6 Compatibility](#https://babeljs.io/)

Create the .babelrc file
```
npm install --save-dev babel-cli babel-preset-env
echo '{
  "presets": ["env"]
}' >> .babelrc
```

## Build
Add the build tag to the scripts tag in the package.json file.
```
{
  ...
  "scripts": {
    "build": "babel ./src -d ./lib",
    ...
  }
  ...
}
```

## Source File
Create the source directory in the root of the project
```
mkdir ./src
cd src/
vi index.js
```
Add the following lines to the index.js file
```
function sayHiTo(name) {
  return `Hi, ${name}`;
}

module.exports = sayHiTo;
```
Build the package
```
npm run build
```
Go to the lib directory created and see the new index.js file converted to ES5
```
cd ./lib
```

## [Linting NPM Packages](#https://en.wikipedia.org/wiki/Lint_%28software%29)

Most popular lint analizers [ESLint](#https://eslint.org/), [JSHint](#http://jshint.com/), and [JSLint](#http://www.jslint.com/)

We have to instruct NPM to install it for us, then we can use the --init option provided by ESLint to generate a configuration file:

Saving ESLint as a development dependency
```
npm i -D eslint
```
This is the same as
```
npm install --save-dev eslint
```

### Initializing the configuration file
```
./node_modules/.bin/eslint --init
```
Answer the questions

* How would you like to configure ESLint? **Use a popular style guide**
* Which style guide do you want to follow? **Airbnb**
* Do you use React? **No**
* What format do you want your config file to be in? **JSON**

This will generate a small file called .eslintrc.json with the following content:
```
{
    "extends": "airbnb-base"
}
```

What is nice about ESLint is that it also enables us to adhere to popular style guides (in this case the Airbnb JavaScript Style Guide). There are other popular styles available to JavaScript developers and we could even create our own. However, to play safe, we will stick to an existing and popular choice.

To add ESLint to our build process, we can create a new script that executes ESLint and make it run in the build script:
```
{
  ...
  "scripts": {
    "build": "npm run lint && babel ./src -d ./lib",
    "lint": "eslint ./src",
    ...
  },
  ...
}
```

## Publish NPM Package
What we want instead is to automatically tie the build script to publish. Luckily for us, when NPM is publishing a new version of a package, it checks the package.json file to see if there is a script called prepublishOnly. If NPM finds this script, it runs whatever command is inside it. Therefore, what we have to do is to configure prepublishOnly in our package.json file as follows:

```
{
  ...
  "scripts": {
    ...
    "prepublish": "npm run build"
  },
  ...
}
```

Looks like we are ready to publish our package. Let's run npm publish and make it available to the world. Note that, before publishing, we might need to create a NPM user and to login to our NPM CLI (npm login).
```
npm publish
```

## Test Package Locallly
Locally test your npm modules without publishing them to npmjs.org

This will create, in the same directory, a tarball named after the name of the project + the version specified in the package.json.
```
npm pack 
```
This is exactly what is published to npmjs.org! You can now see what’s inside
```
tar -tf packagename-version.tgz
```
Or you can install it in your project to test it in integration.
```
cd path/to/your/project/
npm install ../path/to/your/npm/packagename-version.tgz
```
