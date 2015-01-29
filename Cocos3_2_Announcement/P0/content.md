---
title: Cocos2D 3.2 with CCEffects is coming
slug: p1
gamernews_id: 402
---                

Cocos2D keeps improving and iterating at rapid speed. The Cocos2D 3.2 beta is now available ([download](https://s3.amazonaws.com/spritebuilder/cocos2d-swift-3.2.0-beta.1.zip)) and it comes with a huge new feature called **CCEffects**. CCEffects make it simple to add stunning visual details, such as reflections or blur to your game.

<span style="">Today we will give you brief preview of the new CCEffects feature. We will take a look at three different effects and will show you how easy they can be implemented. We will start with the Pixelate effect.</span>

# Pixelation

Using Pixelation is very simple. Here is the original image that we used for this demo:

![](https://s3.amazonaws.com/mgwu-misc/CCEffectsDemo/spaceship.png)

And here is how it looks in our demo project after applying a *CCEffectPixelate*:

![](https://s3.amazonaws.com/mgwu-misc/CCEffectsDemo/spaceship_pixel)

## How does it work?

Here's the code for the example above, and it is amazingly simple:

        _spaceship.effect = [CCEffectPixellate effectWithBlockSize: 5];

That's it. With *CCEffectPixellate* you can now create retro effects for your game in one line of code.

# Glass

The Glass effect is definitely one of the most amazing effects that Cocos2D 3.2 provides. The glass effect reflects and refracts the environment which adds real 3D feel to your 2D game. Here is what the glass effect in our demo looks like:

![](https://s3.amazonaws.com/mgwu-misc/CCEffectsDemo/glassEffect3.gif)

You can clearly see how the refraction and the reflection of the background adds a 3D feel to this demo.

## How does it work?

Creating a glass effect is definitely more complicated then adding pixellation to your game but it still can be done in a few lines of code:

        CCSpriteFrame *normalMap = [CCSpriteFrame frameWithImageNamed:@"crystals/normalmaps/crystal-1-normal.png"];
        CCEffectGlass *glassEffect = [CCEffectGlass effectWithRefraction:0.1f refractionEnvironment:_background reflectionEnvironment:_background normalMap:normalMap];
        _crystal.effect = glassEffect;

In this demo *_crystal* and *_background* are code connections for two *CCSprites* that have been created in SpriteBuilder. The most complicated part about creating a glass effect, is creating a normal map. *Thanks a lot to the SpriteBuilder team for providing normal maps and art for the crystals in this demo.* 

A normal map is an image file that uses different colors to describe the surface of an object. N<span style="">ormal maps are used to simulate 3D effects, such as shadows or refraction of light on a sprite that is actually flat (</span>[more info](http://en.wikipedia.org/wiki/Normal_mapping)<span style="">).</span><span style=""> To create the crystal from the demo above we need two images, one that is actually displayed in the game and another one that is used as a normal map, describing the surface of the crystal. Cocos2D needs to know what the surface of the crystal looks like in order to reflect and refract light correctly. Here are the two images from the demo:</span>

![](https://s3.amazonaws.com/mgwu-misc/CCEffectsDemo/NormalMaps.png)

On the left side you can see the image displayed in the game. On the right side you can see the normal map used to calculate reflection and refraction. Creating a normal map is the hardest part of using *CCEffectGlass*. After the final release of Cocos2D 3.2 we will discuss tools that help you generate them!

# CCEffectStack

*CCEffectStack* allows you to combine multiple *CCEffects* and apply them at once. For our example project we have created a *CCEffectStack* that consists of a <span style="">*CCEffectBrightness* and *CCEffectContrast*. Another great feature of *CCEffect* is the ability to change values of *CCEffects* that are already applied. That allows developers to create animations that change parameters of *CCEffects*. Here's the scene from our demo project that uses a *CCEffectStack* and animates the brightness of the *CCEffectBrightness* over time:</span>

![](https://s3.amazonaws.com/mgwu-misc/CCEffectsDemo/brightness.gif)

## How does it work?

Brightness and Contrast are simple *CCEffects*. They can be created in one line of code each. Here is the entire code for the example above:

    @implementation EffectStackScene {
        CCSprite *_spaceship;
        CCEffectBrightness *_brightnessEffect;
        NSTimeInterval _totalTime;
    }
    - (void)didLoadFromCCB {
        // Effects like changing the contrast and the brightness are one liners.
        CCEffectContrast *contrastEffect = [CCEffectContrast effectWithContrast:0.2];
        _brightnessEffect = [CCEffectBrightness effectWithBrightness:sin(0)];
        // A CCEffectStack allows you to combine multiple CCEffects and apply them to a Sprite
        CCEffectStack *effectStack = [[CCEffectStack alloc] initWithEffects:@[contrastEffect, _brightnessEffect]];
        _spaceship.effect = effectStack;
    }
    - (void)update:(CCTime)delta {
        _totalTime += delta;
        // effects can be changed over time, which allows you to animate parameters of effects. In this case we are animating the brightness of a CCEffectBrightness we created earlier
        _brightnessEffect.brightness = sin(_totalTime);
    }
    @end

In this beta version of Cocos2D *CCEffectStack* does not have a convenience initializer and needs to be created with *alloc/init* however, that will be added for the final release of Cocos2D 3.2. Besides that, creating a *CCEffectStack* is very simple. The most interesting part about this demo is how we update the brightness of the brightness effect inside of the *update* method which creates a nicely animated effect.

# More Effects

The *CCEffects* you have seen so far are really amazing and will make it easy to create great looking games with Cocos2D. However, we have only demonstrated a few *CCEffects* in this brief demo. We haven't taken a look at <span style="">color adjustments, blur or bloom effects yet. After the final release of Cocos2D 3.2 we will discuss all available effects in detail. Overall this is just the beginning and the Cocos2D developers will be adding more effects in future releases.</span>

# Demo Project

The demo project discussed throughout this post is available on [GitHub](https://github.com/MakeGamesWithUs/CCEffectsDemo). Feel free to download it and play around with all *CCEffects*.

We are looking forward to awesome looking Cocos2D games! Stay tuned for the full tutorial on *CCEffects*!

            <div>

            </div>
