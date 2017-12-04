# Linux Security

## Intro to Linux Security
Now that you're going to be putting another computer out there on
the Internet, accessible by anyone, we need to discuss security.
Not only for your own application's sake, but
for everyone else out there as well.
If a bad guy gets access to your server, they can do anything they want with it.
Send spam, launch denial of service attacks, and so much more.
In this lesson we'll discuss a number of security-related topics,
including managing users, packages or
the software installed on the server, various methods of authenticating users,
how Linux manages file permissions, and finally, how to configure a firewall.

## The Rule of Least Privilege
One of the most important rules in security is the rule of least privilege.
Put simply, this means a user or
an application only has enough permission to do its job, nothing extra.
You've already experienced this within your virtual machine,
although you may not be aware of it yet.
When we access our virtual machine, using vagrant ssh.
We're logged in as a standard user named, vagrant.
Let's try running a command only an administrative user would be
allowed to run.
Let's list all of the files with in the ubuntu users ssh directory.
You see, we get this error here, permission denied.
Only the ubuntu user or
the root user can read the files within this directory.
You may be thinking, but I am the administrator of this virtual machine.
I am root.
So how do I actually log in as root?
It's time to learn about super user.

## Becoming a Super User
Since every Linux machine comes with the user name root and
that user is super powerful, they can do anything they want on this machine.
It's very common to disable the ability to remotely log in as root.
Instead, we'll log in as a user we create, and
then we can run individual commands as root by using another command.
This is to make any potential attacker's job a little more difficult
by eliminating the username that they already know exists on this on this box.
Our vagrant virtual machine has already set up the security pattern for us and
many other cloud providers will do this for you, as well.
If not, it's highly advised that this be one of the very first things you do when
you're setting up a new server.
We'll cover exactly how to do that a bit later.
>> Let's run that same command again,
except this time we'll prepend the command with this sudo command here.
Now we see the results.
The pseudo command ran this command as if we were root.

## sudo vs su
It's typically regarded as a best practice that you not
use the SU command.
Why?
The rule of least privilege that we discussed earlier.
Do you really need to switch your entire working context over to the root user
to run a single, or even a few, commands?
What happens if you forget that you are currently within SU?
You could potentially do some extremely damaging operations, and
there's no safety net there to warn you when doing so.
Now, not every user has the ability to work as the superuser.
You have to give that user those permissions specifically.
We'll cover that in more detail when we start adding new users.
For now, you know how to perform operations as the root user, and that's
all we need to start managing software known as packages on this machine.
So let's dive into that a bit.

## Package Source Lists
When you need new software, what do you typically do?
You might visit an app store like this and download it or
you could actually walk into a store and buy a physical copy.
You have a lot of different options.
You might even say you have a list of options.
Now how often have you seen software for Linux sitting up on a store shelf?
Not very often, if at all.
But we still have a list of various places we can go to get software.
This is called a package source list.
Let's take a look.
All of your available package sources are listed in this file
/etc/app/sources.list.
Remember when we were discussing distributions,
we said each one approves and releases packages in their own way and
that's one of the big ways in which they differ.
This is the package's list for your current version of Ubuntu.
As you skim through the file, you'll see some pretty recognizable parts.
We see a URL here and the word trusty looks familiar.
That's the code name for the version of Ubuntu we're running.
We know Ubuntu is also based off of Debian, so
that's probably what this deb stands for here.
This is a list of software repositories that Ubuntu set up for us automatically.
There are a lot in this list and each of these would be referenced
when you try to update or install new software.
Speaking of which, let's go ahead and
make sure that all of our software is up to date.

## Updating Available Package Lists
One of the most important and simplest ways to ensure your system is secure
is to keep your software up to date with new releases.
Because Linux systems focus on reliability and they run a variety of
complex applications that have numerous dependencies of their own, most Linux
distributions do not auto-update the software installed on the system.
You'll need to do this yourself and
test your apps to make sure any recent updates don't break your application.
The first step to upgrading your installed software is to update your
package source list.
We do this with the command sudo apt-get update.
See the sudo there?
We have to run this as the root user.
The update command will run through all of the repositories we saw within our
etc/app/sources.list file, and it will check to see what all software is
available and what those version numbers are.
This command doesn't actually perform any changes to the software
on your system.
It just makes sure your system is aware of the latest information stored
within all of these repositories that you're making use of.

