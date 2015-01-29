---
title: Touch Handling in Cocos2D 3.x
slug: touch-handling-cocos2d-3
gamernews_id: 366
---            

Cocos2d 3.0 comes with a completely overhauled touch handling system. This tutorial will provide you with all relevant information about working with touches:

*   Accepting touches
*   Retrieving touch positions
*   Working with different touch lifecycle events
*   Implementing a drag &amp; drop mechanism

# Getting started

Get started by downloading the [starter project](https://s3.amazonaws.com/mgwu-misc/TouchTutorial/Touch-Handling-Cocos2d-3.0-starter-project.zip). Once you have downloaded project you should run it and see this screen:

![](https://static.makegameswith.us/gamernews_images/hBLdTxNjdg/iOS Simulator Screen shot 04 Feb 2014 13.54.23.png)

# Note for Cocos2D 3.3

If you are using Cocos2D 3.3+ (it's part of SpriteBuilder 1.3+) you will have to replace all occurrences of *UITouch* and *UITouchEvent* with *CCTouch* and *CCTouchEvent*, respectively. You can find more information as part of the [SpriteBuilder update guide](http://www.spritebuilder.com/update).

# Accepting touches

In Cocos2d 3.0 every CCNode and every subclass of CCNode can receive touches. You only need to turn on one option. Let's do that in our custom initializer. Replace the code of the init method in *MainScene.m* with this one:

    - (id)init
    {
        if (self = [super init])
        {
            // activate touches on this scene
            self.userInteractionEnabled = TRUE;
        }
        return self;
    }

Now Cocos2d will know that we want to handle touches in this scene.

# Handling touches

Cocos2d informs us about four different types of touch events:

*   When touches begin
*   When touches move
*   When touches end
*   When touches are cancelled

These different methods let you track touches as they move along the screen. For our first example we will only need be informed about the beginning of touch events.

Add the following method to *MainScene.m*:

    - (void)touchBegan:(UITouch *)touch withEvent:(UIEvent *)event
    {
        CCLOG(@"Received a touch");
    }

When user interaction is enabled for a Node, all implemented touch handling methods will be called. We have now implemented *touchBegan* which is called whenever a new touch begins*.* We use *CCLOG* to print a debug message to the console whenever a touch occurs.

Now run the app. Every time you touch the screen a "Received a touch" message should appear in the console. Now you know how to receive touches on any Node in your game - that is powerful.

# Retrieving touch locations

The most interesting part of a touch is it's location. In the next step we're going to use the touch location to add a Sprite at every position the player touched. To achieve this we'll need to change the *touchBegan* implementation. Replace the old implementation with this one:

    - (void)touchBegan:(UITouch *)touch withEvent:(UIEvent *)event
    {
        // we want to know the location of our touch in this scene
        CGPoint touchLocation = [touch locationInNode:self];
        // create a 'hero' sprite
        CCSprite *hero = [CCSprite spriteWithImageNamed:@"hero.png"];
        [self addChild:hero];
        // place the sprite at the touch location
        hero.position = touchLocation;
    }

This implementation is very straightforward. The most interesting part is retrieving the actual location of the touch. The *locationInNode* method transforms a touch position on the OpenGL view into a touch position within the Node that you provide as parameter (in this case *self*, because we want to know the location of the touch in the *MainScene*). Once we have the touch position we simply create a new *CCSprite* and add it to the *MainScene*. The asset 'hero.png' is part of the starter project you downloaded at the beginning of this tutorial.

Once you run your project you should be able to place MGWU heroes all over the screen:

![](https://static.makegameswith.us/gamernews_images/6WM18bmGe7/iOS Simulator Screen shot 04 Feb 2014 14.13.38.png)

Now that you are familiar with the very basics we will take a closer look at the touch lifecycle and how you can make use of more complex touch handling in your games.

# Making use of the touch lifecycle

Let's enhance our application. Wouldn't it be nicer if a user could touch the screen and drag a hero to a position of his choice and drop it there?

In order to make that happen we will have to use all of the touch events provided by Cocos2d 3.0:

*   **touchBegan:** gets called when the user touches the screen
*   **touchMoved:** gets called when the user moves his finger over the screen
*   **touchEnded:** gets called when the user stops touching the screen
*   **touchCancelled:** gets called when user still is touching the screen but some other issue stops your Node from handling the touch (e.g. touch moved outside the boundaries of your Node)

Our new hero placing algorithm shall work as following:

1.  when the user touches the screen a hero will be spawned
2.  as long as the user moves his finger over the screen the hero will be moved along
3.  once the user lifts his finger the hero will be dropped at the last touch position

# Implementing the new hero placement

First of all we need a variable that holds a reference to the hero we are currently moving, therefore we will add a private instance variable. Modify your *MainScene.m* file.

Replace this code:

    @implementation MainScene

With this code:

    @implementation MainScene {
        // this is the section to place private instance variables!
        CCSprite *currentHero;
    }

Now we have a new private variable. This variable shall always hold a reference to the hero we are currently dragging so that we can update his position when a touch moves across the screen. Let's begin by assigning this variable in the *touchBegan* method.</span>

Replace this code:

    // create a 'hero' sprite
    CCSprite *hero = [CCSprite spriteWithImageNamed:@"hero.png"];
    [self addChild:hero];
    // place the sprite at the touch location
    hero.position = touchLocation;

With this code:

    // create a 'hero' sprite
    currentHero = [CCSprite spriteWithImageNamed:@"hero.png"];
    [self addChild:currentHero];
    // place the sprite at the touch location
    currentHero.position = touchLocation;

Now a reference to the current hero is stored as soon as it is created. This way we have access to the last created hero in this whole object and can implement the other touch handling methods.

When the user moves his finger over the screen, we want to move the just created hero. Therefore we need to implement the *touchMoved* method which is called every time a touch changes its position. Add this method to *MainScene.m:*

    - (void)touchMoved:(UITouch *)touch withEvent:(UIEvent *)event
    {
        CGPoint touchLocation = [touch locationInNode:self];
        currentHero.position = touchLocation;
    }

What does this method do exactly? Every time a touch moves we get the location of that touch and move the last created hero to the new position.

Our last step is to reset the hero reference when a touch ends or gets cancelled, because we only want to hold a reference to the currently selected hero. Add these two methods to *MainScene.m*:

    - (void)touchEnded:(UITouch *)touch withEvent:(UIEvent *)event
    {
        currentHero = nil;
    }
    - (void)touchCancelled:(UITouch *)touch withEvent:(UIEvent *)event
    {
        currentHero = nil;
    }

Now you can build and run your project again. The behaviour should be exactly as outlined in the algorithm and should look like this:

![](https://static.makegameswith.us/gamernews_images/j4u16hHDCL/DragHero.gif)

Well done, you're another step closer to mastering touch handling in Cocos2d 3.0!

# Making the hero a touchable object

There is another feature that would be very useful. A lot of users will want to pickup already existing heroes and move them over the screen. Let's implement the following:

*   if a user touches an empty spot, a new hero will be created
*   if the user touches an existing hero, no hero will be created, instead he will drag the already existing one

To achieve this we need to create a subclass of *CCSprite*. Go ahead and create a class called *CCDragSprite* that inherits from *CCSprite*:

![](https://static.makegameswith.us/gamernews_images/f7cWyUPH8u/Screen Shot 2014-02-04 at 14.58.50.png)

Add this line to *CCDragSprite.h* to import the Cocos2d headers:

    #import "cocos2d.h"

Now we can take care of the implementation in *CCDragSprite.m*! I promise you it will be surprisingly simple. We need to do the following:

*   turn on user interaction once a *CCDragSprite* appears on screen
*   move the *CCDragSprite* around as long as the touch moves

Place the following code between *@implementation* and *@end* in *CCDragSprite.m*:

    - (void)onEnter {
        self.userInteractionEnabled = TRUE;
    }
    - (void)touchBegan:(UITouch *)touch withEvent:(UIEvent *)event
    {
    }
    - (void)touchMoved:(UITouch *)touch withEvent:(UIEvent *)event
    {
        // we want to know the location of our touch in this scene
        CGPoint touchLocation = [touch locationInNode:self.parent];
        self.position = touchLocation;
    }

By now you should be familiar with most of the code above. When our *CCDragSprite* enters the screen, we enable user interaction for it. We need to add an empty implementation of *touchBegan* so that Cocos2d knows that we want to claim this touch (otherwise the next responder will receive the touch).

In the *touchMoved* method we ask for the position of the touch in the **parent Node** (the CCScene) and move the *CCDragSprite* to that position within the parent Node.

Before we can see our drag &amp; drop mechanism in action we need to make use of this new class which we just implemented. Open *MainScene.m* and add this import statement to the top of the file:

    #import "CCDragSprite.h"

Now we are going to change the code that creates the hero to use our new class instead of *CCSprite*.

Replace this line:

    currentHero = [CCSprite spriteWithImageNamed:@"hero.png"];

With this line:

    currentHero = [CCDragSprite spriteWithImageNamed:@"hero.png"];

Now whenever the user touches the screen our new draggable subclass of *CCSprite* is initialized! Run the project to drag heroes around the screen!

![](https://static.makegameswith.us/gamernews_images/S41KjwFrrA/DragHero22.gif)

Well done! By now you should know enough about touch handling in Cocos2d 3.0 to use it in your individual game.

As always feel free to contact me with any questions or feedback.

benji@makeschool.com
