---
layout: post
status: publish
published: true
title: Ruby driver for Crate database
author: Christoph Klocker
date: 2014-04-21
categories:
- tech
tags:
- ruby
- crate
excerpt_short: Ruby driver for Crate
---

Last week I was invited by the guys from [Crate](http://www.crate.io) to take part at their Mountain Hackathon. We went
for 3 days to Schwarzenberg, in the lovely Bregenzerwald and spend time hacking on Crate related topics.

## First day

My part was to develop a Ruby gem to connect to Crate using their REST API. After half a day I had the basic driver ready,
however I decided to remove the dependency from the rest-client and refactored it into a zero dependency gem, think that
is the way to go.

Developing was pretty tricky at the start. I coded my tests, implemented the code and all was good. Continued with other tasks,
ran the tests again and the tests failed. Ok, I broke something. Trying to figure out whats broken... Like always, when
such things happen, you don't commit your code, so I couldn't roll back. So debugging, for ages, then finally it worked again.
Then, next you continue to code again, and then, it happens again. Damn! ... Again you can't figure out whats going on.
If you would code on Windows, you probably would just reboot. I did reboot, however not my PC, but Crate, and finally all tests
were green again. WTF?.

So, after talking to the Crate guys, I was told that Crate automatically checks the network and finds other Crate nodes
and starts replicating and sharding right away, and as we were 20 people and nearly everyone was booting and turning off
their Crate instances again, that what was causing the problems. After reconfiguring Crate to only search localhost all
problems were gone.

Thinking about what was happening, I realized what was happening was really cool. Starting up another database node and
it gets found automatically and the data is replicated and sharded instantly is pretty awesome. Everyone who has already
dealt with database sharding before knows what implicates in sharding.

So if you want to start playing around with Ruby and Crate get Crate [here](https://crate.io/download/) and the Ruby
gem using

     gem install crate_ruby

## Second day

On the second day I started working on the Crate adapter for ActiveRecord (activerecord-crate-adapter) to use it inside Rails. After some investigation
and studying the SQLServer adapter I started coding. I quickly got something going and at the end of the day I was happy
to show off the first running Rails app using Crate as database and initializing and storing a record.

I'm currently still refactoring the code, but at the time you are reading this, it will probably
be available at [crate/activerecord-crate-adapter](https://github.com/crate/activerecord-crate-adapter)


## Third day

On friday, after a long night and a late breakfast I continued on the ActiveRecord adapter before I headed back to Dornbirn.


I really enjoyed my first hackathon. For myself it was really something different, being on a hut and just focus on coding on
a joint effort (move Crate further) with people I hardly knew. With all the great food from Christian, Jodok and Bruno's famous Käsknöpfle,
some beers and great people I had an awesome time.

Now I can't wait to see my driver in action ;)


## Some impressions

<img src="/images/hackathon/IMG_20140409_085147_1.jpg" class="img-responsive img-thumbnail">
<img src="/images/hackathon/IMG_20140409_144602_1.jpg" class="img-responsive img-thumbnail">
<img src="/images/hackathon/IMG_20140410_205404_1.jpg" class="img-responsive img-thumbnail">
<img src="/images/hackathon/IMG_20140410_210400_1.jpg" class="img-responsive img-thumbnail">
