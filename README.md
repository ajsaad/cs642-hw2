# CS 642: Computer Security - Homework Two

This homework assignment covers exploitation of web security. You must work with a partner. There are four parts; all are required.

Much of this assignment is borrowed from a Stanford CS 155 homework assignment. Thanks for their hard work!

It is due **March 28, 2016** by 11:59 PM local time. 

## Web Security Project
You are given the source code for a banking web application written in Ruby on Rails and you will implement a variety of attacks.
The web application lets users manage bitbars, a new form of crypto currency. Each user is given 200 bitbars when they register for the site. They can transfer bitbars to other users using an intuitive web interface, as well as create and view user profiles. You have been given the source code for the bitbar application. Real web attackers may or may not have access to the source code of the target website, but having source might make finding the vulnerabilities a bit easier. Note that we will grade assignments on an unmodified copy of the target web application.

## Setup
A VirtualBox image with Ubuntu Desktop, Ruby on Rails, and the bitbar application is available at http://pages.cs.wisc.edu/~agarwalt/Ubuntu-VM%20for%20Assignment%202.vmdk
Note this file is YUUGE! It is 5.5GB. Downloading it from a campus internet connection may be much faster than downloading it from home internet connection.




The Bitbar application should be run locally on your machine (using Ruby 2.0 and Rails 4.0). When the server is running, the site should be accessible at the URL http://localhost:3000. This is the environment the grader will use during grading, and therefore all of your attacks should assume the site is located at the URL http://localhost:3000.
On the client side, we will grade using the latest version of Chrome. The specific browser should not make a difference for most of the attacks, but we recommend you test your attacks in Chrome in order to be safe. Detailed setup instructions:
1. Install Ruby 2.0 and Rails 4.0. See the following page for instructions: http://www.stanford. edu/class/cs142/cgi-bin/railsInstall.php
2. Download and extract the starter code from the CS 642 website.
3. Navigate to the bitbar directory of the starter code. This is the Rails root directory.
4. Run the following command to install the Rails plugins you will need: bundle install
5. To start a server, run the following command on a terminal in your bitbar directory: rails server
1
This will make the bitbar application available in your browser at the URL http://localhost: 3000 You can close the server by pressing Ctrl + C in the terminal. Note that you do NOT need to restart the server every time you make a change to the Rails source; the running Rails server will automatically update the website when you make a change to the source code.
3 Attacks
3.1 Attack A: Cookie Theft
• Your solution is a URL starting with http://localhost:3000/profile?username=
• The grader will already be logged in to Bitbar before loading your URL.
• Your goal is to steal the the user’s bitbar session cookie and send it to http://localhost:3000/steal_cookie?cookie=...cookiedatahere...
• You can view the most recently stolen cookie using this page: http://localhost:3000/view_stolen_cookie
Only the first 64 characters of your stolen cookie will be stored/displayed. This is expected and ok.
• The attack should not be visibly obvious to the user. Except for the browser location bar (which can be different), the grader should see a page that looks as it normally does when the grader visits their profile page. No changes to the sites appearance or extraneous text should be visible. Avoiding the red warning text is an important part of the attack. It is ok if the number of bitbars displayed or the contents of the profile are not correct (so long as they look normal). Its also ok if the page looks weird briefly before correcting itself.
• Put your final answer in a file named a.txt
• Hint: try adding things random text to the end of the URL. How does this change the HTML
source code loaded by the browser?
3.2 Attack B: Cross-Site Request Forgery
• Your solution is a short HTML document named b.html that the grader will open using the web browser.
• The grader will already be logged in to Bitbar before loading your page.
• Transfer 10 bitbars from the graders account to the attacker account. As soon as the transfer is complete, the browser should redirect to http://pages.cs.wisc.edu/~rist/ 642-fall-2014/ (so fast the user might not notice).
• The location bar of the browser should not contain localhost:3000 at any point.
• The framebusting code may make this attack harder than necessary. For this attack - and ONLY this attack - you may disable framebusting on a page by adding disable fb=yes to a URL. For example:
     http://localhost:3000?disable_fb=yes
 2
