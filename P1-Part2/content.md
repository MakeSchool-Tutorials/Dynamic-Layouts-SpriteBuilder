---
title: Dynamic Layouts with SpriteBuilder and Cocos2D 3.x - Part 2
slug: part-2
---               


In this part of the tutorial we will be applying the theoretical knowledge of part 1 to implement three different tablet versions of the same game.

# Three ways to make your game tablet compatible

Now that you got the gist of the different layout options we will apply them to real world examples. When you decide to make your game tablet compatible you can basically choose between three different ways to do this:

*   **Scale up with flexible screen mode:** scale the gameplay up and position the UI elements relative to the screen size. This approach combines the advantages of simply scaling the gameplay and dynamically positioning the UI elements.
*   **Scale up with fixed screen mode:**<span style=""> scale everything up on tablets. Maintain the exact same positions and sizes for the gameplay and the UI elements. This approach can be even simpler than the first ones, but the results are less appealing. </span>
*   **Increase visible area:** if you have a side scroller or any other type of game involving some sort of camera you can choose to display a wider area of  the screen.

It is worth mentioning that there are more than only these three options, you can combine parts of these three approaches, you can load completely independent different interface files for different device types - however, these three approaches are the most important and they will teach you all the details about the scaling and positioning system that you will need make your own game tablet compatible. Let's take the dive.

# Option 1: Scale up the gameplay (flexible screen mode)

This is the preferred approach for scaling up your gameplay content and most likely the one that you will be using most, so we will look at it first.

## Getting started

Create a new SpriteBuilder project to get started. Remove all nodes that come with the template project.

## Setting up a fake Gameplay Scene

As this tutorial is about layouts, sizing, and device compatibility we will not create an actual game. Instead we will create a small Gameplay scenes that *could* represent an actual game. Then we use these fake scenes to apply different scaling and sizing options.

Create a new *Layer* called *Gameplay.ccb* and give it a size of 480 x 320 (the size of the safe area).