## Upgrading Installed Packages
Now that our system is aware of what all software is available, and the most
recent version numbers, it's now time for us to actually update the software.
We do this with the command, sudo apt-get upgrade.
Once again, we have to use sudo.
Remember this is an administrative task that we have to run as the root user.
After a few seconds, you'll be presented with a list of everything that's going
to change on your system.
And this question, do you want to continue, yes or no?
We'll say yes in a second, but let's review this information real quick.
Here we have a list of packages that will be upgraded.
Some of these names look familiar.
See a name python here, python down here.
But others, not so much.
This early in setting up a new machine, you can be pretty safe in just accepting
that the system is always making the best decisions for you.
Later on, when you actually have your web application running on this system,
and it's serving your users.
You're going to want to take more care in reviewing this list.
And testing everything in a non-production environment
before performing similar operations on your production server.
For now, we'll just hit yes and we'll go take a coffee break as
all of these new versions are downloaded and installed.

## Other Package Related Tasks
The apt get application is your main interface to a ton of package related
functionality.
We can check out everything you can do by reading the man page for
it with this command, man apt get.
We see here it can install packages, it can even remove packages,
it can do all sorts of stuff.
For now, let's see if there are some packages that are no
longer required that we can just automatically remove.
We'll do this with the command apt-get autoremove.
And once again, it's an administrative task that has to be run as root.
So we use sudo.
After a few seconds,
we're returned back to our prompt to let us know everything is all done.
Now let's use apt-get to install new software.
We'll install an application called finger.
It's something we'll use a little bit later on when working with users.
We do this by typing the command apt-get install finger, and
once again using sudo.

## Discovering Packages
So how did I know the package I wanted to install was called Finger?
Most of the time the package names are pretty easy to guess, but
not all the time.
For instance Python has two extremely popular versions,
you have Python 2 and Python 3.
It may be a bit more difficult to figure out what the name of that
package would be.
Fortunately, each distribution tends to publish an easy to browse version
of their repositories.
For Ubuntu, you can search for packages at packages.ubuntu.com.
If I were to search for Python and I look at this top result that says 2.7,
so that's Python version 2.
If I search for Python 3 and
I look at the top result it says 3.40, so I know that's Python version 3.

## Using Package Search
Let's get a bit more practice searching for
packages using Ubuntu's package website.
Use the site to search for the package names for Apache HTTP Server,
PostgreSQL, and Memcache on Ubuntu Trusty.
Enter your answers in these boxes.
You don't need to include the full app to get command, just the package name.

The correct answers are apache2, postgresql, and memcached.

## Using Finger
Now that we've installed Finger, let's use it.
This application will look up various pieces of information about a user, and
display it in an easy to read format.
If I type Finger, the command will output information about
all of the users currently logged into the system.
You can see the vagrant user here, that's us, and
the last time we logged in.
You can also pass a username to the Finger application
to see additional information about a specific user.
Type finger vagrant, and you'll see some additional information
including our home directory and what shell we're using.

## Introduction to /etc/passwd
So where is finger retrieving all of this information such as our user name,
our home directories, the shell?
Much of this information is found within a file that stores information
about each user.
This file is called etc/passwd.
Let's take a look at that file using the cat command.
Each line within this file is an entry for a single user, and
each entry has a number of fields that are separated by colon characters.
Let's find the entry for our current user, vagrant.
The line looks like this,
vagrant: x:1000:1000: :/home/vagrant:/bin/bash.
These two numbers 1000:1000 might be different on your system, but
that's okay.
It's nothing to worry about.
The first field reads vagrant and that's this users username.
The second field used to store encrypted passwords.
Historically storing encrypted passwords in this file wasn't an issue
as the hardware was too slow to crack a well chosen password.
These days, almost every distribution will just insert a character that is
ignored in this field.
In this case, ubuntu uses an x.
The third and fourth field store your user ID and group ID.
We'll discuss these a bit more when we get into talking about file permissions.
There's a fifth field here that may be hard to see since it doesn't include
any text.
It's empty.
This field is used to store a better description about this user.
You can see one user does have a better description here.
Gnat's and then this full description, gnats bug reporting system admin.
The last two fields are the user's home directory and finally,
the user's default shell.
Our home directory is /home/vagrant as we already knew.
And our default shell is bin/bash.

**/etc/passwd**

This is a very important file on your system! It's used to keep track of all users on the system. Run cat /etc/passwd and look at the output; each line is organized in this format:

**username:password:UID:GID:UID info:home directory:command/shell**

