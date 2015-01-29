---
title: How to create an iPad version of your iPhone game
slug: create-an-ipad-version-of-your-iphone-game
gamernews_id: 291
---            

As of today Apple has sold approximately 170 million iPads (Oct. 2013). While this number is lower then the over 250 million iPhones (Jun. 2013) Apple has been able to place in customers hands, it is out of question that developers building apps solely for the mobile phone are neglecting a huge amount of potential customers. Apple incentivizes iOS applications built for the iPad by ranking them higher on the iPad App Store search results which can be very important for games with a small marketing budget.

**The good news about all of this is that building an iPad version for your game does not necessarily mean investing a huge amount of effort and time.** We will skim over different approaches and outline what you potentially need to add or change for an iPad version of your game.

*   **Approach 1: Blow your game up to iPad dimensions.**

    This is the approach with the least time effort, which can still create great results. When publishing with MakeGamesWithUs, we will provide you **a new set of assets**, optimized for the iPad screen size. These images will have a *-ipad* suffix and will be automatically picked by cocos2d, when your game runs on an iPad. If you plan to use this approach, get in contact with us and we will help you specifying the necessary dimensions.

    The only thing you have to take care of, is to **use relative positioning in your game**.

    The developer of [Blend](https://itunes.apple.com/us/app/blend-fruity-insanity!/id725766849?mt=8) used this approach to build an awesome version of his iPad game.

    ![](https://static.makegameswith.us/gamernews_images/txQm0gzDYt/iOS Simulator Screen shot 25 Oct 2013 17.55.43.png)

*   **Approach 2: Increase visible area of your Gameplay.**

    If you have a game like an infinite runner, a platformer or any other game that lets you scroll through a game world that is bigger than just one iPhone screen, you have the opportunity to build an iPad version that shows a bigger part of this game world. Additionally you should blow up the size of buttons and menu entries by using separate iPad assets. This approach was taken by the developer of [](https://itunes.apple.com/us/app/cheese-miners-lunar-supremacy/id570118272?mt=8)[the Deeps.](https://itunes.apple.com/ca/app/deeps-subterranean-carnage/id532041586?mt=8)

    ![](https://static.makegameswith.us/gamernews_images/kBmfkEZNUv/iOS Simulator Screen shot 25 Oct 2013 17.53.54.png)

*   **Approach 3: Alternate gameplay/content.**

    This is the most complicated approach and we don't have developers that have used it yet. However, for some games the different dimensions of an iPad device can leverage the capabilities of a game, creating a new unique experience. For example for a puzzle games this approach can mean increasing the size of the puzzle grid and potentially creating a whole new set of rules. Because of the big effort of this approach, most games will want to use approach 1 or 2.

##Basics when implementing an iPad version of your game

*   Set your game up to be a universal app. Therefore open your project settings and go to the "General" tab. There you select "Universal" as device family:

    ![](https://static.makegameswith.us/gamernews_images/JF9DZF5Igs/Screen Shot 2013-10-25 at 18.00.14.png)

*   If you want to run different code, if someone is using an iPad (such as loading a different level) you can use this code snippet, to detect iPads:

        if ( [[UIDevice currentDevice] userInterfaceIdiom] == UIUserInterfaceIdiomPad) {
    // run iPad specific code here
    }

*   Once you decide that your game should be a universal game, you should use relative positioning and relative sizing as much as you can. The basic idea is to express sizes of objects in relation to sizes of parent layers/scenes instead of using fixed numbers. Some examples:

*   A Button that shall be as wide as the layer and have a left and a right margin of 5 pixels:

        myButton.size = CGSizeMake(self.contentSize.width -10, 40);

*   You want to display a sprite on the top right screen of the corner:

        mySprite.position = ccp(self.contentSize.width - mySprite.contentSize.width, 		self.contentSize.height - mySprite.contentSize.height);
        
*   Using iPad specific images: once you decide to build an iPad version of your game, we will provide *-ipad.png* and *-ipad-hd.png* images for you. You can drop these images into your project and cocos2D will use these assets when running on an iPad.

Now you can start building a great iPad version of your game! If you need any additional help, please drop me a line at: [benji@makegameswith.us](mailto:benji@makegameswith.us)
