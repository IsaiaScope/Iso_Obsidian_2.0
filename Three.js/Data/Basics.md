---
tags:
  - three_js
---

# Basics


## Introduction [00:00](https://threejs-journey.com/lessons/first-threejs-project#)[](https://threejs-journey.com/lessons/first-threejs-project#introduction)

  

Enough talking, now is the time to start coding and creating our first Three.js scene.

  

If you are familiar with nowadays front-end development tools like Node.js and Vite, you can probably skip most of this lesson.

  

Otherwise, stick with us and do not worry, as I’m going to explain everything in detail.

  

## Local server and build tools [00:33](https://threejs-journey.com/lessons/first-threejs-project#)[](https://threejs-journey.com/lessons/first-threejs-project#local-server-and-build-tools)

  

In order to run our soon-to-be Three.js website, we need to run a local server.

  

We could have created an HTML file somewhere on our computer and double-click on it, but browsers limit the functionalities of websites opened like this for security reasons. As a result, we won’t be able to load Three.js, models, textures, etc.

  

The simplest solution to this problem is to use a “build tool” or “bundler”.

  

### The state of build tools[](https://threejs-journey.com/lessons/first-threejs-project#the-state-of-build-tools)

  

There are many build tools available these days and you’ve probably heard of some of them like Webpack, Vite, Gulp, Parcel, etc.

  

They all have various features with pros and cons, but we are going to use a very specific one in the following lessons.

  

Nowadays, the most popular build tool is Webpack. It’s widely used, it has a great community and you can do a lot with it. But while Webpack is the most popular, it’s not the most appreciated.

  