Let’s run through what each of those mean:

1. username: the user’s login name
2. password: the password, will simply be an x if it’s encrypted
3. user ID (UID): the user’s ID number in the system. 0 is root, 1-99 are for predefined users, and 100-999 are for other system accounts
4. group ID (GID): Primary group ID, stored in /etc/group.
5. user ID info: Metadata about the user; phone, email, name, etc.
6. home directory: Where the user is sent upon login. Generally /home/
7. command/shell: The absolute path of a command or shell (usually /bin/bash). Does not have to be a shell though!

## Reading /etc/passwd Entries
The following is an entry in the systems /etc/passwd file.
What is the following user's home directory?
Enter your answer here.

The users home directory can be found, in the second to the last field.
In this case, that value is /var/list.

## User Account Info
Using a combination of the finger application and reading the etc/password
file, identify the following information for the root user.

User ID and the Group ID are both 0.
In fact, this is a special rule on Linux systems.
The root user will always have 0 as its User and Group ID.
The Home Directory is /root.
And the default shell is /bin/bash.

## Introduction to User Management
If you recall, when we were discussing Sudo, we mentioned that it's
a common pattern to disable the ability to log in as root, and
to only log in as a different user that has the ability to use Sudo.
This is a security measure,
since every bad guy out there knows every Linux box has a user named root.
By disabling this account from remote log,
in we remove a very easy attack vector.
Now vagrant took care of this for us.
They created a user name vagrant and
we just type vagrant ssh from our terminal to automatically connect.
But not every hosting provider is going to set something like this up for you.
So let's do this ourselves.

## Creating a New User
We can create a new user by using the adduser command.
This is an administrator feature, so we'll have to use sudo as well.
Let's go ahead and create a new user named student.
We'll then be asked to enter a password for the student.
I just used the word student, but as you can see,
in these password fields, you don't see what you're typing.
We're then asked the number of additional questions about the user.
All of these are optional, so you can ignore them.
But I'm going to go ahead, and
add a bit more additional information here in the Full Name section.
And that's all there is to it.
We can confirm this user was created by using the finger command.

## Connecting as the New User
Now, that we've created this user.
Let's go ahead and connect to our server as that user.
I've opened a new terminal here, and this terminal is on my local machine.
I have not connected to the server yet.
I can connect to the server using this command, ssh student@127.0.0.1 -p 2222.
Let's break this command down a little bit.
We've been connecting to our server using vagrant SSHwhich
is just a shortcut for all of this.
SSH is the application we use to remotely connect to the server.
And 127.0.0.1 is the IP address we want to connect to.
This is a standard IP address that always means localhost or
the same computer I'm currently on.
The student@ is the user we want to log in as.
We want to log in as student@ this IP address.
Finally, the -p 2222 flag tells us to connect using port 2222.
When Vagrant set up our virtual machine, it automatically set up this port on
our local machine to forward to the virtual machine.
After hitting Enter, you may be asked an authenticity verification question.
Just hit yes, and then you're asked for the user's password.
Enter that, and you'll be logged in as the student.

## Introduction to etc sudoers
Now that we're logged into our server as the student user, let's try and
run a sudo command.
We'll try and run sudo cat /etc/passwd.
We're asked for a [sudo] password for the student which is just the standard
password we set for the user when we initially made the user.
We now get this warning.
The student is not in the sudoers file.
This incident will be reported.
Our students does not have permission to use the sudo command.
So, let's fix that.
I switch back to my other terminal where I'm logged in as vagrant.
This is a user we know can run sudo commands.
They can perform administrative tasks.
Now, the list of users that are allowed to do this
is within the etc/sudoers file.
Let's read that file using sudo cat /etc/sudoers.
Here we can see that the root user is listed
along with a few groups using % and then the name of the group.
On some systems you would just add the student user just like this,
using a special program called visudo, that's allowed to edit this file.
But on Ubuntu, it handles it a bit differently.
If you look at the very bottom of this file,
there's a here that says includedir /etc/sudoers.d.
This command tells the system to also look through any files in etc/sudoers.d
and include those as if they were written directly within this file.
This is a common pattern since distribution updates could
overwrite this file.
And if that were to happen, you would lose all of the users you added.
By keeping your customizations in this other directory,
the system eliminates that risk.
Let's see what files are currently included in that directory
by running sudo ls etc/sudoers.d.
We see a file here called vagrant and
that makes sense since we're actually using sudo here.
Even though vagrant wasn't within our etc/sudoers file itself, this file is
being included by this directory, giving this user the permissions it needs.

