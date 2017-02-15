---
layout: post
title: Working with Cocoapods
date: '2014-05-04 03:21:15'
tags:
- ios
- dependancy
- manager
- cocoapods
- pods
- pod
---

First of all, what is Cocoapods!!

CocoaPods is the dependency manager for Objective-C projects. It has thousands of libraries and can help you scale your projects elegantly.

Almost every develper uses this to manage dependency in their projects. Couple of days ago I've started a sample chat App which was open source. From the project I learnt Cocoapods. Then I loved it as I already love verious dependency managers like Bower, Composer, NPM etc.

You can find Cocoapods <a href="http://cocoapods.org/" target="_blank">here</a>

You need Ruby Gems installed on your sysem to install Cocoapods as it's a Ruby Gems module itself. To install it just use this easy command:

<pre class="brush: shell;">
$ sudo gem install cocoapods
</pre>

If you wanna update Cocoapods then just install the gem again. It will update itself as other Ruby Gems modules.

Cocoapods uses its Podfile to know the dependencies. Podfile is a file named "Podfile" which contains the list of all dependencies.

A Podfile can be very simple like:

<pre class="brush: shell;">pod 'AFNetworking', '~> 1.0'</pre>

Or very complex like:

<pre class="brush: shell;">platform :ios, '6.0'
inhibit_all_warnings!

xcodeproj 'MyProject'

pod 'ObjectiveSugar', '~> 0.5'

target :test do
    pod 'OCMock', '~> 2.0.1'
end

post_install do |installer|
    installer.project.targets.each do |target|
        puts target.name
    end
end
</pre>

<a href="http://guides.cocoapods.org/using/the-podfile.html" target="_blank">Here</a> is the total documentation about the Podfile.

When we wanna install all dependencies then:

<pre class="brush: shell;">$ pod install</pre>

That's all, you are good to go. You can see other open source projects how they are maintaining dependencies using Cocoapods.


####How to uninstall Cocoapods

Now here comes the unexpected part. How to uninstall cocoapods after you've successfully installed it on your Mac. Here's how.

Remember, cocoapods is only a Ruby gem. So to uninstall it we first need to do is:

<pre class="brush: shell;">
$ gem list --local | grep cocoapods
</pre>

It will show something like this:

<pre>
cocoapods (0.27.1, 0.20.2)
cocoapods-core (0.27.1, 0.20.2)
cocoapods-downloader (0.2.0, 0.1.2)
</pre>

Now we just need to uninstall these 3 Ruby gems.

<pre class="brush: shell;">
$ gem uninstall cocoapods
$ gem uninstall cocoapods-core
$ gem uninstall cocoapods-downloader
</pre>

That's it. Cocoapods is uninstalled. If you have multiple version installed on your mac then you probably need to do it:

<pre class="brush: shell;">
$ gem uninstall cocoapods -v 0.20.2
</pre>

So the uninstall process is done.

I love dependency managers, and I love Cocoapods very much.