In fact, the most appreciated build tool these days is [Vite](https://vitejs.dev/) (French word for "quick", pronounced `/vit/`, like "veet”). It’s faster to install, faster to run, and less prone to bugs. Ultimately, the developer experience is much better.

  

Initially, all Three.js Journey exercises were running on Webpack and some of the following lessons have been recorded like that. But now, exercises are running on Vite. Don’t worry, I’ve configured Vite so that the files and Vite’s behaviour look very similar to the Webpack configuration and you might not even notice the difference while following the videos.

  

### Vite[](https://threejs-journey.com/lessons/first-threejs-project#vite)

  

This is not a lesson about Vite, but because I can’t help myself, I’m going to explain a few things about it.

  

As mentioned earlier, [Vite](https://vitejs.dev/) is a build tool. The idea is that we are going to write web code like HTML/CSS/JS and Vite will build the final website. It’ll also do a bunch of things like optimizations, cache breaking, source mapping, running a local server, etc.

  

While Vite handles the most basic needs, we can also add plugins in order to handle more features like exotic languages, or special files. We are actually going to add plugins later in the course, which will be able to handle GLSL files in order to create custom shaders but also to run React.

  

Vite was created by Evan You, the creator of Vue.js, is highly maintained by hundreds of developers, and is getting a lot of hype.

  

### First Vite project[](https://threejs-journey.com/lessons/first-threejs-project#first-vite-project)

  

You might have noticed a **starter** file associated with this lesson. In later lessons, you’ll also find a **final** file.

  

The **starter** file is the one you’ll need to start in order to follow the exercise and the **final** file is the final result of the exercise in case you need it. But instead of using the **starter** file right away, I’m going to show you how to start a project from scratch. At a later stage, we are going to use that file.

  

### Terminal[](https://threejs-journey.com/lessons/first-threejs-project#terminal)

  

We are going to run some commands in the terminal. We could use the **Terminal** (MacOS) or **Command Prompt** (Windows), but I actually recommend you to run the terminal integrated into VSCode (if you are using VSCode like me). Launch VSCode and press `CMD + J` (MacOS) or `CTRL + J` (Windows) to open the integrated terminal.

  

If you do so, the following commands will be the same regardless of your OS.

  

The terminal works a bit like your file explorer and you need to be in the right folder to run the commands.

  

You can use `cd` followed by the name of the folder to browse in it. You can also use `cd` (with a space at the end), and drag and drop the folder. You can test where you are with `pwd` and list the files in the current folder with `ls`.

  

### Troubleshooting[](https://threejs-journey.com/lessons/first-threejs-project#troubleshooting)

  

We are going to do a bunch of commands and manipulations. If you are having any kind of issue, there is a section near the end of the lesson where I enumerate classic issues. If you still can’t make it work, as a member of Three.js Journey, you get access to a [private Discord server](https://threejs-journey.com/discord). Use the top menu to access the [Discord](https://threejs-journey.com/discord) page and share your issue there. We will help you out.

  

### Node.js[](https://threejs-journey.com/lessons/first-threejs-project#node-js)

  

In order to run Vite, you need to have [Node.js](https://nodejs.org/) installed on your computer.

  

Node.js enables running JavaScript on your computer outside of a browser. It’s great to run various tools like Vite, it has been around for many years and is very popular.

  

If you don’t know if Node.js is already installed or which version is installed run `node -v` from your terminal.

  

If the answer tells you that the `node` command isn’t recognised, then it’s not installed.

  

If it’s installed, the answer will contain the current version. Make sure it’s updated to the last version. Vite currently works with version `14.18` and above, but I recommend you always have the LTS (Long Term Support) version of Node.js installed.

  

To install or update Node.js, go to [https://nodejs.org/en/](https://nodejs.org/en/), download the “LTS” and install it with the default settings.

  

Close your Terminal (MacOS) or Command Prompt (Windows), re-open it, and run `node -v` again to check the version.

  

### Create a Node.js project[](https://threejs-journey.com/lessons/first-threejs-project#create-a-node-js-project)

  

Now that we have Node.js and we know how to use the terminal, it’s time to create our Node.js project.

  

Create a folder that will contain your website (try to stay organised, there are a bunch of lessons to come).

  

What you can do now is open that folder in VSCode (if you are using VSCode). This will come in handy for two main reasons:

  

-   The VSCode terminal will already be located in that folder.

-   We can manage the files more easily from the sidebar and those files won’t be hidden unlike with your OS file explorer.

  

Now open the terminal that should be located in that folder and run `npm init -y`.

  

When we installed Node.js, we automatically installed NPM. [NPM](http://npmjs.com/) stands for Node Package Manager and is a dependency manager that will fetch the packages we need such as Three.js, Vite, and the various libraries we are going to use in the course. Additionally, NPM comes with a command line interface named `npm` that we can use from our terminal.

  

By running `npm init -y`, NPM will create a `package.json` that will contain the minimal information needed to run a Node.js project.

  

### Add dependencies[](https://threejs-journey.com/lessons/first-threejs-project#add-dependencies)

  

In this lesson, we are going to use two dependencies. Vite and Three.js.

  

If you are curious, you can find them on the [NPM website](https://threejs-journey.com/lessons/npmjs.com):

  

-   [Vite](https://www.npmjs.com/package/vite) (as `vite`)

-   [Three.js](https://www.npmjs.com/package/three) (as `three`)

  

For now, we are going to focus on Vite only and add Three.js later. To add a dependency to a Node.js project, in the terminal, run `npm install vite`.

  

![](https://threejs-journey.com/assets/lessons/3/000.png)

  

You should notice three things.

  

-   A `node_modules/` folder has been created. This folder contains the project dependencies and you should never modify things inside that folder. Besides containing the `vite` dependency as the `vite/` folder, it also features the other dependencies that are being used by `vite`.

-   The `package.json` has been updated and now contains an array of dependencies written in the `"dependencies"` properties with numbers corresponding to the version with a level of tolerance.

-   A `package-lock.json` has been written and contains information about the dependencies and the exact versions that have been installed in your project without tolerance.

  

Those 2 files and the folder enable project sharing. If you want to share your beautiful Three.js website with another developer, you’ll want to remove the `node_modules` folder and share the rest of the project. The other developer will retrieve your project and run a command that will automatically re-create the `node_modules` folder according to what is written in `package.json`

  

Don’t know how to do that? Don’t worry, we are going to review that command shortly. Note that `package-lock.json` is optional, but if it’s present in the project, the other developer will get the exact same versions of the dependencies as you with no tolerance.

  

### Basic website[](https://threejs-journey.com/lessons/first-threejs-project#basic-website)

  

We have our Node.js project, we have Vite added as a dependency. Now, we need to create a basic website.

  

Still in the project folder, create an `index.html` file:

  

```html

<!DOCTYPE html> <html lang="en"> <head> <meta charset="UTF-8"> <meta name="viewport" content="width=device-width, initial-scale=1.0"> <title>03 - First Three.js Project</title> </head> <body> <h1>Soon to be a Three.js website</h1> </body> </html>

```

  

Do not open this file directly! Doing so would go against what we are doing right now with Vite.

  

In `package.json`, replace the `"scripts"` part with the following:

  

```json

{ // ... "scripts": { "dev": "vite", "build": "vite build" }, // ... }

```

  

By having those two “scripts”, we can now run those `dev` and `build` commands from the terminal and those commands will trigger `vite` and `vite build` respectively by using the `vite/` dependency from the `node_modules/` folder.

  

To run the `dev` script, in the terminal, run `npm run dev`.

  

![](https://threejs-journey.com/assets/lessons/3/001.png)

  

Vite should display a URL looking something like `http://localhost:5173/`. Copy it and paste it into your browser to open it like any website.

  

![](https://threejs-journey.com/assets/lessons/3/002.jpg)

  

Congratulations, you have your Vite website running.

  

Your terminal might seem stuck, but it’s actually running the server right now, and stopping the script will result in shutting down the server, which we will do shortly.

  

The `build` command will output the final version of the website. However, this is for a later lesson, where we will learn how to put our website live.

  

### Add JavaScript[](https://threejs-journey.com/lessons/first-threejs-project#add-javascript)

  

This is great, but now we want to execute some JavaScript on our beautiful website.

  

Still in the project folder, create a `script.js` file:

  

```javascript

console.log('JavaScript is working')

```

  

In the `index.html`, add the following `<script>` at the end, inside of the `<body>`:

  

```html

<!-- ... --> <body> <h1>Soon to be a Three.js website</h1> <script type="module" src="./script.js"></script> </body> </html>

```

  

Don’t forget the `type="module"`.

  

Open your **Developer Tools** (or **DevTools**) and you should see `JavaScript is working` in the **Console**.

  

![](https://threejs-journey.com/assets/lessons/3/003.jpg)

  

You might have noticed that you didn’t even have to reload the page. Vite detects the change and reloads the page for you.

  

## Three.js [37:12](https://threejs-journey.com/lessons/first-threejs-project#)[](https://threejs-journey.com/lessons/first-threejs-project#three-js)

  

Just like we added Vite to the project, we need to add Three.js.

  

### Add the dependency[](https://threejs-journey.com/lessons/first-threejs-project#add-the-dependency)

  

The name of the dependency on NPM is `three`.

  

We need to run `npm install three` but our terminal is currently busy running the Vite server.

  

You have two options:

  

-   Open a new terminal and run the command while making sure you are in the same folder.

-   Shut down the server, run the command, and start the server again.

  

We are going to shut down the server.

  

Press `CTRL + C` to stop the server. You might need to press the shortcut multiple times on a Windows OS or confirm with the letter `o`.

  

Now run `npm install three`:

  

![](https://threejs-journey.com/assets/lessons/3/004.png)

  

The `three/` folder should have been added to the `node_modules/` folder and both `package.json` and `package-lock.json` now contain references to it.

  

Restart the server with `npm run dev`:

  

![](https://threejs-journey.com/assets/lessons/3/005.png)

  

### Import Three.js[](https://threejs-journey.com/lessons/first-threejs-project#import-three-js)

  

Back to `script.js`, we can import Three.js from the `three` dependency by writing:

  

```javascript

import * as THREE from 'three'

```

  

This will import all core classes of Three.js inside the `THREE` variable from the `three` dependency.

  

This way of importing dependencies is called “ES modules”. We won’t dive too much into that topic, since it’s related to JavaScript basics more than Three.js. In the future, we will cover additional examples throughout the course, when we need them.

  

### Use Three.js[](https://threejs-journey.com/lessons/first-threejs-project#use-three-js)

  

Inside of our `script.js` file, we now have access to a variable named `THREE`. Be careful and always write it using uppercase.

  

If you `console.log()` this variable, you'll see that there is a lot going on inside:

  

```javascript

import * as THREE from 'three' console.log(THREE)

```

  

![](https://threejs-journey.com/assets/lessons/3/006.jpg)

  

The `THREE` variable contains most of the classes and properties you might need on a classic Three.js project. Unfortunately, not all classes are inside this variable, but we will see later how to access them.

  

To use one of those classes, you need to instantiate it. For example, if you want to create a scene, you'll write `const scene = new THREE.Scene()`. If you want to create a sphere geometry, you need to write `const sphereGeometry = new THREE.SphereGeometry(1.5, 32, 32)` —We'll dig deeper into these later.

  

### First scene[](https://threejs-journey.com/lessons/first-threejs-project#first-scene)

  

It's time to create our scene and produce something on the screen.

  

We need 4 elements to get started:

  

-   A scene that will contain objects

-   Some objects

-   A camera

-   A renderer

  

#### Scene[](https://threejs-journey.com/lessons/first-threejs-project#scene)

  

The scene is like a container. You place your objects, models, particles, lights, etc. in it, and at some point, you ask Three.js to render that scene.

  

To create a scene, use the [Scene](https://threejs.org/docs/index.html#api/en/scenes/Scene) class:

  

```javascript

// Scene const scene = new THREE.Scene()

```

  

#### Objects[](https://threejs-journey.com/lessons/first-threejs-project#objects)

  

Objects can be many things. You can have primitive geometries, imported models, particles, lights, and so on.

  

We will start with a simple red cube.

  

To create that red cube, we need to create a type of object named [Mesh](https://threejs.org/docs/#api/en/objects/Mesh). A [Mesh](https://threejs.org/docs/#api/en/objects/Mesh) is the combination of a geometry (the shape) and a material (how it looks).

  

There are many geometries and many materials, but we will keep things simple for now and create a [BoxGeometry](https://threejs.org/docs/index.html#api/en/geometries/BoxGeometry) and a [MeshBasicMaterial](https://threejs.org/docs/#api/en/materials/MeshBasicMaterial).

  

To create the geometry, we use the [BoxGeometry](https://threejs.org/docs/index.html#api/en/geometries/BoxGeometry) class with the first 3 parameters that correspond to the box's size.

  

```javascript

// Object const geometry = new THREE.BoxGeometry(1, 1, 1)

```

  

To create the material, we use the [MeshBasicMaterial](https://threejs.org/docs/index.html#api/en/materials/MeshBasicMaterial) class with one parameter: an object `{}` containing all the options. All we need is to specify its `color` property.

  

There are many ways to specify a color in Three.js. You can send it as a JS hexadecimal `0xff0000`, you can send it as a string hexadecimal `'#ff0000'`, you can use color names like `'red'`, or you can send an instance of the [Color](https://threejs.org/docs/index.html#api/en/math/Color) class —we'll cover more about it later.

  

```javascript

// Object const geometry = new THREE.BoxGeometry(1, 1, 1) const material = new THREE.MeshBasicMaterial({ color: 0xff0000 })

```

  

To create the final mesh, we use the [Mesh](https://threejs.org/docs/index.html#api/en/objects/Mesh) class and send the `geometry` and the `material` as parameters.

  

```javascript

// Object const geometry = new THREE.BoxGeometry(1, 1, 1) const material = new THREE.MeshBasicMaterial({ color: 0xff0000 }) const mesh = new THREE.Mesh(geometry, material)

```

  

You can now add your mesh to the scene by using the `add(...)` method:

  

```javascript

scene.add(mesh)

```

  

If you don't add an object to the scene, you won't be able to see it.

  

#### Camera[](https://threejs-journey.com/lessons/first-threejs-project#camera)

  

The camera is not visible. It's more like a theoretical point of view. When we will do a render of your scene, it will be from that camera's point of view.

  

You can have multiple cameras just like on a movie set, and you can switch between those cameras as you please. Usually, we only use one camera.

  

There are different types of cameras, and we will talk about these in a future lesson. For now, we simply need a camera that handles perspective (making close objects look more prominent than far objects).

  

To create the camera, we use the [PerspectiveCamera](https://threejs.org/docs/index.html#api/en/cameras/PerspectiveCamera) class.

  

There are two essential parameters we need to provide.

  

**The field of view**

  

The field of view is how large your vision angle is. If you use a very large angle, you'll be able to see in every direction at once but with much distortion, because the result will be drawn on a small rectangle. If you use a small angle, things will look zoomed in. The field of view (or `fov`) is expressed in degrees and corresponds to the vertical vision angle. For this exercise we will use a `75` degrees angle.

  

Here's a video to explain what the field of view variation looks like:

  

**The aspect ratio**

  

In most cases, the aspect ratio is the width of the canvas divided by its height. We haven't specified any width or height for now, but we will need to later. In the meantime, we will create an object with temporary values that we can re-use.

  

Don't forget to add your camera to the scene. Everything should work without adding the camera to the scene, but it might result in bugs later:

  

```javascript

// Sizes const sizes = { width: 800, height: 600 } // Camera const camera = new THREE.PerspectiveCamera(75, sizes.width / sizes.height) scene.add(camera)

```

  

#### Renderer[](https://threejs-journey.com/lessons/first-threejs-project#renderer)

  

The renderer's job is to do the render. Bet you didn't see that coming!

  

We will simply ask the renderer to render our scene from the camera's point of view, and the result will be drawn into a canvas. You can create the canvas by yourself, or let the renderer generate it and then add it to your page. For this exercise, we will add the canvas to the HTML and send it to the renderer.

  

In `index.html`, instead of the `<h1>`, create the `<canvas>` element **before** you load the scripts and give it a class:

  

```html

<canvas class="webgl"></canvas>

```

  

To create the renderer, we use the [WebGLRenderer](https://threejs.org/docs/index.html#api/en/renderers/WebGLRenderer) class with one parameter: an object `{}` containing all the options. We need to specify the `canvas` property corresponding to the `<canvas>` we added to the page.

  

Create a `canvas` variable at the start of the code, then fetch and store in it the element we created in the HTML using `document.querySelector(...)`.

  

It's better to assign the canvas to a variable because we'll use it for other purposes in the next lessons.

  

We also need to update the size of your renderer with the `setSize(...)` method using the `sizes` object we created earlier.

  

The `setSize(...)` method will automatically resize our `<canvas>` accordingly:

  

```javascript

// Canvas const canvas = document.querySelector('canvas.webgl') // ... // Renderer const renderer = new THREE.WebGLRenderer({ canvas: canvas }) renderer.setSize(sizes.width, sizes.height)

```

  

Right now, you won’t be able to see anything, but your canvas is there and has been resized accordingly. You can use the Developer Tools to inspect the `<canvas>` if you are curious.

  

### First render[](https://threejs-journey.com/lessons/first-threejs-project#first-render)

  

It's time to work on our first render. Call the `render(...)` method on the renderer and send it the `scene` and the `camera` as parameters:

  

```javascript

renderer.render(scene, camera)

```

  

![](data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 3072 1920'%3E%3C/svg%3E)

  

A black screen? Where is our object?

  

Here's the issue: we haven't specified our object's position, nor our camera's. Both are in the default position, which is the center of the scene and we can't see an object from its inside (by default).

  

We need to move things.

  

To do that, we have access to multiple properties on each object, such as `position`, `rotation`, and `scale`. For now, use the `position` property to move the camera backward.

  

The `position` property is an object with three relevant properties: `x`, `y` and `z`. By default, Three.js considers the forward/backward axis to be `z`.

  

To move the camera backward, we need to provide a positive value to that property. You can do that anywhere once you've created the `camera` variable, yet it has to happen before you do the render:

  

```javascript

const camera = new THREE.PerspectiveCamera(75, sizes.width / sizes.height) camera.position.z = 3 scene.add(camera)

```

  

![](data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 3072 1920'%3E%3C/svg%3E)

  

Congratulations, you should see your first render. It looks like a square, and that's because the camera aligns perfectly with the cube, and you can see only one side of it.

  

Don't worry about the render's size; we'll learn how to make the canvas fit the viewport later.

  

In the next lessons, you will learn more about the `position`, `rotation`, and `scale` properties, how to change them, and animate the scene.

  

## Using the starter [01:04:55](https://threejs-journey.com/lessons/first-threejs-project#)[](https://threejs-journey.com/lessons/first-threejs-project#using-the-starter)

  

While we’ve learned how to do it from scratch, a **starter** file is provided with every exercise so that you don’t have to go through the whole process every time.

  

Moreover, you can actually find a **starter** with this lesson.

  

Most following lessons will work just the same and I’m going to explain how to use those files in order to complete the exercises as you follow the lessons.

  

### Zip files[](https://threejs-journey.com/lessons/first-threejs-project#zip-files)

  

Download and unzip the **starter** file (right below the player).

  

Unzipping a file depends on your OS and which applications you have installed. On MacOS, you can double-click on it. On Windows, you should right-click and choose something like `Extract…`, followed by options like the folder in which it’ll be unzipped.

  

### Dependencies[](https://threejs-journey.com/lessons/first-threejs-project#dependencies)

  

You might have noticed that the `node_modules/` folder isn’t there. As mentioned earlier, when sharing a Node.js project, do not share the `node_modules/` because this folder can get tremendously big.

  

Yet, we need the dependencies and those are actually listed in the `package.json` file. To install those listed dependencies, open the terminal in the project folder and run `npm install`:

  

![](data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 885 375'%3E%3C/svg%3E)

  

Wait for a little while dependencies are being fetched and you should see a `node_modules` folder being created alongside the `package-lock.json`.

  

### Run the server[](https://threejs-journey.com/lessons/first-threejs-project#run-the-server)

  

Now, to run the server, from the terminal in the project folder, run `npm run dev`.

  

Wait a second or two and the website should open in your default browser.

  

If the page doesn’t open, the terminal should display two URLs that look something like `http://localhost:5173/` and `http://192.168.1.25:5173/`.

  

Try them in your browser:

  

![](data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 3072 1920'%3E%3C/svg%3E)

  

The configuration is slightly more advanced than what we’ve done earlier from scratch.

  

There is a `vite.config.js` in which I provide some specific settings. We won’t go into details because we’ve spent already too much time talking about Vite. Yet, if you are curious, you can find all the information you need on the [Vite documentation](https://vitejs.dev/guide/).

  

## Troubleshooting [01:09:40](https://threejs-journey.com/lessons/first-threejs-project#)[](https://threejs-journey.com/lessons/first-threejs-project#troubleshooting-1)

  

If everything’s worked out, you can skip this section.

  

If you’ve had issues along the way, I’m going to enumerate some classic troubleshooting and how to fix those issues.

  

### Terminal folder[](https://threejs-journey.com/lessons/first-threejs-project#terminal-folder)

  

Make sure your terminal is opened in the project folder.

  

Use the `pwd` command to display the folder in which the terminal is currently.

  

Use the `cd` (with a space at the end) command, drag and drop the folder, and then press `Enter` to move the terminal into that folder.

  

### Long path[](https://threejs-journey.com/lessons/first-threejs-project#long-path)

  

If your project folder is nested very deep in other folders, you might end up with a path so long that Node.js can’t handle it.

  

Move the folder up and make sure to move the Terminal accordingly before trying again.

  

### Versioned folder[](https://threejs-journey.com/lessons/first-threejs-project#versioned-folder)

  

Be careful with tools like OneDrive, Google Drive, Dropbox, etc. that will “save” your files online.

  

They can mess up your Node.js dependencies.

  

Try to move the project outside of those “saved” folders.

  

### Special characters in the path[](https://threejs-journey.com/lessons/first-threejs-project#special-characters-in-the-path)

  

Avoid having special characters in the path from the root of your computer down to the project.

  

Ideally, you should only have lowercase letters, numbers, dashes, and underscores.

  

### MacOS and Xcode[](https://threejs-journey.com/lessons/first-threejs-project#macos-and-xcode)

  

MacOS might ask you to install “Xcode Command Line Tools”.

  

It’s usually a harmless warning, but sometimes it prevents the Node.js project from running.

  

If you want more info, follow [this article](https://medium.com/@mrjohnkilonzi/how-to-resolve-no-xcode-or-clt-version-detected-d0cf2b10a750). But to sum it up, just run `xcode-select --install` from your terminal and follow the instructions.

  

### Permissions[](https://threejs-journey.com/lessons/first-threejs-project#permissions)

  

Sometimes, permissions are messed up and NPM can’t install the dependencies.

  

If you know what you are doing, try to fix the permissions. Otherwise, delete the folder, re-download the starter, and follow the instructions again.

  

### Vulnerabilities[](https://threejs-journey.com/lessons/first-threejs-project#vulnerabilities)

  

While you install dependencies, NPM might warn you about “vulnerabilities”. Those are most of the time (if not always) harmless. I’m keeping the course dependencies up to date regularly in order to avoid those warnings but you should not worry too much.

  

Do not try to fix them using the command suggested by NPM, otherwise, you might end up with different versions than the ones we use in the lesson, resulting in bugs.

  

### Still not working[](https://threejs-journey.com/lessons/first-threejs-project#still-not-working)

  

If you can't find the problem, go to the course's Discord server and share any information you have. We will help you out.

  

## More about the Vite configuration [01:15:06](https://threejs-journey.com/lessons/first-threejs-project#)[](https://threejs-journey.com/lessons/first-threejs-project#more-about-the-vite-configuration)

  

Here are a few things you need to know about the Vite configuration and some reminders:

  

-   You need to run `npm install` only once after downloading the project.

-   You need to run `npm run dev` each time you want to run the server and start coding. Your terminal might seem stuck, but it's perfectly normal, and it means that the server is running.

-   The only folders you need to explore, are the `src/` and the `static/` folders.

-   In the `src/` folder, you can find the traditional `index.html`, `script.js`, and `style.css` files.

-   You can put static files (also called “public files”) in the `static/` folder. Those files will be served as if they were available in the root folder (without having to write `static/`). You can test by adding `/door.jpg` at the end of the URL (`http://localhost:5173/door.jpg`). We'll use this to load textures and models later.

-   The page will automatically reload as you save any of those files.

-   You can access your local server from any other device on the same network by typing the same URL that looks something like this `http://192.168.1.25:5173`. It’s very useful to debug on other devices like mobiles. If it’s not working, it’s usually because of your firewall.

  

## Conclusion [01:18:45](https://threejs-journey.com/lessons/first-threejs-project#)[](https://threejs-journey.com/lessons/first-threejs-project#conclusion)

  

And that’s it. We are ready to go through the many lessons and use the various exercises.

  

If you are not confident about doing that again on your own, don’t worry, we are going to do it together in the next lesson.

  

Remember that some of the following lessons have been recorded using Webpack and you might see slight differences. You should be able to ignore those differences.

  

How about we do some Three.js now?

---
