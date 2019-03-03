---
layout: post
title:  "Creating Gutenberg blocks and a little more!"
date:   2019-03-03 19:48:10 +0200
categories: gutenberg blocks
---
First things first, we need to create a new block so we can understand how it works. To do that we need to have npm version >=5.2. New custom block means that we will create a new plugin that we can activate in the site's WP homepage.


#### **Create the plugin**

After we are done with the local installation of the project we need to go into localProject/wp-content/plugins/ with the terminal. Typing **npx create-guten-block my-block** will create a custom block with the name "my-block". Now we should activate the plugin from WP.


#### **Activate the plugin**

Activate the created plugin from the **Plugins** section from WordPress side of your project.


#### **Run/compile the plugin**

After we are done with the **installation** and **activation** of the plugin, we need to run it. That is done with the command **npm run start**. That will listen to changes in the plugin and will compile it on every change. After you are ready with all modifications and you need to push it to production, run the command **npm run build**.


#### **Understanding the structure of the block/plugin**

First thing we need to do is to run **npm run start** so that the npm will listen to our changes. This is what we should be able to see:

![Console in Compiling]({{site.baseurl}}/images/creating-blocks-and-little-more/compiling.jpeg)


NOTE: if you get this kind of error while working with your block, just remove the block from the already saved page, save it again and the error will be gone



![Error whilw Compiling]({{site.baseurl}}/images/creating-blocks-and-little-more/error.jpeg)



In src/block we have **block.js, editor.scss and style.scss.** The **editor.scss** is the style for the editors/backend page in WP, but the **style.scss** is the style for **both_ front-end_** page and _editor_/backend page.

In block.js things get little bit confusing, but we will cover most of the stuff. We have one main function (**registerBlockType**) with some arguments.



*   **title**: This is the name of the block we will see in the editors page
*   **icon**: The icon we will see for this block if we want to change it from the default "shield" icon, we can go to [here](https://developer.wordpress.org/resource/dashicons/)`.` Type the type of dashicon without the **dashicon** word.
*   **category**:  In which section of the Gutenberg's block we will find our custom one.
*   **attributes**: There are our variables that we will work with. They will be used to pass information from **edit/backend** to **save/frontend** functions/views. Every attribute should have type value (_boolean, string, number, array_ or _object_) and its recommended to have a default value ( they will be initialized with it ).



![registerBlockType]({{site.baseurl}}/images/creating-blocks-and-little-more/registerBlockType.jpeg)





*    **edit:** This is the function for the editors page. This function's return is like render in React – renders the JSX. This also means that we can have "html" values in an array attribute and then render it on **edit and/or save** function (we will talk on this on the next follow up post). Changing the defined attributes is done with the function **setAttributes**, defined in the function's arguments (this is like Reacts' **setState**). But in order to use these attributes we need to include them in our function.


![Edit function]({{site.baseurl}}/images/creating-blocks-and-little-more/edit.jpeg)



Then we need to define some function to be able to change the already defined attributes. 

Every component has:



*   **onChange/onSelect:** they will be called every time we change/select some content of the component from the backend/editor. This function will be called with some argument and this argument is the current selected content ( text, image and etc.). We will take current argument and will pass it to our defined function, so we can change our main attributes. 
*   **value:** this is the current value of the component. We refer this value to the specific attribute we define. When we change this attribute, the value of the component also changes instantly. If you define the attribute with a default value, this value will be loaded in the component.
*   **className:** this is for styles and refers to html's class.

To find the components used in Gutenberg and how they work, you can go [here](https://github.com/WordPress/gutenberg/tree/master/packages/editor/src/components).

In **save**,things are more simple, we just need to take the attributes from function's arguments and then render them with the **return** This function will be called every time you save your current work at the backend/editor.

![Save function]({{site.baseurl}}/images/creating-blocks-and-little-more/save.jpeg)


There will be a follow up posts for how to advance our block with **_InspectorControls,PanelBody_** ( the Settings section in the editor)_, **MediaUpload** with mutiple pictures_, how to use jQuery in Gutenberg in general, how to work with objects and arrays as attributes, etc ) .