## Giving Sudo Access
Let's go ahead and give our student user access to sudo themselves.
I'll first copy the vagrant file and name it student.
I'll then need to make a small edit to this file and
I'll just use nano to do that.
This second line here is actually what's doing all of the work.
The file name doesn't mean anything,
so we'll change the word vagrant here to student.
There are a few more options here.
And if you'd like to understand them all,
I've placed a link in the instructor notes for more information.
For now, we just want student users pseudo access to function exactly as
the vagrant users currently does.
After saving that file, I've switched back to my terminal where I'm logged in
as the student and we'll try to run this sudo command again.
And there we go, we see the results.
Our student user now has access to use sudo.

## Resetting Passwords
We've just provided super user access to the user student, but
if you remember, that user has an extremely simple password.
The user themselves could reset their password using the passwd command, but
you can't rely on that user to do so all the time.
As a super user, you can foresee users password to expire.
Use the man page and read through the documentation for the passwd command.
Enter the command that you would use to force
the student user's password to expire.

You would use the command, sudo passwd -e student,
to force the student user to reset their password the next time they login.
Student is the user and this -e sets that password to expire.

## Another Authentication Method
You just added a powerful user to your server that authenticates using a user
name and password.
Hopefully, you chose a strong password since attackers will soon start running
bots against your server attempting to guess any valid usernames and passwords.
This is going to cause all sorts of issues for your server.
Your logs are going to be filled with invalid login attempts, and
if one of these hackers manages to gain access,
well that's about the worst thing we could possibly imagine.
There's another way to perform user authentication that's much more secure.
It doesn't rely on passwords, which we're pretty horrible at making secure,
since we have to make them simple enough to memorize.
Instead, this form of authentication, called key based authentication,
relies on physical files located on the server and
your personal machine, the one you're logging in from.
Before we get into key based authentication,
let's demonstrate how public key encryption generally works.

## Public Key Encryption
Let's imagine I wanted to send a message to Cameron without anyone else being to
see that message.
If I were to just place this note on Cameron's desk, anyone could come by and
read it.
I could find a box with a lock and lock the message away but
then I have to somehow get this key to Cameron.
That's not going to work because anyone will be able to come by and
grab the key.
But what if Cameron already had a box set up on his desk,
with his own lock, and the key to unlock that box is always in Cameron's pocket?
He never shares that key with anyone.
I can then come by his desk, place my message in the box, and lock it.
And no one else can ever see that message.
In this example, the box is called the public key.
The box can be left out in the open, shared around without any consequence.
The key in Cameron's pocket is called a private key.
Cameron never lets anybody else borrow that key or see it.
This combination of public and
private keys allows me to securely communicate with Cameron.
This same cryptography trick can be used to authenticate a client with a server.
The server will send a random message to the client.
The client will encrypt that message with their private key, and
then send that encrypted message back to the server.
The server will decrypt this message with their public key and
if that value equals the same value they sent, then everything checks out and
the client has authenticated.

## Generating Key Pairs
We'll generate our key pair on our local machine, not on our server.
Remember, you never, ever want to share your private key with anyone else.
It should remain firmly in your possession at all times.
For this reason, you always generate key pairs locally.
If you were to generate the key pair on the server,
you cannot claim that the private key has always been private.
We'll generate our key pair using an application called ssh-keygen.
You will first be asked to give a file name for the key pair.
I've given this one the name users/udacity/.ssh/linuxcourse.
This directory here is the default directory that key pairs should exist
in, so I advise you to keep that the same.
But you can name it what you'd like.
We'll then add a pass phrase to our key pair,
just in case someone does happen to get these files.
This pass phrase will prevent them from actually using them.
Once done, you'll see that ssh-keygen has generated two files,
linuxCourse and linuxCourse.pub.
This file, linuxCourse.pub,
is what we'll place on our server to enable key based authentication.

## Supported Key Types
The ssh-keygen application can generate key pairs of various types.
The key you just generated was an RSA key,
which is the default type if you don't specifically define a different type.
Read through the ssh-keygen man page and
determine what other key types are supported by the SSH version 2 protocol.

The SSH version two protocol supports the DSA, ECDSA,
ED25519, and RSA key types.
MD5 and SHA256 are hashing algorithms that are not suitable for
public key encryption.