We choose 480 x 320 because for this approach (scaling the gameplay up) we are going to create a gameplay that fits on a single screen. Now [download our art pack for this tutorial](https://s3.amazonaws.com/mgwu-misc/LayoutingArtPack.zip).

Unzip the art pack and drag it to SpriteBuilder in order to add it to your project. Next, place an arbitrary amount of game objects in *Gameplay.cbb,* once again, the only goal we have here is simulating a single screen gameplay scene. When you are done your *Gameplay.ccb* could look similar to this:

![](https://static.makegameswith.us/gamernews_images/1ZumQCHYKT/Screen Shot 2014-02-21 at 10.14.53.png)

Next you need to set the anchor point of the root node of the *Gameplay.ccb* to (0.5, 0.5) so that we can center the gameplay scene within the *MainScene.ccb* (In SpriteBuilder 1.0.3 there is an issue with the representation when you set the anchor point on the root node of a CCB-File, it will be fixed in future releases).

Add the *Gameplay.ccb* to the *MainScene.ccb* by dragging it onto the stage. And center the *Gameplay.ccb* by expressing the position *in % of parent container* and choosing a position of (50,50):

![](https://static.makegameswith.us/gamernews_images/Pe5B9sqjIE/Screen Shot 2014-02-25 at 08.25.41.png)

Next we need to add some padding for this scene. Add the  *background.png* asset to the scene and change the order in the timeline to place it *behind* the game objects:

![](https://static.makegameswith.us/gamernews_images/yB3dLDvfPQ/Screen Shot 2014-03-03 at 11.26.32.png)

Set the position to be 50% of the parent's width and height and set the anchor point to (0.5, 0.5) to center the background behind the gameplay.

The great advantage of scaling the game up while using the *flexible* screen mode is that we can create dynamically resizing UIs by defining positions of UI elements relative to the screen borders.

Let's add a button to the top right  corner of *Gameplay.ccb* to demonstrate this. Drag a button from the component library on the left to the stage:

![](https://static.makegameswith.us/gamernews_images/eRJ2WO2xtu/Screen Shot 2014-02-25 at 08.34.59.png)

Set the *reference corner* of the button to the top right corner. Set the anchor point of the button to (1.0,1.0), which is the top right corner of the button. This way we can define distance between the right edge and the top edge of the screen and the button. Choose (10,10) as position. Now our button will have a distance of 10 points to each screen border on any device type. Since we are using *Points* and not *UIPoints* that distance will be scaled up on tablets.

Now run the game on multiple device types and take a look at the results:

![](https://static.makegameswith.us/gamernews_images/qDHVWM0isC/ResultsFlexibleScaleMode.png)

You can see a couple of things when comparing the layout on different devices:

*   The button is always placed in the top right corner with a margin of (10,10)
*   A larger portion of the background image becomes visible on larger devices
*   All image sizes on tablets increase (using the 4x resolution), so that the images use a similar fraction of the screen as on phones

**This approach is the preferred option when you want to scale your game up. It is a simple approach that lets you create nice User Interfaces by using relative positioning.** 

If you need an even simpler approach where nodes are positioned exactly the same across multiple devices you should take a look at option 2, the *fixed* screen mode.

# Option 2: Scale up the gameplay (fixed screen mode)

If you don't want to deal with relative layouts as you need to when using approach 1, you should take a look the **fixed** screen mode that we discussed briefly at the beginning of part 1 of this tutorial. It however also comes with a disadvantage: you cannot place anything relevant in the unsafe area of the screen. This results in some limitations when creating UI elements. You'll get to see them soon.

To get started with this approach, change the project settings to use the fixed screen mode:

![](https://static.makegameswith.us/gamernews_images/QPjUGWsM4s/Screen Shot 2014-02-25 at 09.05.45.png)

Once you change this setting you will see that the appearance of the stage changes. The safe area of the screen is 480 x 320 (the size of the 3.5 inch iPhone). The size of the complete screen, including the unsafe area is 568 x 384:

![](https://static.makegameswith.us/gamernews_images/SEHXixOZma/Screen Shot 2014-02-25 at 09.08.07.png)

As you can see the button in the top right corner is placed in the unsafe area. The unsafe are will not be visible on all devices so we cannot position buttons there. You need to move the button to the safe area instead:

![](https://static.makegameswith.us/gamernews_images/Ls4O3rS5Ec/Screen Shot 2014-02-25 at 09.09.49.png)

This was the only additional change that was necessary to run the game in *fixed* screen mode. You should only use the *fixed* screen mode when you have problems implementing your layout in the *flexible* screen mode. The *fixed* screen mode is a little simpler, because the root node has the same size on any device, however that results in many limitations.

Publish the SpriteBuilder project and run it the game in Xcode. Look at the results when running the game on different device types:

![](https://static.makegameswith.us/gamernews_images/eK5dSjzqRn/ResultsFixedMode (2).png)

Here are the differences compared to option 1 (*flexible* screen mode):

*   The button is also always positioned at the exact same position
*   The only thing that changes on different devices is the amount of padding that is visible

As you can see the iPad version does not look as elegant as in the first approach because the button is not nicely aligned at the edge of the screen.

**Use this approach when you want to scale your game up and have problems with using relative layouts. You will need more effort to come up with UI designs that look great on all devices when using this option.**

# Option 3: Increase the visible area of the Gameplay

Scaling up isn't great for all types of games. A strategy game for example would benefit from simply seeing a larger portion of the gameplay instead of scaling it up. So let's take a look at how we can create an unscaled tablet version of this game.

The basic principle for this approach is: when we do not want to scale the game up, we need to configure the SpriteBuilder project to use exactly the same assets  on phones and tablets instead of using the larger (4x) assets for the tablet version.

First, let's switch our project back to *flexible* screen mode to implement this third option:

![](https://static.makegameswith.us/gamernews_images/ZlwgHJBuyN/Screen Shot 2014-02-21 at 10.29.40.png)

Now we are going to change our Gameplay scene. We want to simulate that our game is not a single screen game but actually has content that extends beyond the visible area of the screen (like in our [Peeved Penguins project](https://www.makegameswith.us/tutorials/getting-started-with-spritebuilder/)).

## Change the background image

For this approach we need a new background image. We want to use the same asset for the background image on the iPhone and on the iPad (instead of the 4x asset we used for the tablet version so far). This means the background image needs to be lot higher to fill out the full iPad screen. If you can't follow this chain of thought yet, don't worry, it will become clearer once we start working with it. [Download the new background image](https://s3.amazonaws.com/mgwu-misc/SpriteBuilderLayoutingTutorial/background_large.png) and add it to the SpriteBuilder project.

Remove the old background image from the scene and add the new one:

![](https://static.makegameswith.us/gamernews_images/jnAgP9hk0z/Screen Shot 2014-02-25 at 09.29.55.png)

**Select the image in the left panel and set the tablet resolution to *1x***. This way phone and tablet will use exactly the same background image. Set the position and the anchor point of the background image to (0,0). When you switch between the different resolutions (phone, tablet) you will see that the background image does not resize anymore, instead a larger portion of the background image becomes visible.

## Change the positioning and scaling options

You have successfully completed the first step - the background image does not resize anymore. Now we need to apply this to all other objects in the game.

Change the resolution setting **for every resource in the *LayoutArtPack*** to use the *1x* resolution on tablets:

![](https://static.makegameswith.us/gamernews_images/aHfWby23rM/Screen Shot 2014-02-25 at 09.32.24.png)

Double check you applied this change for every resource!

If you preview the iPad version after applying all the changes the result should look similar to this:

![](https://static.makegameswith.us/gamernews_images/dfWwPmYRBs/Screen Shot 2014-02-25 at 09.34.09.png)

The images aren't scaling up anymore which is good news, but they all appear a little misplaced. We will be fixing this in *Gameplay.ccb*.

Open the *Gameplay.ccb* file and set the anchor point of the root node to (0,0) since we will not be centering the gameplay in this approach anymore:

![](https://static.makegameswith.us/gamernews_images/Sg89cwneO8/Screen Shot 2014-02-25 at 09.35.38.png)

Now, let's fix the misplaced objects. If you remember the first part of this tutorial you may know why the objects appear spread apart on the iPad. Currently we are using *Points* to define positions for all the nodes in *Gameplay.ccb*. *Points* are scaled up on tablets. If we want to define absolute, unscaled positions we need to use *UI Points*.

As a next step, change the position type of **all objects on the stage** to *UI Points*:

![](https://static.makegameswith.us/gamernews_images/Ddayn89MCC/Screen Shot 2014-02-24 at 11.34.01.png)

Now open *MainScene.ccb* and change the position of the *Gameplay.ccb* embedded within it. We don't need to center the gameplay anymore, as we did in the fixed screen mode. Instead we want it to be placed at the same position for all devices. Drag the *Gameplay.ccb* to the left bottom corner and set the position type to *UI Points*:

![](https://static.makegameswith.us/gamernews_images/aEPpmtAx0l/Screen Shot 2014-02-25 at 09.41.44.png)

Well done! None of the images or positions are resizing anymore. In the tablet version we simply get to see a larger piece of the landscape.

Since we are using the *flexible* screen mode again, we can position UI elements relative to the screen edges. Let's move the "Main Menu" button to the top right corner again:

![](https://static.makegameswith.us/gamernews_images/nhv4fnXhqZ/Screen Shot 2014-02-25 at 09.43.48.png)

## Increase the gameplay layer size

Let's make one last change to show the advantages of this approach a little clearer. We will increase the width of the *Gameplay.ccb* and add a couple of game objects to the right part of the layer. That way it will be clearly visible that the iPad version displays a larger portion of the game world content.

Open *Gameplay.ccb* and change the stage size of the stage be 1024 points wide (the screen width of the iPad), by selecting *Document -&gt; Edit Stage Size* from the top menu:

![](https://static.makegameswith.us/gamernews_images/oSGGoGefCV/Screen Shot 2014-02-24 at 11.40.58.png)

Also change the size of the root node to be defined in *UI Points* (we don't want the gameplay scene to scale up on tablets) and have a width of 1024:

![](https://static.makegameswith.us/gamernews_images/Bs2ayUioNW/Screen Shot 2014-02-24 at 11.42.27.png)

As a last step add a couple more objects to the right part of the scene. Ensure that the position type for all of these is *UI Points*:

![](https://static.makegameswith.us/gamernews_images/BBJEM3WpBH/Screen Shot 2014-02-24 at 11.50.36.png)

Now you can run the game on different device types. The results should look like this:

![](https://static.makegameswith.us/gamernews_images/Ss3YVGnsN0/ResultsFlexibleMode%20(2).png)

No scaling anymore - the bigger the device, the bigger the visible area of the game! **This solution can be useful for tiled games, strategy games and also for side scrollers.**<span style=""></span>

# Conclusion

If you completed this small tutorial series you have learned a lot about the new features in SpriteBuilder and Cocos2D that allow you to build games for a lot of different devices. Let's recapture:

*   *Flexible* screen mode should be used when you want to scale your game up and can provide a certain flexibility for the position of the gameplay within the background. The *flexible* screen mode is also used for the third approach where you display a larger portion of the gameplay on larger devices
*   *Fixed* screen mode can be used when you want to scale your game and avoid relative layouts. It has some drawbacks when you try to design UIs.
*   If you use *Points* as positioning/sizing type, sizes and positions will be scaled up on larger devices
*   If you use *UI Points* all positions and sizes are defined absolute, independent of the device type
*   When you select a resource in the left panel you can decide which resolution of the image shall be used on which device type (you can decide if a tablet uses a larger image or if it uses the same image as the phone version)
*   If you plan to create an upscaled version of your game for tablets all your assets should be provided in 4x resolution (otherwise SpriteBuilder will scale up your 2x images which will result in blurry images).
*   Cocos2D lets you change the *reference corner*, the *anchor point,* the *sizing type* and the *positioning type* of nodes in your scenes. All these options, discussed in detail in part 1 of this tutorial, give you fine grained control over the positioning and sizing of nodes and let you create designs that dynamically adapt to different screen sizes

Apply this knowledge to your own games as early as possible and compatibility for all sorts of devices will come for free!

Happy coding.

benji@makegameswith.us

* * *

Thanks a lot to [Viktor](https://twitter.com/viktorlidholt), the developer of SpriteBuilder, for providing valuable support during the creation of this tutorial series.
