---
layout: post
title:  "Creating a simple chrome extention"
date:   2019-10-07 00:34:10 +0200
categories: chrome extentions
---
Chrome extentions are very easy ways to manipulate pages the way we want. We can also extent the functonality of a page/s by writing a custom extention that will do the things we need. To start things off, we need to understand how can we setup the baseline of our extention codebase.

## Chrome Extention baseline
Let's say we have a piece of code that needs to be runned in a page, but under an extention. How can we tell the browser that this is an extention and we need to import it. Well, to describe as fully as I can the idea about that I will show mine [extention](https://github.com/mariyan-dimitrov/Cool-Asana-Extention) for [Asana](https://app.asana.com/).

## File structure

### Manifest.json - what does it do and what should we write in it

Basically, the manifest.json file is our config file from which the browser loads different permissions and resources. We also point out to the browser what version we are loading, the extension's name, author and description, which can be viewed from the browser when the extension is loaded properly.

First we need to create a _manifest.json_ file and add these lines of code into it:
```json
{
    "name": "DX Asana Goodies",
    "version": "1.1",
    "author": "Mariyan Dimitrov & Martin Spatovaliyski",
    "description": "Extention for the cool teammates!",
    "manifest_version": 2,
    "web_accessible_resources": [
        "resources/**"
    ],
    "content_scripts": [{
        "js": ["jQuery.js", "popup.js"],
        "css": ["style.css"],
        "matches": [ "https://app.asana.com/*" ]
    }],
    "default_icon": {
        "16": "img/16.png",
        "32": "img/32.png",
        "128": "img/128.png"
    },
    "icons": {
        "16": "images/salimeyes16.png",
        "32": "images/salimeyes32.png",
        "48": "images/salimeyes48.png",
        "128": "images/salimeyes128.png"
    }
}
```

### Importing extension icon
__default_icon__ - this property is responsible for the icon the user sees in his extension toolbar when the estension is active.
__icons__ - this is the property responsible for the icon the user sees when he access the __Chrome extentions__ page and importing our plugin.

### content_scripts - loading all assets in our extension
__js/css__ - All styles and scripts that can be included: the order of loading the scripts will be followed by the order of the scripts ( as described, we want jQuery to load first and our custom JavaScript second).
__matches__ - For which website should the resources be loaded.
 
The __content_scripts__ will start loading as soon as a match is found from the __matches__ array. In our case, this will be loaded only in app.asana.com. One thing to be noted though, the JavaScript files are loaded on `interactive/DOM` ready state.

### Accessible resources - where should we store our mp3, pictures and etc.
Sometimes we want our extention to load some pictures, mp3 files or any other type of files. To have access to them, we need to tell the manifest.json file that these files are actually there and can be used. We do that by adding this piece of code in our json file. By using `**`, we are currently telling our extension to look for all files in `resources` folder recursively.
```json
    {
        "web_accessible_resources": [
            "resources/**"
        ]
    }
```
Next step is to use the linked resources and that is done by using the function `chrome.extension.getURL();`. In our case we could actually assign the mp3 file into a variable and then use it later on like this:

```javascript
let musicURL = chrome.extension.getURL('/resources/' + memeName + '/music.mp3'  );
let music = new Audio(musicURL);
music.play();
```

### Loading the created extension
When we start developing our extension, we need to include it in our browser and see how does it behaves when we are writing lines of code. To do that we need to follow these steps:
1) Open the __Extension Manager__ page by going to __chrome://extensions__.
- You can also open it by clicking on the Chrome menu, hovering over More Tools then selecting Extensions.

2) Enable Developer Mode by clicking the toggle switch next to Developer mode.

3) Click the `Load unpacked` button located in the top left corner of our window and select the extension's folder.

4) Activate yout extension by clicking on the toggle button on the bottom right corner of your extension card.
### Debugging the loaded extension
When your extension up and running we need to debug it. We need to reload the extension every time when we do a change, speciialy when we change the __manifest.json__ file. That way, we are changing how the whole extension should behave and what resources should be loaded within it, that's why we need to reload it. For easier debugging, Google is thoughfull enough to create an functionality to reload the extension on the go by clicking the circular arrow on the bottom right corner of our extension card in __chrome://extensions__. Many time through our work, we will make mistakes and we need to learn from them. All of the errors (following this implementation) will be logged in the console of your browser. At the sametime however, our Chrome browser will keep a backlog with all errors that were throwed before.


References:
 - https://developer.chrome.com/extensions/background_pages