## Installing a Public Key
Now that we've generated our key pair locally, we still need to place
the public key on our remote server, so SSH can use it to log in.
There are multiple ways to do this and there are even some applications
that will do most of the work for you, but we're going to do it the manual way.
First we want to make sure we're logged into our server as the student.
I'll first create a directory called .ssh using the mkdir command
within my home directory.
This is a special directory where all of your key related files must be stored.
I'll then create a new file within this directory called authorized keys.
This is another special file that will store all of the public keys
that this account is allowed to use for
authentication, with one key per line in that file.
Now, back on my local machine I've read out the contents of linuxcourse.pub,
and I just want to copy that.
Then, back on my server as the student user,
I went to edit this authorized key file.
And in here I'll just paste in that content and save it.
The final thing we need to do is set up some specific file permissions
on the authorized key file and the SSH directory.
This is a security measure that SSH enforces to ensure
other users cannot gain access to your account.
We'll discuss file permissions in a lot more detail shortly.
For now we'll set the permissions using the following commands.
We'll run chmod 700 on our SSH directory, and
chmod 644 on the authorized keys file.
Finally we're all done and we can now log in as the student user, but
instead of using user name and password.
This I flag and the definition here of what key pair we want to use
will allow me to login using that key pair.
If you set a passphrase for your key pair, you'll be asked to enter that.
But, once you're done, you'll see you've logged into the server and
you did not have to enter your remote password for this user.

## Forcing Key Based Authentication
The final thing you'll want to do to secure the authentication process
is to disable the password base logins.
This will force all of your users to only be able to login using a key pair.
To do this, you'll have to edit the configuration file for SSHD.
Which is the service that's running on the server listening for
all of your SSH connections.
This configuration file is located at etc/ssh/sshd_config.
And you can edit it using sudo nano.
There are a lot of options in here, and you can read through them all to get
a better understanding of how SSH is configured.
The comment lines start with the hash symbol here.
And they're pretty good at explaining what everything does.
The line we're looking for is right here, PasswordAuthentication yes.
We just want to change that to no, and then we'll save the file.
Now, the SSHD service is currently running, and
it only reads its configuration file when it's initially started up.
So we need to restart the service so
it runs with the new configuration option we just made.
We restart the service by sudo service ssh restart and
that's all there is to it.
Now all users will be forced to log in using a key pair.
SSH will not allow users to log in with a user name and password any longer.

## Introduction to File Permissions
A bit earlier you changed some permissions on the authorized key file
and the .ssh directory using a command called chmod.
But what exactly does that mean?
Let's dive a bit deeper into how Linux manages file permissions.
I've listed the contents of the students' home directory.
And we're going to look at three pieces of information provided here.
The first column we've looked at briefly before.
Remember the d means that this is a directory.
The dash means that it's a file.
Now, you might think that there are nine additional spaces here for information,
but what's actually happening is there are three separate sections and
each of those sections has three pieces of information.
Let's break out this entry here for bash.rc.
The three individual entries are rw-, r--, and r--.
Let's go ahead and label each of these entries.
This first one I'm labeling owner, then group, and finally everyone.
This means the owner can read and write the file.
The dash here indicates that the owner cannot execute the file.
If they could, there would be an x here.
Users within the group can read this file, but they cannot write or
execute it.
Finally, everyone can read this file, but they cannot write or execute it.

## Owners and Groups
So how do we identify who the owner in the group are?
If we look back at our directory listing,
we'll see two columns, here in the middle.
Most of the entries read student student, but
there's one here that reads root root, and I'll come back to that one.
These two columns identify the owner and the group for each entry in this list.
Now it's important to remember, although each of these has the same word,
student, listed in the two columns,
they're in two entirely different things.
The system has a username student, which is the owner of the file.
And a group name student,
which was automatically created when we made this user.
This is pre-common practice on a Linux system,
to have a group name the same as the user.
Just remember that they are two entirely different things.
So, what's up with this entry here that has root root?
We can see that the entry's name is .., and that's just a shortcut for
the parent directory.
And we know we're in our student's home directory.
So this entry here, is the equivalent of /home.
That directory is owned by the root user, and has a group of root.
And if we look at the permissions,
we see that only the root user can write to that directory.
Lets test this out for ourselves.
I'll move into the directory using cd..
And then I'll try to write a file.
You'll get a permission denied error,
just as this permission system told us we would.
Only root is allowed to write in this directory.
And our current user's definitely not root.
We can still list the files in this directory, because we have read access,
and we even entered the directory because we have execute.
We just can't write.

