---
layout: post
title: Publishing npm packages
categories: npm
tags: [gitHub, javascript, npm]
---

The [npm repository](https://www.npmjs.com/){{ site.new_tab }} holds over 700,000 packages, and adding your own public package is an easy task.

<!--more-->

## npm account

First, you will need to set up an account on npmjs.com, see the [Getting started](https://docs.npmjs.com/getting-started/installing-node){{ site.new_tab }} guide for more details.  This will also show you how to install npm if you haven't already, check it has been installed by using:

```bash
npm -v
```

## Logging into your account

You will then need to log into your npm account so that you can publish to the repository, using:

```bash
npm login
```

to check that you are logged in correctly enter

```bash
npm whoami
```

## Setting up your package

First create a new folder on your computer, for instance 'examplepackage', then initialise git and npm:

```bash
npm init -y
git init
```

this will create a package.json file similar to:

```json
{
  "name": "examplepackage",
  "version": "0.1.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "N Lupton",
  "license": "MIT"
}
```

Now create three files in the folder - index.js, index.test.js and README.md.

## Github repository

On Github (or whichever Git hosting service you use) create a new repo for your examplepackage

![image text]({{ "/images/posts/npm packages - create a new repo.png" | absolute_url }} "Creating a new GitHub repository - MIT Licence selected")

(note I included adding the MIT licence in this step, you don't need to include that if you don't want to).

Now connect your local repository to the Github repo, then do a pull to get the licence file created by Github:

```bash
git remote add origin git@github.com:<your repo name>
git pull origin master
```

Now we can set up out index.js and index.test.js files:

```javascript
// index.js
function sayHello() {
    console.log('Hello from your example package!');
}

module.exports = sayHello;

// index.test.js
const assert = require('assert');
const sayHello = require('./index.js');

assert( typeof sayHello === 'string' );

console.log('test complete!');

```

For the test file I'm just using the node assert library, and initially using a test that should fail, so that we can be sure that we aren't getting any false positives.
The only point of this test is to makes sure the function is imported correctly, we are only doing this so that we have a test file to exclude from the package later on.

For README.md put some basic details in for now:

```markdown
# Example Package

A simple package to demonstrate publishing an npm package.

```

Now we can commit these initial files and push them to the repository:

```bash
git add .
git commit -m 'initial setup'
git push
```

## Testing your files

Let's do some testing on the package, first let's try index.test.js which should throw an error:

```bash
$ node .\index.test.js
assert.js:42
  throw new errors.AssertionError({
  ^

AssertionError [ERR_ASSERTION]: false == true
```

Now we can fix the test and try again

```javascript
const assert = require('assert');
const sayHello = require('./index.js');

assert( typeof sayHello === 'function' );

console.log('test complete!');

```

```bash
\examplepackage\ (master)  $ node .\index.test.js
test complete!
```

and we can try out our package using the Node REPL:

```bash
\examplepackage\ (master)  $ node
> require('./index.js')()
Hello from your example package!
undefined
```

but note that we have a package.json file, and in that file we have `"main": "index.js"` which tells node what our entry point is, so we can change this to:

```bash
\examplepackage\ (master)  $ node
> require('.')()
Hello from your example package!
undefined
```

and our test program to

```javascript
const assert = require('assert');
const sayHello = require('.');

assert( typeof sayHello === 'function' );

console.log('test complete!');

```

Re-running our test should give us the 'test complete!' message.  So at this point, we have a fully working, highly advanced package which we are ready to share with the world.

## Scoped packages

When you publish to npm your package name needs to unique on the repository, you can't overwrite any existing package (and examplepackage does already exist), so you need to change the name to something unique.  
The package name is taken from name field in your package.json, which when you use `npm i -y` defaults to the current directory name, so at the moment it will be something like `"name": "examplepackage"`.  You could try adding numbers to this name to make it unique, but polluting the repository with test packages isn't going to win you any friends.  
Fortunately, a better solution exists - [scoped packages](https://docs.npmjs.com/getting-started/scoped-packages){{ site.new_tab }} - this allows you to scope the package to your own npm id so that you don't have to worry about an existing package having a conflicting name.  It also helps users make sure they install the correct package, for instance, if they mistyped the `npm install` command and used the name of a different package that exists on the repository, with potential security problems.

Setting your package as a scoped package is easy- just alter the `name` field to the following format `@scope/project-name` using your npm id as the @scope, for instance:

```json
   "name": "@neil188/examplepackage",
```

while we are updating package.json, let's add some more details - setting the test script, adding your repository :

```json
{
  "name": "@neil188/examplepackage",
  "version": "0.1.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "node index.test.js"
  },
  "keywords": [],
  "author": "N Lupton",
  "license": "MIT",
  "repository": {
    "type": "git",
    "url": "https://github.com/Neil188/eslint-config"
  }
}
```

If you want to npm init to use a scoped package by default you can use the command

```bash
npm config set scope <your npm id>
```

This will update your .npmrc file to include this default.

## Testing your package

At this point, you've tested your program runs, but you really want to test that the package works before you actually publish it.  Fortunately, this is easy to do with the [npm link](https://docs.npmjs.com/cli/link){{ site.new_tab }} command, which creates a symlink to your local package.  First in your examplepackage folder use command

```bash
npm link
```

Now we can create a new project to use our package without publishing to the repository or using a direct link to the examplepackage folder.  Outside of you examplepackage folder, create a new folder, in that folder try to install your package using `npm link <package name>`:

```bash
cd ..
mkdir testexamplepackage
cd testexamplepackage
npm link @neil188/examplepackage
```

In your testexamplepackage folder, you should now see a node_modules folder created, and that folder should contain a sub-folder for your scope, which will contain a link to your package (instead of containing the package downloaded from the repository that you should usually see.

![image text]({{ "/images/posts/npm packages - symlink.png" | absolute_url }} "Directory structure showing link to package in node_modules folder")

Now we can test the package works - create an index.js file and populate with:

```javascript
const sayHello = require('@neil188/examplepackage');

sayHello();

```

and try running this program:

```bash
$ node index.js
Hello from your example package!
```

So now you've tested you can use your package successfully, so you can go back to publishing to the npm repository.

## Building the package

Before we send the package to the repository, let's see what our package will contain.  Run the command `npm pack` which will create a [tarball](https://en.wikipedia.org/wiki/Tar_(computing)){{ site.new_tab }} file named <name>-<version>.tgz that contains the contents of your package.

Once created you can use the command `tar -xzf <package name>.tgz` to extract the files (they will be unpacked to a /package/ directory)

```bash
$ npm pack
npm notice
npm notice package: @neil188/examplepackage@0.1.0
npm notice === Tarball Contents ===
npm notice 317B  package.json
npm notice 110B  index.js
npm notice 146B  index.test.js
npm notice 1.1kB LICENSE
npm notice 81B   README.md
npm notice === Tarball Details ===
npm notice name:          @neil188/examplepackage
npm notice version:       0.1.0
npm notice filename:      neil188-examplepackage-0.1.0.tgz
npm notice package size:  1.2 kB
npm notice unpacked size: 1.7 kB
npm notice shasum:        f012191912fa8592f417b9bb4e8670d30ba047e4
npm notice integrity:     sha512-N/hGu1SNpFnvf[...]0Wtn/20JGhXFA==
npm notice total files:   5
npm notice
neil188-examplepackage-0.1.0.tgz
$ tar -xzf .\neil188-examplepackage-0.1.0.tgz
$ cd .\package\
$ dir


    Directory: D:\Users\Neil\Workspace\github\examplepackage\package


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----       26/10/1985     09:15            110 index.js
-a----       26/10/1985     09:15            146 index.test.js
-a----       26/10/1985     09:15           1068 LICENSE
-a----       26/10/1985     09:15            317 package.json
-a----       26/10/1985     09:15             81 README.md
```

Notice the package has included our index.test.js file - while we may want this in our git repository, we don't really need it in our npm package, and anyone who uses the package probably doesn't want any extra files they aren't going to use.

### Excluding files from the package

#### gitignore

First off, in the project root directory, if you try running the `npm pack` command again you will see that it will now include the package directory we just created, so the problem just got worse!  Obviously, you would just delete this directory as you don't need it anymore, but for this demonstration, we are going to leave the directory in place.

We also wouldn't want to accidentally upload this directory or the tarball file to GitHub, so let's add a .gitignore file

```text
package/
*.tgz
```

now try running `npm pack` again - note this time the package/ directory has not been included - if you don't have a .npmignore file, then npm will use the .gitignore file to exclude files.  

#### npmignore

Now try creating a .npmignore file to exclude our test file, populated with 

```text
index.test.js
```

Try running `npm pack` again, you should see index.test.js has been excluded, but now our package directory contents are back- because we now have a .npmignore file the process is no longer checking the .gitignore file, so we need to make sure all the files are included in the .npmignore file.

```text
index.test.js
package/
*.tgz
```

Running `npm pack` now should give you

```text
npm notice === Tarball Contents ===
npm notice 316B  package.json
npm notice 110B  index.js
npm notice 1.1kB LICENSE
npm notice 81B   README.md
```

which is what we want.

#### Package.json "Files"

An alternative method is to use the "files" field in package.json.  First, delete the .npmignore file so that doesn't exclude the files.  Then add files to you package.json:

```json
{
  "name": "@neil188/examplepackage",
  "version": "0.1.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "node index.test.js"
  },
  "keywords": [],
  "author": "N Lupton",
  "license": "MIT",
  "repository": {
    "type": "git",
    "url": "https://github.com/Neil188/examplepackage"
  },
  "files": []
}
```

Now try running `npm pack` again, you should see the following

```bash
npm notice === Tarball Contents ===
npm notice 331B  package.json
npm notice 110B  index.js
npm notice 1.1kB LICENSE
npm notice 81B   README.md
```

Note that we gave files an empty array, but four files have still been included - this is because certain files will always be included in your package:

* package.json
* README
* CHANGES / CHANGELOG / HISTORY
* LICENSE / LICENCE
* NOTICE
* The file in the "main" field

README, CHANGES, LICENSE & NOTICE are not case-sensitive and can have any extension.

as we don't need any extra file in our package "files": [] is all we need.

At this point, we can commit all the changes we've made to our files.

## Package version

Up to this point, the version number in package.json has been set to 0.1.0 (if your npm defaults are different and you didn't change package.json you could have a different version number).  When we publish our package though we want to use version 1 to show it is ready for use, a simple way of doing this is to use the [npm version](https://docs.npmjs.com/cli/version){{ site.new_tab }} command which will update the version and create a git tag:

```bash
$ npm version major
v1.0.0
```

Push your changes to GitHub using `git push` then you are ready to publish the package.

## Publishing a scoped package

To publish a package use command

```bash
npm publish
```

But unless you have paid subscription this should fail as by default all scoped packages are private.  To switch this to a public package use

```bash
npm publish --access=public
```

This will publish the package with public access after you have done this you can just use `npm publish` to update your package without needing to include the access option.

## Viewing your package online

If the publish command completed without errors you should now be able to view your repository online at www.npmjs.com/package/{userid}/{packagename}

for instance, this example is at [www.npmjs.com/package/@neil188/examplepackage](https://www.npmjs.com/package/@neil188/examplepackage){{ site.new_tab }}

You should see npm has used your README file as the description and picked up the version number, licence and repository fields from your package.json file.  You should now update your README to give a full description and include installation instructions for using your package.

These fields are also accessible via the command line, for instance, to view the GitHub repository for this enter command

```bash
npm repo @neil188/examplepackage
```
