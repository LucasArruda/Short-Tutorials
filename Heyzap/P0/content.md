---
title: Heyzap SDK
slug: heyzap-sdk
---

Introducing Heyzap
====================
Heyzap is a simple way to monetize your app by adding advertisements. After downloading their SDK and adding the framework to your application, it is easy to show the user ads upon reaching a new level or getting a game over.

Heyzap pays you each time an ad converts to an installation. If someone playing your game installs an app from an advertisement then you will make money.

You can also show rewarded video ads -- ads that offer the user an in-game incentive to watch an advertisement. For example, you could offer a user 100 coins of in-game currency if they watch a 15 second ad.

Making an Account
====================
First things first, you'll need to [make an account with Heyzap](https://www.heyzap.com/register?next=https://developers.heyzap.com/dashboard). Sign up with an account to "Monetize apps."

![Make an Account](./MakeAnAccount.png)

You do not need to include a URL to your game.

Claiming your App
====================

You'll next need to claim your application through Heyzap. After creating an account with Heyzap, enter your App Store *Apple ID* (a unique ID number identifying your app) [here](https://developers.heyzap.com/dashboard/games/add_game).

#### Finding the Apple ID for Published Applications
If your app is already in the App Store, you can find your Apple ID with the following steps:

1. Log in to [the iTunesConnect website](https://itunesconnect.apple.com/).
2. Click on "[My Apps](https://itunesconnect.apple.com/WebObjects/iTunesConnect.woa/ra/ng/app)."
3. Click on the icon of the application you want to add.
4. The Apple ID should be listed on the page.

	![Apple ID](./AppleID.png)

Once you have found your Apple ID, [claim your app here](https://developers.heyzap.com/dashboard/games/add_game).


#### Getting the Apple ID for Unpublished Applications

You can do this even if your app has not yet been published to the App Store, by completing the steps necessary for submitting your app to the App Store without submitting the binary:

1. Log in to [the iTunesConnect website](https://itunesconnect.apple.com/).
2. Click on "[My Apps](https://itunesconnect.apple.com/WebObjects/iTunesConnect.woa/ra/ng/app)."
3. In the upper left hand corner, click on the ```+``` button to make a new app.

	![Make a New App](https://s3.amazonaws.com/mgwu-misc/Heyzap+Tutorial/AddNewApp.png)

4. Fill out the required information and hit "Create."
5. The Apple ID will be listed on the next page.

	![Apple ID](./AppleID.png)

Once you have your Apple ID, [claim your app here](https://developers.heyzap.com/dashboard/games/add_game).

Adding the Framework
====================
Download the Heyzap SDK by [clicking here](https://developers.heyzap.com/sdk/download?platform=iphone).

Unzip the download and navigate to the ```ios-sdk``` folder. In your Xcode project, drag the framework under the Frameworks folder in Xcode project navigator.

![Add Frameworks](./AddFrameworks.png "AddFrameworks")

Be sure ```Copy items if needed``` and ```Add to targets``` are checked.

![Copy Items if Needed](./CopyIfNeeded.png "CopyItems")

In the app's ```Build Settings```, use the search bar to search for ```modules```. Set ```Enable Modules (C and Objective-C)``` to ```Yes```.

![Enable Modules](./BuildSettingsModules.png "EnableModules")

Finally, navigate to the ```AppDelegate.m``` file, and add the following semantic import statements:

	@import StoreKit;
	@import QuartzCore;
	@import AdSupport;
	@import MediaPlayer;
	@import CoreTelephony;

Initializing the SDK
====================
In the ```AppDelegate.m``` file, add the following import line:

	#import <HeyzapAds/HeyzapAds.h>


In the ```application: didFinishLaunchingWithOptions:``` method (in ```AppDelegate.m```), you need to start the Heyzap SDK with ***your personal Publisher ID***.

You can find your Publisher ID under your[ Account at Heyzap](https://developers.heyzap.com/account). Add the line ```[HeyzapAds startWithPublisherID: @"YOUR PUBLISHER ID HERE"];```. This is what the method will look like -- but replace the zeros with **your Publisher ID**:

	- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
	{
    	[HeyzapAds startWithPublisherID: @"00000000000000000000000000000000"];

    	...

Build your app before going on, to check for any compiler errors.


Displaying Interstitial Ads
====================
Heyzap's advertisements are interstitial, meaning that they pause update loops and other code running in the background and appear before or after expected content. They take up the whole screen (as opposed to banner advertisements).

In Xcode, open the ```.m``` file of the scene where you will be triggering the ad (between levels or after a game over are good times). At the top, add:

	#import <HeyzapAds/HeyzapAds.h>

Now, all you have to do to trigger the ad is the line:

	[HZInterstitialAd show];

That's it! The ad is automatically fetched from their server, and gameplay will resume as normal once the user exits out of the advertisement. It will take approximately 5 minutes and 10 impressions before it moves out of "test mode" and beings to display real ads.


Displaying Video Ads
====================
The ```HZInterstitialAd``` class will show both interstitial ads and video ads, depending on which ad is selected by Heyzap's algorithm at the time of showing. This is the recommended option.

However, if you want to force a video ad, you can use the ```HZVideoAd``` class instead. Video ads are best shown only when you expect user sessions to be long. Taking up to 30 seconds away from a 2-5 minute session is a substantial amount of time to ask of users.

If you are forcing a video ad, you need to make sure there is one queued up. This should be done as early as possible in the app's lifecycle, using the command ```[HZVideoAd fetch];``` and also every time after a video ad is shown, to allow as much time as possible to download the necessary assets.

In your ```AppDelegate.m```, after the line where you started the SDK, add the ```fetch``` method call. Here is what it should look like now (with the zeros replaced, of course):

	 	- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
	{
    	[HeyzapAds startWithPublisherID: @"00000000000000000000000000000000"];
    	[HZVideoAd fetch];
    	...

You can now use the lines:

	[HZVideoAd show];
	[HZVideoAd fetch];

to show video ads in the same way as you did with the interstitial ads. The ```fetch``` is called right after ```show``` to give as much time as possible for the next ad to be queued up. Be sure you have imported the Heyzap framework at the top of your ```.m``` file:

	#import <HeyzapAds/HeyzapAds.h>

A video ad will only show if there is one ready to be played, otherwise, things will continue uninterrupted. You can also check if a video ad is lined up and ready to be shown with ```BOOL adIsReady = [HZVideoAd isAvailable];```.

One thing to note is that while the advertisements will pause your update loops and other scheduled code, they don't pause the background music or sounds being played with ```OALSimpleAudio```, so that is something you need to do manually for video advertisements. Before you call the ```show``` method, pause the background music. You can put the code to unpause the music right after you call the ```show``` method. Make sure you don't turn music on if it was not playing before the video ad though!

Best Practices for Cocos2D
=========

In general, accidental clicks will only succeed in annoying your users, not in raising conversion rates. You will only make money if the user decides they want to download the advertised app, so when a user accidentally clicks on an advertisement that they have no interest in downloading, you have navigated them away from your game for no reason.

Users are much more likely to click on an ad after a game has ended than before it started, because they have just finished a task and won't mind as much navigating away from the app.

The examples below assume you are using Cocos2D, but also parallel SpriteKit closely. ```replaceScene``` and ```presentScene``` are effectively the same thing.


#### When to show ads

###### If your restart calls ```replaceScene```
The ads will immediately display after you call ```show```. If you call ```[HZInterstitialAd show]``` immediately after a game over, the user might be confused. Ideally, you would display a "Game Over" message to let the user know that the game ended, then display the ad after a short delay to ensure the user has time to process the message.

If you reset the game after a "Game Over" by calling ```[[CCDirector sharedDirector] replaceScene:myNewScene];``` you can use the following method to delay the display of the ad by 2 seconds:

	[self scheduleBlock:^(CCTimer *timer) {
    	[HZInterstitialAd show];
    } delay:2.0];

If the user presses the **Restart** button before the ad appears, then the ```self``` is destroyed, removing all associated scheduled blocks. No ad will be shown if they hit restart before 2 seconds have passed. This is because ```replaceScene``` creates a new instance of ```self```, and ARC deallocates the old one.

The automatic removal of scheduled blocks is important, because we don't want an ad showing up in the middle of a game. However, that also means that if the user is quick, we wouldn't be showing them any ads. In this case, we need to create a boolean that checks if we have displayed an ad. If they press the restart button before an ad is displayed, we will display the ad before starting the new game. Initialize a boolean instance variable ```BOOL adShown;```. Add the following to the ```scheduleBlock```:

	adShown = NO;
	[self scheduleBlock:^(CCTimer *timer) {
    	[HZInterstitialAd show];
    	adShown = YES;
    } delay:2.0];

Then, just before you call ```replaceScene``` add this to check if you need to show an ad:

	if (!adShown) {
		[HZInterstitialAd show];
	}

######If your restart does not create a new instance of Gameplay Scene
That technique *will not work* if you restart the game without creating a new instance of the ```self```, since after the 2 second delay, the self will still have the block scheduled. If the **Restart** button were pressed before the 2 second delay had expired, an ad would pop up in the middle of the game. We need to make sure that the call to ```HZInterstitialAd``` has been unscheduled before restarting the game. Since this is not possible to do with a schedule block, we need to schedule a selector.

We still need to initialize ```BOOL adShown;```. This time, we first create a new method called ```showAd```.

	-(void) showAd {
		[HZInterstitialAd];
		adShown = YES;
	}

Call that method in place of the schedule block that we used above using:

	adShown = NO;
	[self schedule:@selector(showAd) interval:2.0];

Now, add the following before where you restart the game:

	if (!adShown) {
		[self unschedule:@selector(showAd)];
		[HZInterstitialAd show];
	}

####How often to show ads

When to show the ad is best determined by analyzing analytics. If your game is a fast paced arcade game, with short average sessions, feel free to show ads around once every minute or two. If you have longer, puzzle-based, strategy games, you have more time to monetize a user, so show ads less often. You should also give them more time to play the game before showing the ad, so with long sessions, show the ad late in the experience.

You want to minimize the disruption to the user experience, so the user continues to visit your app and does not get too annoyed.

To keep track of how often you have shown ads, try [using a Singleton](http://en.wikipedia.org/wiki/Singleton_pattern).



Displaying Rewarded Video Ads
====================

Heyzap offers two types of video advertisements. There are ads that the user can hit a button to skip (which we just covered with the ```HZVideoAd``` class), as well as 10-15 second unskippable ones for "rewarded video ads," for which we use the ```HZIncentivizedAd``` class.

These ads intrude more than usual on the user experience, so they are reserved for situations in which the user has an incentive to watch the video. You could offer the user a free hint or in-game currency if they agree to watch the ad.

The theory behind this is that users are more likely to be interested in downloading an app if they are exposed to it more, so you're more likely to convert an ad view to a download if they have to watch a full advertisement.

Just as with the ```HZVideoAd``` class, you need to fetch the ```HZIncentivizedAd``` as early as possible. Add ```[HZIncentivizedAd fetch];``` after where you started the SDK in your ```AppDelegate.m```.

Before you pull up the scene offering the incentive, you should check to see if there is a video queued up and ready to display:

	if ([HZIncentivizedAd isAvailable]) {
    	[self offerIncentiveToWatchAd];
	}

Once you are ready to show the ad, you can show and fetch it with the same method as ```HZVideoAd```:

	-(void) offerIncentiveToWatchAd {
		if (userAgreedToWatchAd) {
			[self giveUser100Coins];
			[HZIncentivizedAd show];
			[HZIncentivizedAd fetch];
		}
	}

If no ad has been fetched successfully, no ad will show. That is why we check to make sure that something is available before displaying the offer (since it would not make sense to offer an incentive when there is no ad to show).

Once you have seen a functioning Heyzap ad in your app, you are ready to submit to the App Store!