## Identifying Owners and Groups
Explore the file system a bit and identify the owner and
group for each of these files or directories listed here.

All of these files are owned by the user, root.
But some of them have different group owners.
.bashrc has a group owner of root, /var/local has a group of staff,
/var/log has a group syslog, and this file has a group of root.

## Octal Permissions
So we know permissions are represented as Rs, Ws and Xs, indicating read,
write and execute.
But when we changed the permissions of some files earlier we used numbers.
How did the digits we entered translate to these values?
We can translate these values as follows.
Rs are equal to 4.
W's are equal to two, X's are equal to one and
if we don't want any permissions, that will be a zero.
By adding the numbers together,
we end up with a result identifying the full set of permissions to apply.
For example, if we wanted to give read and
execute permissions, we'd have values of four and
one, which when added together, gives us a final value of five.
To represent, read, and
execute permissions, you would use the number five.
But remember, permissions are done in sets of three to identify what
permissions are set for the individual user, the group, and everyone.
Let's analyze our student's .bashrc file once again.
And convert its current permissions into octal form.
The current permissions for this file are rw dash, r dash dash, and
r dash dash, r is a 4 and write is 2.
So the user value would be 6.
For group, we just have an r.
So the value is 4.
And for everyone we have a value of 4.
To represent this permission set in octal form, we'd use the value 644.

## Octal File Permissions
Convert these symbol-based file permissions to their octal form.
Remember, read is four, write is two,
execute is one, and nothing would be zero.

The first is 644.
This one is 777.
The third one is 755.
Remember, this D does not chance the octal permissions.
It just represents that this is a directory.
The final one is 600.

## chgrp and chown
We've already seen how to change file permissions using the chmod command.
But what if you need to change a files group or owner?
There are also commands that allow you to do that.
They are Chown.
C-H-O-W-N.
And change group.
C-H-G-R-P.
We'll play with this bash history file here located on our home directory.
Its permissions are set so only the owner can read and write the file.
This file stores a recent history of every command the user has typed, so
it's for security reasons only that the user can read and modify the file.
If you run cat on bash history,
you'll see that we can currently read this file.
If we change the file's group to root using this command sudo chgrp root and
then then name of the file.
If we try to cat this file again, you'll see that we can still read it.
The group has no permissions on this file, so there's pretty much no effect.
Our ability to read this file right now is determined by the owner setting,
not the group setting.
But now, if we change the owner to root of the bash history file,
you'll see that we can no longer read the file.
Permission is denied, and this is because only the owner can read and
write the file and that owner's root.
Our current user student would fall in the everyone category and
they have no permission at all to read this file.
Go ahead and change the owner and the group back to student on this file.
We were just experimenting to show these commands and
when you might need to use them.
Now let's move on to the last security topic we'll discuss, firewalls.

### Errata
The `.bash_history` file is created by the `bash` shell the first time you log out of your Linux system. This means that if you are logging into the system for the first time, it won't have been created yet. If you log out and log back in (e.g. `vagrant ssh` if you're using Vagrant here) then the file will be created.

You can also experiment with `chown` and `chgrp` using another file. To create an empty file named `testfile`, use the command `touch testfile`.

## Intro to Ports
You now have a server sitting out there on the Internet and this server is doing
a lot of different things and talking to other devices on the Internet.
Depending on your application it could be responding to web requests,
database queries, sending and receiving email, and let's not forget
handling the SSH sessions we've been using this whole time.
But how does your server know which application is in charge of handling
each type of request?
The answer is ports.
Each of your applications are configured to respond to requests destined for
a specific port.
For example, a web server would by default respond on port 80,
the default port for HTTP.
We can control which ports our server is allowed to accept requests for
using an application called the firewall.
We'll do that shortly, but for now let's explore some common ports.

## Default Ports for Popular Services
Below are a few of the most common services a web server would run.
Identify the default port for each of these services.
- HTTP runs on port 80.
- HTTPS is 443.
- SSH is 22.
- FTP is 21.
- POP3 is 110.
- And SMTP is 25.