3.3 Attack C: SQL Injection
• Your solution is one or two HTML documents named c.html and (optionally) c2.html. The grader will open c.html using the web browser.
• The grader will already be logged in to Bitbar before loading your page.
• The grader will interact with the web page in a way that is reasonable. This means that if there is a button on the web page for the user to click, the grader will click it. The same goes for any other interaction on the page.
• After interacting with the page, 10 bitbars should be transferred from the graders account to the attacker account. As soon as the transfer is complete the browser should be redirected to:
http://pages.cs.wisc.edu/~rist/642-fall-2014/.
• Your attack must work by involving user interaction (i.e. do NOT just do another CSRF attack for this). In particular, you attack must target the pages http://localhost:3000/ protected_transfer and/or http://localhost:3000/protected_post_transfer, which have CSRF protection enabled. You may NOT directly call http://localhost:3000/transfer or http://localhost:3000/post_transfer in any way for this attack.
• It should not be obvious that your page is loading content from http://localhost:3000.
• There is some basic framebusting code that protects the application from this sort of at- tack. You must come up with a way of bypassing this defense. You may NOT used the disable fb=yes paramater you used in the previous attack.
• Make sure to test your page layout after resizing your browser to make sure the attack will work for reasonable page resolutions (we will test using a minimum resolution of 800x600).
3.4 Attack D: Profile Worm
• Your solution is a profile that, when viewed, transfers 1 bitbar from the current user to a user called attacker and replaces the profile of the current user with itself.
• Submit a file named d.txt containing your malicious profile.
• Your malicious profile should include a witty message to the grader (to make grading a bit
easier).
• To grade your attack, we will cut and paste the submitted profile into the profile of the attacker user and view that profile using the graders account. We will then view the copied profile with more accounts, checking for the transfer and replication.
• The transfer and application should be reasonably quick (under 15 seconds). During that time, the grader will not click anywhere.
• During the transfer and replication process, the browsers location bar should remain at http: //localhost:3000/profile?username=x, where x is the user whose profile is being viewed. The visitor should not see any extra graphical user interface elements (e.g. frames), and the user whose profile is being viewed should appear to have 10 bitbars.
 3
• You will not be graded on the corner case where the user viewing the profile has no bitbars to send.
• Hint: The site allows a sanitized subset of HTML in profiles, but you can get around it. The MySpace vulnerability may provide some inspiration.
4 Deliverables
Create files named a.txt, b.html, and c.html (you can optionally include a second file c2.html), d.txt containing your attacks. You may include a separate README file, which will be used for partial credit if an attack fails. Submission is via Moodle site.
5 Grading
Each attack is worth up to 2 points. If the attack works, then one will receive full credit. Partial credit will be given for a good description of the vulnerability and how an exploit should work.
Beware of Race Conditions: Depending on how you write your code, all of these attacks could potentially have race conditions that affect the success of your attacks. Attacks that fail on the graders browser during grading will receive less than full credit. To ensure that you receive full credit, you should wait after making an outbound network request rather than assuming that the request will be sent immediately.

## The Environment 

You will test your exploit programs within a virtual machine
(VM) we provide, which is configured with Debian stable ("Lenny"), with ASLR (address
space layout randomization) turned off. 

See: https://github.com/ace0/vulnerability-demo for information about VMs and the VM image.

## The Targets

The `targets/` directory contains the source code for the targets, along with a Makefile for building them

Your exploits should assume that the compiled target programs are
installed setuid-root in `/tmp` -- `/tmp/target1`, `/tmp/target2`, etc.

To build the targets, change to the `targets/` directory and type
`make` on the command line; the Makefile will take care of building
the targets.

To install the target binaries in /tmp, run:
```
make install
```

To make the target binaries setuid-root, run:
```
make install
su
make setuid
```
Once you've run `make setuid` use `exit` to return to your user
shell.  Alternatively, you can keep a separate terminal or virtual
console open with a root login, and run `make setuid` (in the
`~user/hw1/targets` directory!) in that terminal or console.

Keep in mind that it'll be easier to debug the exploits if the targets
aren't setuid.  (See below for more on debugging.)  If an exploit
succeeds in getting a user shell on a non-setuid target in `/tmp`, it
should succeed in getting a root shell on that target when it is
setuid.  (But be sure to test that way, too, before submitting your
solutions!)

## The Exploits

The `sploits/`contains skeleton
source for the exploits which you are to write, along with a Makefile
for building them.  Also included is `shellcode.h`, which gives Aleph
One's shellcode.

## The Assignment

You are to write exploits, one per target. Each exploit, when run in
the virtual machine with its target installed setuid-root in `/tmp`,
should yield a root shell (`/bin/sh`).

Your task is to attack these targets. Targets 1-4 are required; target5 is extra credit.

## Hints 

Read the Phrack articles suggested below.  Read Aleph One's paper
carefully, in particular.

To understand what's going on, it is helpful to run code through gdb.
See the GDB tips section below.

Make sure that your exploits work within the provided virtual machine.

Start early! Theoretical knowledge of exploits does not readily
translate into the ability to write working exploits. Target1 is
relatively simple and the other problems are quite challenging.

