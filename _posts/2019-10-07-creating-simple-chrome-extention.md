---
layout: post
title:  "Creating a simple chrome extension"
date:   2019-10-07 00:34:10 +0200
categories: chrome extensions
---
## How to Create Your First Chrome Extension -v2
Chrome extensions are small programs that will customize your browsing experience. This short guide will help you understand and create a nice and simple file structure for the Chrome extension.

### Chrome Extension Baseline
Chrome extensions are used to manipulate pages, extract additional data, intercept some HTTP requests and etc. Here, only your imagination is your boundary.

To start things off, you need to understand how to set up the baseline of your extension codebase. For an easier start, here is the official overview from Chrome: https://developer.chrome.com/extensions/overview.

I started using extensions when I wanted to customize my experience in any site, in my case – Asana. By default, Asana shows up a unicorn when I complete a task and I wanted to replace that with something that will cheer me up when I do something. That’s why I created an extension that is replacing the default animations with some memes.

Let say you have your code ready as an extension, but how can you tell the browser that this is your extension and it needs to be imported? To fully describe how extensions work, let’s see an example with an extension for Asana.

## File Structure

### 1. Manifest.json File 
First, you need to create a manifest.json file. What does this file do and what should you write in it? Basically, the manifest.json file in our config file is how the browser loads different permissions and resources. With this file, you point out to the browser that this is an extension, what version it is, its name, author, description, assets, and etc. Here is an example of how a simple manifest.json file should look.
```json
{
     "name": "DX Asana Goodies",
     "version": "1.1",
     "author": "Mariyan Dimitrov & Martin Spatovaliyski",
     "description": "extension for the cool teammates!",
     "manifest_version": 2,
     "web_accessible_resources": [
         "resources/**"
     ],
     "content_scripts": [{
         "js": ["jQuery.js", "popup.js"],
         "css": ["style.css"],
         "matches": [ "https://app.asana.com/*" ]
     }],
     "icons": {
        "128": "img/128.png" 
 	}
 }
```
To start developing your extension you need to include it in your browser and see how it behaves when you are writing lines of code. To do that, follow these steps:
* Open the Extension Manager page by going to this link – chrome://extensions/.
* Enable Developer Mode by clicking the toggle switch next to Developer mode at the top right side of your screen.
![This is my Chrome using dark mode in page chrome://extensions/]({{site.baseurl}}/images/creating-simple-chrome-extension/developer_mode.png)
* Click the Load unpacked button located in the top left corner of our window and select the extension’s folder.
![This is my Chrome using dark mode in page chrome://extensions/]({{site.baseurl}}/images/creating-simple-chrome-extension/load_unpacked.png)
* Select the folder in which your manifest.json file is in.
* If you selected the correct folder, you will see that extension’s card and the only thing that’s needed here is to activate it by clicking on the toggle button on the bottom right corner of your extension card.
![This is my Chrome using dark mode in page chrome://extensions/]({{site.baseurl}}/images/creating-simple-chrome-extension/extension_card.png)

### 2. “content_scripts” – Loading All Assets in Your Extension
The extension can also load custom assets as JavaScript and CSS and they should be added as:
__js/css__ – All styles and scripts that can be included: the order of loading the scripts will be followed by the order of the scripts (as described, we want jQuery to load first and our custom JavaScript second).
matches – Here you specify for which website(s) the resources should be loaded.
The __content_scripts__ will start loading as soon as a match is found from the matches array. In our example, this will be loaded only on https://asana.com. One thing to be noted though, the JavaScript files are loaded on interactive/DOM ready state.

Here is a simple example of how your JavaScript code could look like:
```javascript
alert("Hello World!");
```
Yup … nothing too fancy! You need to write your code as if you are pasting it in the console of your browser – ready for execution. When your extension is up and running you need to test and debug it. That means to reload the extension every time when you do a change, especially when changing the manifest.json file. That way, you are changing how the whole extension should behave and what resources should be loaded within it.

Google, however, has thoughtfully created functionality to reload the extension on the go by clicking the circular arrow on the bottom right corner of our extension card in chrome://extensions.
This is my Chrome using dark mode and this is my extension’s card.

### 3. Importing Extension Icon
If you want to customize your extension even more, you can add a custom icon. To do that, you need to add the following properties into your manifest.json file like showed into the example json file:
__icons__ – This is the property responsible for the icons the user sees when a user accesses the Chrome extensions page ( like in the icon above ) and the extension’s icons on the top right corner of your browser.
### 4. Accessible Resources 
Sometimes you may want your extension to load resources like pictures, mp3 files or any other type of files. Where should you store those mp3, pictures and etc.?

To load and give access to resources, you need to tell the manifest.json file that these files are actually there and can be used. You do that by adding the following piece of code in your JSON file. By using **, you are telling your extension to look for all files in the resources folder recursively.
```json
{
   "web_accessible_resources": [
      "resources/**"
   ]
}
```
The next step is to use the linked resources and that is done by using the function chrome.extension.getURL();. In our case we could actually assign the mp3 file into a variable and then use it later on like this:
```javascript
let musicURL = chrome.extension.getURL('/resources/' + memeName + '/music.mp3'  );
let music = new Audio(musicURL);
music.play();
```
When you start developing your extension, you will need to know where all of the errors (following/changing this implementation) will be logged. Chrome browser will keep a backlog with all the errors in the extension’s card ( button “Errors” ).
### Wrapping it up
You can find the other files’ content and here is how the whole extension’s structure looks like:
![This is my VS Code using dark mode]({{site.baseurl}}/images/creating-simple-chrome-extension/file_structure.png)
You can find the whole project at this [GitHub repo](https://github.com/mariyan-dimitrov/dank-customizer-extension-asana).

### Further FAQ
Do I need Google / Chrome permission to run an extension? – No. These are custom extensions that we run locally. It is like changing the HTML of a site locally. It is not violating any rules.
Are there any paid extensions? – Yes, there are many paid extensions. Some of them are single purchase and some of them are with a subscription plan.
Chrome extensions personalize your browser experience and make it more productive. They are easy to build and add, and a lot of fun to develop.
### CTA:
Ask your question about Chrome extensions here. DevriX front-end developers will be glad to share their experience with you.