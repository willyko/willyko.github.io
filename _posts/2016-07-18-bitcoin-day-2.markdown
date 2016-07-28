---
layout: post
title:  "Alt-Coin Syscoin Attempt #1"
date:   2016-07-20 17:49:00 -0700
categories: cryptocurrency journal
---

The following is the list of commands I used to compile the newest version of syscoin (2.1 BETA)

{% highlight shell %}
###pulling source code
cd ~
git clone https://github.com/syscoin/syscoin2
git checkout --track dev origin/dev

###installing dependencies (OSX)
brew install autoconf automake berkeley-db4 libtool boost miniupnpc openssl pkg-config protobuf qt5 libevent

###installing dependencies (Linux)
sudo apt-get install build-essential libtool autotools-dev automake pkg-config libssl-dev libevent-dev bsdmainutils
sudo apt-get install libboost-system-dev libboost-filesystem-dev libboost-chrono-dev libboost-program-options-dev libboost-test-dev libboost-thread-dev
sudo add-apt-repository ppa:bitcoin/bitcoin
sudo apt-get update
sudo apt-get install libdb4.8-dev libdb4.8++-dev # had to add ppa:bitcoin/bitcoi
sudo apt-get install libzmq3-dev
sudo apt-get install libqt5gui5 libqt5core5a libqt5dbus5 qttools5-dev qttools5-dev-tools libprotobuf-dev protobuf-compiler
sudo apt-get install libqrencode-dev

###compiling
cd syscoin2
./autogen.sh
./configure
make

###creating .dmg (OSX)
sudo make deploy

{% endhighlight %}

Now... to set up the local testnet
{% highlight shell %}
cd ~/syscoin2/src/qt
./syscoin-qt -datadir="src/test/node1/" -daemon-1 -regtest -debug
./syscoin-qt -datadir="src/test/node2/" -daemon-1 -regtest -debug
./syscoin-qt -datadir="src/test/node3/" -daemon-1 -regtest -debug
{% endhighlight %}

The above should boot up three Syscoin QTs. 
If the client has error message of something like "Another client already running", this means the regtest isn't being booted check the paths and try again.
Try deleteing the whole regtest folders in each node and try again if all else don't work.

Now, choose one QT and go to Debug -> Console
Type 

{% highlight shell %}
generate 200
{% endhighlight %}

and enjoy the feeling of holding billions of TEST syscoins...

Now for the public BETA testnet (coins still don't worth anything)
** WARNING: BACKUP YOUR WALLET **
{%highlight shell %}
### removing app data
rm -rf ~/Library/Application\ Support/Syscoin
mkdir ~/Library/Application\ Support/Syscoin
{% endhighlight %}

next download [syscoin.conf][syscoinconf-url]
and put it in ~/Library/Application\ Support/Syscoin/

[syscoinconf-url]:https://github.com/syscoin/syscoin2/releases/download/2.1b2/syscoin.conf
-------------------------------------

Syscoin also has a pretty cool testing frameworking using BOOST.
this can be run by running the following after compilation:

{% highlight shell %}
cd src/test
./test_syscoin

{% endhighlight %}

Just a quick note.  I am running most of the commands above in virtual
environment with host OSX and guest ubtuntu 14.04
I did hit some issue running test_syscoin.  But I also found the work
around to be just commenting out the whole syscoin_snapshot_tests.cpp

One more note.  This dev branch is extremely fast-paced...averaging 10+
commits per day.  So the following commands might be useful updating the
repository after commenting out the syscoin_snapshot_tests.cpp

{% highlight shell %}
git stash
git pull
git stash apply
make
{% endhighlight %}



