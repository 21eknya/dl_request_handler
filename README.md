# Notes.app and Files.app make a good team: Create a service for your iPhone to do something it doesn’t normally do

I love watching snowboarding videos and I love it that the Photos.app on iPhone allows you to drag the clip to view it at any speed your finger moves at. So I download a lot of them from video websites and store them in my iPhone.

There are more than one tool (mainly talking about cli tools) to download the videos. But there’s a catch, I can only do it from my laptop (then airdrop to my phone). But sometimes when I run into this awesome video I don’t necessarily have access to my laptop and when I finally get back in front of my keyboard it can take me hours to look for it or more often I just forget seeing it. I know many websites have this watch-later sort of button. Isn’t it better if I could just download the video right at the moment I see it, no matter I have my laptop around or I’m on a chairlift holding my phone.

From the express_checker project I found Notes.app can be used to implement an iOS / macOS app, which was only used to sync-up info between both platforms. It was only a nice to have feature in that project, while it’s critical to this one. My goal is to create a service that takes requests from both platforms (iOS, macOS) and download videos per the requests.

## Design:

dl_request (string of a video url) > [Notes.app](http://Notes.app) > osascript > dl_tool(a cli tool of your choice that downloads the video) > Files.app > Photo.app

dl_request: can be put down to [Notes.app](http://Notes.app) from either platform

osascript checks this particular note periodically to look for newly added links.

dl_tool: The tool used to download the videos.

The video files are stored in [Files.app](http://Files.app) so they’re available across platforms.

Finally, the videos are saved into [Photos.app](http://Photos.app) from [Files.app](http://Files.app) and mission accomplished.

## Implementation:

The service is deployed on the laptop:

### osascript:

- periodically check the note to look for newly added links.
- Download the videos in local file system
- Put down a mark so that it knows where it finishes last time it checks.
- Move the downloaded video files to an iCloud location

iCloud is providing all the cloud facilities.

A property list is created to run at start up so that the service is always available as long as the laptop is running.

At this point I’ve created a service to help me download the videos I put in the Notes.app from any iOS / macOS device. With the ability to communicate and share files between platforms, there are tons of possibilities to do some crazy things.

## IFTTT:

Immediately after I showed above design to my crew, one reminded me there’s this app to do pretty much what I described and something more. ifttt. I panicked for a split second, and downloaded it and tested it a bit. The approach it (or this applet in its term) takes was to take a request of a YouTube link and download it to google drive, not quite the same but close enough. Yes, it creates a platform for people to share their automations. It can do quite some awesome automations I’ll give it that, but unfortunately it couldn’t live up to my standard in this task (or any). 

One simple factor but a deal breaker is inherent in itself, permissions. It offers to handle those chores that requires the permissions you give. In this task, it needs the permission of google drive and youtube. If you’re here, you probably don’t like that much to give your permissions to a third party. And I can see that as one more and more relies on those applets, ifttt collects all sort of permissions and I know I’m not diligent enough to read through the EULA. On the other hand, although you’ll need to give permissions along the deployment of this service, you’re basically allow your laptop to do whatever you’re doing it by yourself. Isn’t it the exact point of this service?
