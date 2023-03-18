# Java Setup

In the context of Appium and Selenium automation, we think about how we would get a working setup on our local machine. The first thing we want to make sure is present is something called Java. Java is a programming language and set of tools that are used by both Appium and Selenium.
First of all, you can check to see if I already have Java on your machine. It often comes on computers by default nowadays, or you may have already installed it for another purpose. To check if you have it installed, open up either a terminal on macOS, or a command prompt on Windows, and run the appropriate 'echo' command here:

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

But It's often really important to have environment variables set for one reason or another, and it's a pain to set them by hand every time I open a terminal window. So let's talk about how to make it seem like I can persist these variables across sessions. On Mac, it looks like adding the export command I just saw to something called a shell startup script. By the way, when I say "shell", that's just another way of talking about the program I'm using when I open up the terminal. 

Whenever I open up a shell, the shell process tries to read certain files, called startup scripts, to see if I have put any commands in those files for it to run automatically. I can include environment variable exports in these shell startup scripts. The tricky part is figuring out which file I need to create or edit in order to have it work.

On macOS, the file we can create or edit will depend on our operating system. If you have a version older than Catalina, then the file will be called <code>.bashrc</code>. That <code>.</code> in front makes it a hidden or invisible file, and it needs to be located in my home directory. It's called <code>bashrc</code> because it's a startup script for the Bash shell, which is the default in older versions of macOS. To figure out if this file exists, I can run the following command: <code>ls ~/.bashrc</code>. On Mac and Linux, the <code>~</code> character is a shortcut to my home directory, so this is basically asking the system to list the file called <code>.bashrc</code> in the home directory. 

If I'm on Catalina or higher, the default shell for macOS is something called Zsh or Z shell, and its startup script file is <code>~/.zshrc</code> instead. So basically, look for a <code>~/.bashrc</code> or a <code>~/.zshrc</code> file as appropriate for your version of macOS. (In all the instructions that follow, I'll just mention ~/.bashrc, so substitute ~/.zshrc for my case if necessary.)

        % ls ~/.zshrc
        /Users/lanabegunova/.zshrc

If you get a filename listed as the output of that <code>ls</code> command, great! Otherwise, if the <code>.bashrc</code> file does not exist, no problem - you'll just need to create one. So let's go ahead and open up a text editor and I can load that file, or create it if it doesn't already exist in my home directory. So I'll get the open dialog up. There's a problem here, however, which is that the dialog doesn't show hidden files by default. To get them to show up, we need to use a special Mac keyboard shortcut: <code>CMD+SHIFT+period</code>. Now I can see the hidden files, and I'll open up <code>.bashrc</code>. If there's other text in there already, just ignore it for now, it's fine. What I want to do is to create a new line at the very bottom:

        export HELLO=world

Now let's save the file, and close any open terminal windows. Open a new terminal, and try the <code>echo $HELLO</code> command again. Now, it should print out "world", and it should do that in the future as well. So that's how we set environment variables automatically for future terminal sessions on macOS.

On Windows, it's a bit different, and it's best to use the system UI to make this change. First, I'll open up a command prompt however, and try to echo the variable to show that it's not yet set:

        echo %HELLO%

Remember on Windows that when we refer to environment variables, we put percent signs around them. Now let's set up this custom environment variable. First, you want to open up the Start menu and start searching for 'Advanced system settings'.

Now you can open up that control panel item, and it will go straight to the 'advanced' tab of the system properties control panel. Towards the bottom, you'll see a button that says "Environment Variables...". That's what we want, so click it. In the window that pops up, you'll see two sections, one section for user variables and one section for system variables. If you only care about setting these things up for my single user, it's fine to focus on the user variables, otherwise, focus on the system variables. Let's create a new user variable by clicking "New..." underneath that section.

This opens up a dialog where I can enter a name and a value. Enter "HELLO" as the name and "world" as the value, then click OK. Now, save and exit out of all the dialogs, and open up a new command prompt. In this prompt, I can again type <code>echo %HELLO%</code> and I should see 'world' printed back to me! So that's how I set environment variables persistently on Windows.

Back to making sure we have Java set up. Remember, the commands we ran to see if it was accessible via the right environment variable are <code>echo $JAVA_HOME</code> on Mac or <code>echo %JAVA_HOME%</code> on Windows. If you got a value printed out there, examine it. Does it have the string "jdk" somewhere in it? And is the version number 1.8 or higher? If so, you're in luck! It appears Java is already set up correctly.

<img width="500" src="https://user-images.githubusercontent.com/70295997/226087741-eb99600a-c274-416e-98ac-e481422d6b1a.png">

        % echo $JAVA_HOME
        /usr/local/Cellar/openjdk@11/11.0.15/libexec/openjdk.jdk/Contents/Home

What if we got something printed out, but it doesn't say "jdk"? Maybe it says "jre". In that case we'll need to download and install a new version of Java. This is because Appium needs to have something called the JDK, or the Java Development Kit, because this version of Java has more tools bundled with it than the JRE, or the Java Runtime Engine alone. Likewise, if we got nothing printed out at all, then we'll need to install Java.
















