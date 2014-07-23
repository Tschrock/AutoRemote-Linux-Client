AutoRemote-Linux-Client
=======================
About
-----
	First of all AutoRemote is a realy cool app that lets you send messages, notifications, urls, and even files between your android devices and your computer. If you haven't already please check it out here: http://joaoapps.com/autoremote.
	Usualy AutoRemote for linux allows you to send commands to your computer through ssh and get the result back. That's great, but it only supports password authentification, which means security is a bit lacking. AutoRemote-Linux-Client aims to provide a simple and effective way to send messages to your computer with greater security as well as built-in support for common things such as url sharing and file transfer. Tasker is recommended to get full usability from this.
	The main idea behind AutoRemote-Linux-Client is to be as simple as possible; I didn't want to spend 20+ hours trying to work through protocols, etc. The simplest way is to work with what's already there: an ssh client. AutoRemote-Linux-Client simply replaces the default bash shell with a messaging interface that forwards messages to a handler running in another user's session. In this way, the security of your account is no longer in the hands of an app on your phone. If your phone goes missing or you leave it unlocked somewhere, no one can gain access to your computer at home. Any security flaw that presents itself will be at the message handler, not your phone.

Setup
-----
#####Dependancies:
 - bash
 - sed
 - grep
 - xdg-utils
 - libnotify

###AutoRemote-Linux-Client
To use AutoRemote-Linux-Client there are a few things that need done first:
1. Put ```autoremote``` and ```autoremote-handler``` in a directory in your path (Perferably /usr/local/bin) and make sure they're executable.
2. Set up a user to use with autoremote and set their shell to /usr/local/bin/autoremote (or wherever you put it)
   ```sudo adduser --system --shell /usr/local/bin/autoremote --no-create-home autoremote``` should take care of this.
3. Configure your ssh server to allow it to use password authentification.
4. Go to http://joaoapps.com/autoremote/linux/ for how to set up your phone to connect to your computer.
5. Run autoremote-handler in whatever user account you want to recieve messages in (Only one user though)

You should now be able to send messages to your pc! Remote Open Url should work automatically, more advanced things will require tasker. You can also test notifications and stuff by sending them directly from within the AutoRemote app.

###Tasker
Notifier.prj.xml should get you started setting up tasker, looking into app-factory to make this nice.
Basicly you set up profiles for things like Phone Ringing and Battery Level, and use AutoRemote to send the messages to your computer.

###Supported Messages/Commands
Messages sent to your computer will be echoed by autoremote-handler. Commands are messages in the format "Command=:|:=Stuff". Commands that autoremote-handler recognises will do there corresponding task, unrecognized commands will be printed out with the messages.

At the moment, autoremote-handler acts on the following messages/commands:
1. http://www.example.com
   Any plain url's wil open in your default browser (must have the http part)
2. open-url=:|:=Some text and stuff http://www.example.com
   Any text in a open-url command will be scanned and the first url found will open in your default browser (must have the http part).
3. open-file=:|:=/path/to/a/file
   Opens the file in it's default program. Relative paths will be relative to where autoremote-handler is run.
4. show-notification=:|:=NotificationType}-&-{DeviceName}-&-{Notification Text
   The most complicated of them all; Shows a desktop notification.
   Types of notifications:
    - "notification" - a notification from your phone. 
    - "call" - an incoming call from your phone.
    - "text" - a new text message from your phone. 
    - "battery" - a battery level notification.
    - "other" - a generic message from your phone.

