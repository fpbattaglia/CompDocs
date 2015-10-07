CLUSTER CONFIGURATION AND ACCESS
================================

We will access the cluster in two ways. The cluster cannot be accessed from outside the RU science network directly, so you need a workaround.
The best way is via the science faculty login server, lilo.science.ru.nl. 
For this you need a Science account. 
The mosh/byobu method is still preferable though 

Access via lilo
---------------

The idea is to ssh from your computer to lilo, and from lilo to the frontend cluster node
patron.science.ru.nl. For the cluster, you need a separate account that you need to ask to the Science sysadmin (Ben Polman). 

* (optional but strongly advised) Install  (http://mosh.mit.edu "MOSH"). Mosh is a replacement for ssh, its greatest benefit is that it will maintain state even as you lose the network connection of you have a flaky network connection. There are installation instruction for most platforms on the website. You can also install that in Windows via the Cygwin package (although you will still need puTTY or another ssh clone to do sftp__. Once you have that, you can login form a terminal window with 

> mosh your_science_username@lilo.science.ru.nl 

* on lilo, you can use byobu, which is a session manager that allows you to have multiple shells at the same time, split the screen and do other cool stuff. Byobu is already installed on lilo, so you just have to call it
> byobu

   you can also do 
   
> byobu-enable 

   so that next time you connect to lilo, it will get you directly into byobu. 
   There's plenty of information about the many byobu shortcuts and commands at (http://byobu.co). 
   
   NOTE: on the Mac, the function keys are remapped to a lot of weird stuff, which interfere with byobu. to alleviate this, go to Systems Preferences->Keyboard and check the box "Use all F1, F2, etc.  keys as standard function keys". I got the best (not perfect) results by using iTerm2 instead of the standard Terminal Mac app, and do 
> TERM=vt100 mosh your_science_username@lilo.science.ru.nl 

   To avoid you some typing you can add an alias to your bash shell: just say
> echo "alias slilo='TERM=vt100 mosh your_science_username@lilo.science.ru.nl'" >> ~/.profile

   from your computer terminal. Then say 
> . ~/.profile

   From now on, you can log in with 
>  slilo

* (optional but strongly advised) setup ssh private keys as explained for example here: (http://www.linuxproblem.org/art_9.html). This allows you to login in with mosh/ssh without having to type a password each time. Do this in order to be able to log in from your computer to lilo *and* from lilo to patron. 

* From lilo, you can login into patron with 
> ssh your_patron_username@patron.science.ru.nl

   you are now in the cluster. 
   
* Even if you are inside the Science network, this method is still beneficial, as the combination of mosh and byobu will keep the state of your session, so that you can interrupt work and resume it from where you left it. 

* ~~I have added to patron some code that may be of interest:~~

	~~* EPD python in /peones/peon001/battaglia/epd-7.3-1-rh5-x86_64/bin~~
	~~* KlustaKwik in /peones/peon001/battaglia/bin~~
	~~* git in /peones/peon001/battaglia/bin~~
	~~* Anaconda python in /peones/peon001/battaglia/anaconda/bin/python~~

* I updated the code on the cluster, for several reasons, first, the /peones disks get wides and so it's not reliable to store software there, second, in the meantime the London group moved to phy, third, the OS on the cluster is ancient, so the binaries from klustateam won't work, and I had to recompile them, fourth, I don't think it makes sense to use EPD python any more as phy uses anaconda, which is much simpler and better behaved. Therefore, now I have 
	* anaconda3 in /home/battaglia/anaconda3 (add /home/battaglia/anaconda3/bin to PATH to use)
	* git in /home/battaglia/git
	* phy is installed in the anaconda distribution so it will be also available in /home/battaglia/anaconda3/bin 
	
	
	
	you can add these to the PATH to use them 