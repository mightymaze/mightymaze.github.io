---
layout: post
title: Using Virtual Environments in Python
date: '2014-09-11 11:18:50'
tags:
- python
---

If we work with different Python projects professionally then, there are several times when we need to work with multiple version of modules, libraries and even Python itself. Node.js has an advantage on this section as node modules can be installed locally and globally. But Python modules are installed globally.

We can solve this problem using a virtual environment for every project locally and maintaining the dependencies there. **virtualenv** is a tool that creates isolated Python environments. That means we can work with Djungo 1.3 for a project and 1.0 for another one without any conflict.

There are couple of ways we can install virtualenv. Let's see some of them.

If we are using "PIP" then we can install by:

<pre class="brush: shell;">$ pip install virtualenv</pre>

If we are using "Easy Install" then we can install like:

<pre class="brush: shell;">$ sudo pip install virtualenv</pre>

If we are using Ubuntu then we can use the native apt-get to install it like:

<pre class="brush: shell;">$ sudo apt-get install python-virtualenv</pre>

Now that we've successfully installed the virtual environment for Python we can start using it. A new tool is installed which can be accessed from the shell. The tool is called "virtualenv".

To create a virtual environment we can simply run:

<pre class="brush: shell;">
$ virtualenv venv
New python executable in venv/bin/python
Installing distribute............done.
</pre>

We can use a Python interpreter of our choice while making a virtual environment.

<pre class="brush: shell;">$ virtualenv -p /usr/bin/python2.7 venv</pre>

To use the newly created virtual environment, it needs to be activated

<pre class="brush: shell;">$ source venv/bin/activate</pre>

or

<pre class="brush: shell;">$ . venv/bin/activate</pre>

Now we can do whatever we want and all the actions will be done in the virtual environment we have created (not the system default Python).

Whenever we need to disable the virtual environment and come back to the system default Python environment we can run:

<pre class="brush: shell;">$ deactivate</pre>

That's it. We can have as many virtual environments as we want, thus we can maintain different projects with different versions of dependencies.