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

The standard way to install Java is to head to [java.com/download](java.com/download) and follow the instructions for your platform. However, this takes you through a convoluted series of pages and clicks. So instead, head directly to https://www.oracle.com/java/technologies/javase-downloads.html#javasejdk, which is a long link but will save you some time. You will need to download and run an installer for the JDK from that page. Ensure it's the JDK and not the JRE. When you run the downloaded installer, make a note of where you install Java, because we're going to need that information. So go ahead and install Java now.

Now that we've installed Java, we need to set the special JAVA_HOME environment variable. And when I say set the environment variable, I mean going through the steps appropriate for my system that I demonstrated earlier. So on Windows, we do this again by opening up the environment variables area of the advanced system settings, creating a new variable called JAVA_HOME, and as the value setting the path where we installed Java. For example, <code>C:\Program Files\java\jdk1.8.0_222</code>.

On macOS, it's a bit different. I know how to set environment variables in my <code>.bashrc</code> file now, but how do I know where Java was installed? To figure that out it's easiest to use a little helper program that comes with Java. So I can open up a terminal and run the command <code>/usr/libexec/java_home</code>. See what it prints out? For me it's:

<img width="500" src="https://user-images.githubusercontent.com/70295997/226088586-159ce970-3cbe-4ef0-b0d2-d616f881e8f9.png">

        /usr/local/Cellar/openjdk@11/11.0.15/libexec/openjdk.jdk/Contents/Home

For you it might look different. 

        /Library/Java/JavaVirtualMachines/jdk1.8.0_144.jdk/Contents/Home

Regardless, that is the path we should use as the content of the JAVA_HOME variable. Notice that on macOS, it also looks a bit different than on Windows, because it includes a *Contents* and a *Home* directory. But we'll take this string, open up our <code>.bashrc</code> file, and create the appropriate <code>export</code> statement at the end:

        export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_144.jdk/Contents/Home

With any export command, there's something we need to be aware of. If there are any spaces in it, we need to put the value in quotes. In fact, it's safe to always put things in quotes so it's not a bad idea to do that. Let's update our variable to use quotes for safety:

        export JAVA_HOME="/Library/Java/JavaVirtualMachines/jdk1.8.0_144.jdk/Contents/Home

Now I have JAVA_HOME set on macOS as well.

Next, we're going to work with yet another environment variable. This one is called PATH. The purpose of the PATH environment variable is to define a list of directories. These directories are the ones our shell will look in when we want to execute a command. Why do we need to do this? Just because I have a program somewhere on my computer doesn't mean I can just type its name to run it from a shell.

Why would we *want* to run a program by its name rather than by a full path? Because some programs are so useful we end up using them in lots of places, and it would be tedious to have to type out the full path all the time.

The Java JDK comes with a lot of little programs, some of which Appium uses in order to do its work. It makes life easier to have all these little programs on my PATH. So the first step is figuring out where these little programs live. First, let's check out what's inside my Java directory, by running a command that allows us to list the contents of a directory. On macOS, this is the <code>ls</code> command, which we can use in conjunction with our newly set environment variable:

<img width="500" src="https://user-images.githubusercontent.com/70295997/226089135-5a2152c4-2e21-43de-8d68-3a7c703b23a8.png">

        ls $JAVA_HOME
        bin	conf	demo	include	jmods	legal	lib	man	release

Now there's everything that's inside of JAVA_HOME.

Or on Windows, we use the <code>dir</code> command to do the same thing:

        dir "%JAVA_HOME%"

In each case, we'll get out a directory listing. Notice a directory inside of my JAVA_HOME folder called <code>bin</code>. Bin is short for 'binaries' and it's where all these little programs are. So what I want to do is make sure this <code>bin</code> directory is somehow a part of my PATH environment variable.

To update the PATH variable on macOS, we open up our <code>.bashrc</code> file that we've been editing, and check to see if any PATH variable is being exported in it. If so, we can just modify that line. If not, we can create a new line that starts with:

        export PATH=$PATH

The first thing I do is set PATH to itself. Why do I set PATH to itself? Well, PATH may have some values in it already set from some previous shell startup script, and I don't want to clobber those. So I first make sure that I'm keeping any existing values. Then, I add the directory I want to the PATH, by changing the line so that it looks like this:

        export PATH=$PATH:$JAVA_HOME/bin

Now I am using two environment variables to define PATH -- any previous PATH value, and my new JAVA_HOME variable. Since I'm using JAVA_HOME, I need to write this PATH export line after I define the JAVA_HOME variable. And all I'm doing is telling the PATH that I want whatever is inside the bin dir inside JAVA_HOME to be on the path. I can add as many directories as I want to the PATH, by separating them with colons. This is how I build up my list of directories in my path.

On Windows, we head to the Environment variable editor as before. In either your user or system variables, you should see an existing variable called <code>Path</code>. Click to edit it. The editor for the Path variable is special - it shows us a list of paths, and we can add to this list by clicking "New". So click new and add <code>%JAVA_HOME%\bin</code>, then save it to the list. Notice that again we are using another environment variable to help define the Java binary directory we want to use in our path. This means that if you change the value of JAVA_HOME down the line, say because you downloaded a new version of Java, you don't also need to update your Path.

Some older versions of Windows have the Path variable as one long string, in which case the value works just the same as it does on Mac, just separate each directory with semicolons instead of colons (and make sure to use the Windows format for variables and paths of course).

We have now set our Java home environment variable and our path variable in such a way that we should have access to the Java binary programs from our shell without needing to know where they are anymore. So open up a new terminal or command prompt, and check if this is the case by trying to run the <code>java</code> command. If it works, you should get a bunch of help output from <code>java</code>, as shown below. And note that whether you are on Mac or Windows, you can run the same Java command.

        lanabegunova@Lanas-iMac ~ % java
        Usage: java [options] <mainclass> [args...]
                   (to execute a class)
           or  java [options] -jar <jarfile> [args...]
                   (to execute a jar file)
           or  java [options] -m <module>[/<mainclass>] [args...]
               java [options] --module <module>[/<mainclass>] [args...]
                   (to execute the main class in a module)
           or  java [options] <smycefile> [args]
                   (to execute a single smyce-file program)


         Arguments following the main class, smyce file, -jar <jarfile>,
         -m or --module <module>/<mainclass> are passed as the arguments to
         main class.


         where options include:


            -cp <class search path of directories and zip/jar files>
            -classpath <class search path of directories and zip/jar files>
            --class-path <class search path of directories and zip/jar files>
                          A : separated list of directories, JAR archives,
                          and ZIP archives to search for class files.
            -p <module path>
            ...

If it doesn't work, if you didn't get output similar to this, then go back through these steps or check the java installation documentation or usage for your shell to make sure that you've set up your environment correctly. Remember we can always echo environment variables to make sure they have the content we expect. For example, I can echo out the two variables I added:

        % echo $JAVA_HOME
        /usr/local/Cellar/openjdk@11/11.0.15/libexec/openjdk.jdk/Contents/Home

        % echo $PATH
        /usr/local/opt/openjdk@11/bin:/Library/Frameworks/Python.framework/Versions/3.11/bin:/usr/local/bin:/System/Cryptexes/App/usr/bin:/usr/bin:/bin:/usr/sbin:/sbin:/Library/Java/JavaVirtualMachines/openjdk-11.jdk/Contents/Home/bin:/usr/local/opt:/Users/lanabegunova/Library/Android/sdk/emulator:/Users/lanabegunova/Library/Android/sdk/platform-tools

(And on Windows, make sure to use the % signs of course). So that's it for setting up Java and our Java environment.















