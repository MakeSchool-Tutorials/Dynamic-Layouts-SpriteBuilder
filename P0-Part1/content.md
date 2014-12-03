---
title: Dynamic Layouts with SpriteBuilder and Cocos2D 3.x - Part 1
slug: part-1
---              

Cocos2D 3 comes with a completely new positioning system that provides many different position types. This new system helps us to define layouts relative to the screen size and support multiple device sizes out of the box.

This tutorial will walk you through the different options and give you examples of how to use the new positioning types in SpriteBuilder. This is the structure of this tutorial:

**Part 1:**

*   General introduction to new positioning types
*   Screen Modes
*   Sprites on different device sizes
*   Simple examples of different layout challenges

**Part 2:**

*   Different ways to make your game universal (working on phones and tablets)

# Layout options in Cocos2D

I'll first give you brief overview of the different options, then we will use them in practical examples. Here are the different parameters that give you fine grained control over your layouts:

*   **Anchor point:** defines a point within a Node that shall be used as reference point for positioning
*   **Reference corner:** defines the corner in the parent Node that shall be used as reference point (default is bottom-left), meaning that a Node with position (0,0) will be placed in that corner.
*   **Position type:** can be individually chosen for vertical positioning and horizontal positioning. There are three options each:

        *   **Points**: this is the default. Points are scaled by a *UIScaleFactor.* In case you built a tablet version of your game the points will be scaled by this factor. The default *UIScaleFactor* for tablet is *2.0*.That means positions are doubled on a tablet.
    *   **UI Points**: positions using UI Points are never scaled. This means a 10 pt distance will appear 10pt wide on a phone and on a tablet*.* You use this option if you want to build a tablet version of your game that has an increased visible area.
    *   **Normalized**: position is expressed relatively to the size of the parent node. You can choose between 1.0 and 0.0. This is very useful to center Nodes.
*   **Size type:** can be individually chosen for width and height. There is a total of 5 different options:

        *   **Points:** again, this is the default value. The size is scaled by the *UIScaleFactor*.*   **UI Points:** sizes are provided in absolute points. Sizes stay the same on all device types.
    *   **Normalized:** size is expressed relatively to the size of the parent node. You can choose between 1.0 and 0.0.
    *   **Inset Points:** this Node will be the size of the parent node MINUS the inset points you defined. The inset points are scaled depending on the device type.You will get to see a practical example of this within this tutorial.
    *   **Inset UI Points:** the same as *Inset Points* except that the inset is not scaled.

This indeed is a huge list of options! All the concepts will be a lot more comprehensible if we use them in some example layouts. Next we will take a brief look at Screen Modes, another layout feature provided by SpriteBuilder.

# Screen Modes

SpriteBuilder has two different *Screen Modes*:

*   Flexible
*   Fixed

The Screen Mode influences how your Scenes change when running on different devices. When you choose *Flexible* the scenes will resize to fill the screen of the current device type. Therefore when you express positions or sizes of Nodes *in % of parent container* they will change on different device types, depending on the screen size of the device.

When you choose the *Fixed* Screen Mode your scenes will always have the same size. Therefore positions and sizes of Nodes expressed in *% of parent container* will never change.

Here's a short illustration to make the concepts comprehensible, the size of the yellow block is expressed as 20% of parents width and height:

