# Java Setup

In the context of Appium and Selenium automation, I think about how I would get a working setup on my local machine. The first thing I want to make sure is present is something called Java. Java is a programming language and set of tools that are used by both Appium and Selenium.
First of all, I can check to see if I already have Java on my machine. It often comes on computers by default nowadays, or I may have already installed it for another purpose. To check if I have it installed, open up either a terminal on macOS, or a command prompt on Windows, and run the appropriate 'echo' command here:

- Mac: <code>echo $JAVA_HOME</code>
- Win: <code>echo %JAVA_HOME%</code>

For Mac, I would get output that looks something like:

    /Library/Java/JavaVirtualMachines/jdk1.8.0_144.jdk/Contents/Home

Or:

    /usr/local/Cellar/openjdk@11/11.0.15/libexec/openjdk.jdk/Contents/Home

Whereas for Windows, it would look like:

    C:\Program Files\java\jdk1.8.0_221

That's nice, but maybe nothing got printed out. And what am I actually doing anyway? What is the point of these commands? On each platform, these commands are designed to print out the value of something called environment variables.

Let's take a brief digression to understand environment variables since they're important, especially, for getting Appium set up. Environment variables are just like variables in any programming language, like *python*. They're little names that can hold values, and can change over time. Environment variables are designed to be variables that exist in the terminal, or across my whole system. Environment variables are made available to any program that I run from the terminal session where the variable is defined. And I probably already have lots of environment variables set by default, without realizing it.

For example, if I'm on a Mac, open up a terminal and type <code>echo $HOME</code>. I should get back something that looks like <code>/Users/lanabegunova</code>, as mine is.

Or, if you’re on Windows, open up a command prompt and type <code>echo %HOMEPATH%</code>. You should get back something that looks like <code>\Users\lanabegunova</code>, based on my computer's username.

On either platform, what's going on is that the path to your home folder has been saved as an environment variable. Here are a couple more key points about environment variables:
1. First, environment variables are only valid in the terminal session where they are defined. If you define an environment variable in one terminal window, and then open up another one, the variable will not exist in the other one. There is a way to make variables persist across sessions, which I'll discuss momentarily.
2. Second, to check the value of the environment variable, I can use the <code>echo</code> command. On mac, I put a dollar sign in front of the variable name. On Windows, I put percent signs around the variable name.
3. Third, to set environment variables from the terminal, we can use the <code>export</code> command on mac, or the <code>set</code> command on Windows. For example, I can run export <code>HELLO=world</code> on macOS, or <code>set HELLO=world</code> on Windows. In both cases, I don't use the dollar sign or percent signs when assigning values to the variables.

Let's see this in action now. I'll run the export command first:

    export HELLO=world

Which defines the variable <code>HELLO</code>. And now I can try to echo it out, by ensuring that I use the dollar sign when I reference the variable:

    echo $HELLO

And sure enough I get <code>world</code> printed out. But now let's try something. I'll open up a new terminal window, and run the same <code>echo</code> command. What happens? I get nothing printed out, which means the variable is not defined. That's because our <code>export</code> command only set the variable in that particular terminal session. Variables don't span multiple sessions.