## GDB tips

Notice the `disassemble` and `stepi` commands.

You may find the `x` command useful to examine memory (and the
different ways you can print the contents such as `/a` `/i`
after `x`). The `info register` command is helpful in printing
out the contents of registers such as `ebp` and `esp`.

A useful way to run gdb is to use the `-e` and `-s` command line flags;
for example, the command `gdb -e sploit3 -s /tmp/target3` in the VM
tells gdb to execute `sploit3` and use the symbol file in `target3`.
These flags let you trace the execution of the `target3` after the
sploit's memory image has been replaced with the target's through the
execve system call.

When running gdb using these command line flags, you should follow
the following procedure for setting breakpoints and debugging memory:

1. Tell gdb to notify you on exec(), by issuing the command `catch
   exec`
2. Run the program.  gdb will execute the sploit until the execve
   syscall, then return control to you
3. Set any breakpoints you want in the target
4. Resume execution by telling gdb `continue` (or just `c`).

If you try to set breakpoints before the exec boundary, you will
get a segfault.

If you wish, you can instrument the target code with arbitrary
assembly using the __asm__ () pseudofunction, to help with debugging.
Be sure, however, that your final exploits work against the unmodified
targets, since these we will use these in grading.

## Warnings 

Aleph One gives code that calculates addresses on the target's stack
based on addresses on the exploit's stack.  Addresses on the exploit's
stack can change based on how the exploit is executed (working
directory, arguments, environment, etc.); in my testing, I do not
guarantee to execute your exploits exactly the same way bash does.

You must therefore hard-code target stack locations in your exploits.
You should *not* use a function such as get_sp() in the exploits you
hand in.

(In other words, during grading the exploits may be run with a different
environment and different working directory than one would get by
logging in as user, changing directory to `~/hw1/sploits`, and running
`./sploit1`, etc.; your exploits must work even so.)

Your exploit programs should not take any command-line arguments.

## Collaboration Policy

You are encouraged to use the internet, the Piazza discussion board for this class, and 
classmates for information about tools, reference material, 
VM setup, and gdb. Please don't discuss solution specifics with anyone beyond your 
project partner.

## Deliverables

You will have three deliverables.

1. You will need to submit the source code for your exploits (sploit1 through sploit4 or sploit5), along
    with any files (Makefile, shellcode.h) necessary for building
    them.

1. Along with each exploit, include a text
    file (sploit1.txt, sploit2.txt, and so on). In this text file,
    explain how your exploit works: what the bug is in the
    corresponding target, how you exploit it, and where the various
    constants in your exploit come from.

1. Finally, you must include file called ID which contains, on each line: 
   UW ID Lastname, Firstname. Two lines, one for each group member. An example:
```
$ cat ./ID
9064562678 Doe, Jane
9038454624 Dough, John
$
```

Put all these files in the 'sploits/' directory and package them into a tarball with the following command (where UWID1/2 are your groups UWID #s):
```
tar -cf UWID1_UWID2_hw1.tar sploits/*
```

Upload `UWID1_UWID2_hw1.tar` to the hw-1 folder on the Desire2Learn website for this course:
https://uwmad.courses.wisconsin.edu/d2l/home/3199130

Use Assignments > Dropbox to find the hw-1 submission folder.

## Grading 

You will receive two points for each exploit that yields a root shell in our testing. 
The extra credit target, target5, is also worth two points.

There will not normally be partial credit,
but we may make an exception depending on your explanatory writeup.
We may also ask you to explain to us how and why each exploit works.
Be sure that each group member understands every exploit you turn in!


## Suggested reading in Phrack, www.phrack.org

- Aleph One, *Smashing the Stack for Fun and Profit*, Phrack 49 #14.
- klog, *The Frame Pointer Overwrite,* Phrack 55 #08.
- Bulba and Kil3r, *Bypassing StackGuard and StackShield*, Phrack 56 #0x05.
- Silvio Cesare, *Shared Library Call Redirection via ELF PLT Infection,* Phrack 56 #0x07.
- Michel Kaempf, *Vudo - An Object Superstitiously Believed to Embody Magical Powers,* Phrack 57 #0x08.
- Anonymous, *Once Upon a free()...,* Phrack 57 #0x09.
- Gera and Riq, *Advances in Format String Exploiting,* Phrack 59 #0x04.
- blexim, *Basic Integer Overflows,* Phrack 60 #0x10.

## Acknowledgements
This assignment is based in part on materials from Prof. Hovav Shacham at UC San Diego as well as Prof. Dan Boneh at Stanford. Thanks for their hard work.
