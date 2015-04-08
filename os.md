OPERATING SYSTEMS
===

Tracking issues related to different operating systems

* [Mac OS X](#inon)
    *  [Problems related to Mac OS X](#macpro) 
    * [General Installations](#ge)
* [Virtual Machines](#vm)
* Linux

---

<a name="inon"/>
# Mac OS X

<a name="macpro"/>
## Mayor problems with Mac OS X 10.9 (Mavericks)

10/14/2014: The File System Check fails and the operating system must be reinstalled in a brand new laptop (6 months old). The recommendation at the Apple Store is to uncheck automatic updates for the operating system (System Preferences > Apple Store > uncheck automatic updates).

10/16/2014: The computer is not working properly. It has two `lost+found` folders, two `opt` folders, the fan is in full blast mode most of the time (which never ever happened before the crash), it takes too long to delete folders, too long to install anything. I reported all these problems. It is gonna required re-installing everything one more time.

These are the steps to follow after a recovery from Time Machine.

   * As recommended in the Apple Store, before even creating any user, the migration from the Time Machine disk is the first thing to do. If the user is first created, the problem is that when running the "migration assistant", it detects that the user name already exists and duplicates the home folder, i.e., if `someuser` already exists, time machine creates `someuser 1`.
   * Google Drive does not recognize the old folder and ask you to resync it to a new location.
   * Github: refresh all the projects
   * Applications:
      __Re installations__:
        * XCode command line tools again. 
        * Update BREW: brew update; brew doctor; brew prune
        * RUBY: 
		
		rm -rf .rbenv/
    	git clone https://github.com/sstephenson/rbenv.git ~/.rbenv
    	git clone https://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build (to add the option install to rbenv)
    	Test it by: rbenv install 1.9.3-p429 
    	Set as local: rbenv local 1.9.3-p429

   * `gem install rails` or `gem install rails --no-ri --no-rdoc` for each version It might generate a problem with permissions (depending on possible problems with the `libyaml` library)
   * OS X bash Update (manually again)
   * NODE:

		rm -rf .node-gyp
		rm -rf .npm
		brew remove --force node
		brew install node
		npm install -g yo grunt-cli bower
		npm install -g bower-config
		npm install -g jshint (and Sublime-JSHint Gutter)

   * PERL: problem with the oracle driver solved. I just had to restore the ~/opt folder from Time Machine.

   * Virtual Box: solving problem with "Kernel driver not installed (rc=-1908)". I solved it by installing an updated version of virtual box.


<a name="macpro">
## Installations

<a name="ge"/>
### General

- AMPAgent. Northwestern spy
- iTerm 2
- CoRD: a Mac OS X remote desktop client for Microsoft Windows
- Cisco AnyConnect Secure Mobility Client (VPN)
- Eclipse Standard for Mac.
- WriteRoom
- Sublime Text 2
	- Mod: ln -s /Applications/editors/Sublime\ Text\ 2.app/Contents/SharedSupport/bin/subl /usr/local/bin/sublime
- Textmate
- Mou, The missing Markdown editor for web developers
- Chrome extensions: Gliffy diagrams
- GrandPerspective
- Magican
- XQuartz (for X11 X.Org X Window System that runs on OS X)
- Coot
- MacTeX
- Pandoc
- MAMP
- ncbi-blast-2.2.29+.dmg 255 MB	1/6/14 12:00:00 AM
- Circos on Ubuntu: I basically followed the steps [described here](http://kylase.github.io/CircosAPI/circos-on-linux-virtual-machine/), but the only difference is the step of installing the damn libgd library, which requires this command instead: `sudo apt-get -y install libgd2-xpm-dev build-essential`... so now it works for ubuntu desktop.

---

<a name="docker">
# Docker
- [Kitematic](https://kitematic.com/): the app to use Docker on the Mac 

<a name="vm"/>
# Virtual Machines
List of virtual machines available in my box

## Virtual Box
* Ubuntu Desktop
* Ubuntu serve
* Windows 7
* Widhows 8.1 

## Vagrant


<a name="linux">
# Linux