![](https://static.makegameswith.us/gamernews_images/4O4sE7VW44/ScreenModes.png)

As you can see on the left side, using the *flexible* mode, the width of the yellow block changes on different device types. On a wider screen the block appears wider, too.

On the right side, using *fixed* mode the size of the block will stay the same on any screen size.

## Representation in SpriteBuilder

Along with these two screen sizes come two different representations in SpriteBuilder. **Flexible** will allow you to switch between different device types (3.5 inch phone, 4 inch phone, tablet) to get a preview of how sizes and positions change. **Fixed** will give you only one representation, highlighting *safe* and *unsafe* areas of the screen:

![](https://static.makegameswith.us/gamernews_images/TufaBarXXD/safeunsafe.png)

## Safe and unsafe areas

The *safe area* will be visible on any device type (phones &amp; tablets). The *unsafe area* will only be visible on certain device types. On an iPhone 5 for example, the left and right edges of the unsafe area will be visible because the phone is long. On an iPad the top and bottom edges of the unsafe area will be visible because comparing the screen ratio it is higher than phones.

We will discuss how to use the fixed screen mode later on, however the basic concept is to keep all the relevant content in the safe area and add padding or larger background images to cover the unsafe area.

# Sprites and different Device Types

You now have learned about different options to scale up positions and sizes of Nodes for different device types. If you want control over wether Sprites are scaled up or not you need to understand the asset handling in SpriteBuilder in detail.

When using SpriteBuilder you by default provide 4x images to use in your game. From these 4x images SpriteBuilder derives all the smaller image sizes. Let's assume you have a character that has a size of 50x50 points on a iPhone screen, then the 4x image will have a resolution of 200x200 points.

The table below demonstrates the default image sizes used for the different device types in SpriteBuilder:

<table>
<tbody>
<tr>
	<th>
		                      Device
	</th>
	<th>
		                       Default Image Type
	</th>
	<th>
		          Size on screen (points)
	</th>
	<th>
		         Resolution
	</th>
</tr>
<tr>
	<td>
		                      iPhone
	</td>
	<td>
		                      1x
	</td>
	<td>
		                      50 x 50
	</td>
	<td>
		                      50 x 50
	</td>
</tr>
<tr>
	<td>
		                      iPhone Retina
	</td>
	<td>
		                      2x
	</td>
	<td>
		                      50 x 50
	</td>
	<td>
		                      100 x 100
	</td>
</tr>
<tr>
	<td>
		                      iPad
	</td>
	<td>
		                      2x
	</td>
	<td>
		                      100 x 100
	</td>
	<td>
		                      100 x 100
	</td>
</tr>
<tr>
	<td>
		                      iPad retina
	</td>
	<td>
		                      4x
	</td>
	<td>
		                      100 x 100
	</td>
	<td>
		                      200 x 200
	</td>
</tr>
</tbody>
</table>

This means that by default Sprites on the iPad appear double size than on the iPhone. The underlying assumption is that a tablet has more or less the double amount of screen space of a phone - so doubling the image size will lead to a similar representation of the game on phones and tablets.

You can see/change the default settings displayed in the table above when you select an image that is part of your SpriteBuilder project:

![](https://static.makegameswith.us/gamernews_images/Ag6GXUTFMT/Screen Shot 2014-02-19 at 15.39.15.png)

If you ever don't want your tablet version to scale images up, select *2x* for the *tablethd* instead of *4x*. Then all assets will have the same size on the screens of phones and tablets*.* We will be discussing this approach in the second part of the tutorial when we look at different ways to use/avoid upscaling on tablets.

# Layout Examples

We are going to get started with our first example in SpriteBuilder. Open SpriteBuilder and create a new project.

The most important goal we are trying to achieve when using the different layout and sizing options is to create a responsive user interface. When you create a game you should try to target as many device types as possible (3.5 inch iPhones, 4 inch iPhones, iPads, and thanks to Apportable a huge amount of Android devices). All of these devices come with different screen sizes. SpriteBuilder and the new positioning options can help you to define UIs that look good on all kind of devices. In SpriteBuilder's top bar you can open Document -&gt; Resolution and choose between different device types.

![](https://static.makegameswith.us/gamernews_images/xOjgNKfeMa/Screen Shot 2014-02-04 at 10.04.02.png)

These options will give you a nice preview of how your interface will change depending on the screen size of the device it is running on.

## Positioning a Node in the top right corner

Now lets take a look at our first simple layout challenge. Assume we want a Node that:

*   Is positioned in the top right corner with a horizontal and vertical margin of 10 points*   Has a width and height of 50 points
*   Size and margins shall be scaled up on tablets to use a similar portion of the screen

If you worked with this feature of SpriteBuilder before you might know that we need to modify the anchor point and the reference corner to achieve this position. Add a *Color Node* to your *MainScene.ccb* and apply the necessary positioning and sizing options:

![](https://static.makegameswith.us/gamernews_images/cRAR8yBGYO/Screen Shot 2014-02-17 at 13.30.38.png)

You need to change three options to make the node stick in the top right corner:

*   Set the *reference corner* to the top right
*   Set the *anchor point* to (1.0, 1.0), which is the top right corner of the Node
*   Set the *position* to (10, 10)

Now you can switch between the different resolution types and see that the Node is always placed in the top right corner.

## Centering a Node

A common and fairly simple layout challenge. We want a Node that:

*   has a size of (100,100) which is not scaled on tablets
*   is centered on the screen

Add a second *Color Node* to *MainScene.ccb*:

![](https://static.makegameswith.us/gamernews_images/45bXX7FccM/Screen Shot 2014-02-17 at 14.20.47.png)

You need to chose the following options to place and size the Node as described:

*   The positioning type needs to be *in percent of parent container,* the values for x and y need to be *50*
*   The content size needs to be defined in *UI Points* which means that the size will not be scaled up on tablets
*   The *anchor point* needs to be at the center of the node (0.5, 0.5)

## Use Nodes that automatically resize

This layout is a little more tricky: You have two Nodes that shall use the entire screen size, one has a fixed size and the other one shall use the rest of the available space. Luckily SpriteBuilder reduces the complexity of this problem.

Remove the two Nodes you created for the earlier examples. Now create a new *Color Node* and add it to the left part of the screen:

![](https://static.makegameswith.us/gamernews_images/PBJ0Q94gT7/Screen Shot 2014-02-17 at 14.36.35.png)

Set the *position* to (0,0). Set the height to be 100% *of parent container,* and set the the *width* to 150.

	                                       Now add the second Node that shall resize to take the rest of the available screen size:

![](https://static.makegameswith.us/gamernews_images/RdciTbe2lH/Screen Shot 2014-02-17 at 14.45.01.png)

You need to set the Node up as following:

*   Set the *position* to (150, 0) so it will align with the other Node on the left side
*   Set the *width* to be 150 and set the *size type* to *Width Inset in points.* This setting defines that the width of this node should be the width of the parent node *minus* 150. This setting will reserve the 150 points for the Node on the left and otherwise resize the right Node with the screen size
*   Set the *height* to be 100% *of parent container*, too, just as for the first Node

Now  you can again switch between different device types and you will see the Nodes resizing correctly.

# Loading different Interface Files for different device types

In very rare cases all of the layout options aren't enough.

If you want to provide a universal game, but for a part of your game need individual CCB files for phones and tablets you can accomplish this manually by loading different CCB files in code.

You could be creating a *Gameplay-phone.ccb* and a *Gameplay-tablet.ccb* and check the device type in code before loading either of the two. However, for most types of games we do not recommend this approach - it comes with a large maintenance overhead. That's why we will not discuss this option in detail and only mention it to complete the list of different options.

# Conclusion

Well done! While this is only the tip of the layout iceberg part 1 of this tutorial should give you a good idea about the different options you have to solve layout challenges and achieve compatibility for a lot of different device types. Spend some time using all of them and experimenting how they work together.

[Now you can move on to part 2 and learn how to build a table compatible game with SpriteBuilder!](https://www.makegameswith.us/gamernews/370/dynamic-layouts-with-spritebuilder-and-cocos2d-30)

            <div>

            </div>