## Intro to Firewalls
Just because our server can listen on every single port, for
any type of request, that doesn't mean we should.
The rule of least privilege, which we've discussed throughout this entire lesson,
tells us we should only listen on the ports required for
our application to function correctly.
We can configure which ports we want our server to listen to
using an application called a firewall.
Let's imagine this wall is our firewall application.
And each of these slots is a port.
We currently have all the slots filled, which means I can't
pass data from one side, the Internet, to the other, my server.
You could say I'm denying all incoming requests, but
if I open one of these boxes, we'll choose port 80 here.
I can now pass information through.
The server on the other side can now fully function as a web server
with the added benefit of completely ignoring these requests
that we know we're not interested in.
Let's go back to our terminal and start configuring our server's firewall now.

## Intro to UFW
Ubuntu comes with a firewall pre-installed called ufw, but
it's not currently active.
You can verify this by typing command, sudo ufw status.
Status: inactive.
Let's start adding some rules to our firewall,
then we'll actually turn it on.
If we think back to the wall of boxes from our last video,
we were initially blocking all incoming requests.
This is a good practice, as it makes it much easier to manage your rules.
Just block everything coming in, then only allow what you need.
We'll establish this default, deny incoming, by using this rule.
We can also establish a default rule for our outgoing connections,
any request our server is trying to send out to the internet.
We'll set this rule by using this command, sudo ufw the default rule,
allow all outgoing.
Go ahead and check the status of your firewall by typing, sudo ufw status.
You'll see that it's currently still inactive, which is a good thing.
We're just configuring our firewall now.
We actually have to turn it on ourselves once we have everything how we want.
If we were to turn the firewall on now,
we'd we blocking all incoming connections including SSH, which means
our server would be dead in the water and completely inaccessible to us.
It's now time for us to start configuring the firewall to support
the various ports and the applications we know we'll need.

## Configuring Ports in UFW
Let's start allowing the ports we know we'll need for
the applications our server will be supporting.
First and foremost we know we'll need to support SSH so
we can continue administering this server.
Normally you would do this by typing sudo ufw allow ssh and
you can go ahead and do that now.
But remember, we're using a Vagrant virtual machine and
Vagrant set up our SSH on Port 2222.
So we'll need to allow all TCP connections through Port 2222 for
SSH to actually work in our scenario here.
For now, the only other application we plan to support is a basic HTTP server,
so we can allow this by using sudo ufw allow www.
And with that, we can now enable our firewall with sudo ufw enable.
This step here can be a little hair raising,
because our SSH connection is reliant upon these rules being correct.
If all of a sudden you lose your SSH connection to your server,
it's a pretty clear indicator that you messed up some of your rules.
Some cloud providers do offer a way to regain access to your system
through an external control panel.
But many others, you're just out of luck at this point.
For this reason, I recommend configuring your firewall
pretty early in the server setup process.
Finally, we can confirm all of our rules are set up as we indicated
by using the sudo ufw status command.
We'll see all of our rules here and that our firewall is currently active.

## Conclusion
Congratulations.
You now have a server updated and
configured, sitting out there on the wild west that is the internet.
Best of all, you can rest easy at night knowing your server is safe and secure.
Where you do go from here?
It depends on your needs.
There are a lot of different types of servers, email servers,
chat servers, web application servers.
Generally, the only big differences between each of these
is the software they have installed and the ports that they have open.
I've placed a few server set-up walk throughs in the instructor notes below,
that should get you started.
I'd encourage you to start by setting up a web application server,
installing Apache in a database server like PostgreSQL.
Good luck, have fun, and remember, experiment.
This is your own little piece of the Internet.

Here are a few tutorials that will walk you through how to configure your server in a variety of use cases:

* ["LAMP" Stack (Linux, Apache, MySQL, PHP)](https://www.digitalocean.com/community/tutorials/how-to-install-linux-apache-mysql-php-lamp-stack-on-ubuntu-14-04)
* ["LEMP" Stack (Linux, nginx, MySQL, PHP)](https://www.digitalocean.com/community/tutorials/how-to-install-linux-nginx-mysql-php-lemp-stack-on-ubuntu-14-04)
* [PEPS Mail and File Storage](https://www.digitalocean.com/community/tutorials/how-to-run-your-own-mail-server-and-file-storage-with-peps-on-ubuntu-14-04)
* [Mail-in-a-Box Email Server](https://www.digitalocean.com/community/tutorials/how-to-run-your-own-mail-server-with-mail-in-a-box-on-ubuntu-14-04)
* [Lita IRC Chat Bot](https://www.digitalocean.com/community/tutorials/how-to-install-the-lita-chat-bot-for-irc-on-ubuntu-14-04)
