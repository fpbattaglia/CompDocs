CONFIGURING NAS (Sinology) ACCESS
=================================


we have a Sinology NAS with 20TB storage. To access it from the cluster, it is good to have SFTP access. 

instructions for this may be found here (http://techanic.net/2014/04/12/configuring_ssh_and_scp_sftp_on_dsm_5.0_for_synology_diskstations.html) 

However, at the moment, it is not possible to do this from patron (probably because of firewall settings). I'm asking Ben Polman to help with that. 
*UPDATED* this has been fixed by Ben so this is now possible. 
There are different ways to access data back and forth to the NAS. Here's an overview of the possibilities, given the current configuration. 


Using rsync
-----------

Rsync is an utility that will synchronize two folders between two directories located between two different computers. 
It will only copy files that are different (or have been updated) and will leave the files that are unchanged alone. 
In this way, rsync will actually save quite a bit of bandwidth, and transfer time, so it may be the best way to work about. First let's see the basic usage, and then how rsync can be used in an analysis script

Preparation
-----------

Rsync makes use of ssh as its transfer protocol. That means that the authentication is handled by ssh.
In order to use rsync in a batch, we have to use the public key mechanism, that we described in cluster_conf.md 
For this, you need to take your *patron* public key, and you need to copy it into the authorized_keys file on tompouce.
See cluster_conf.md for the details. 
After you've done that, you should be able *from patron* to do `ssh your_tompouce_username@tompouce.science.ru.nl` and you should be able to get shell access into tompouce without entering a password. If that's the case, you are all set authentication-wise.
In addition, you need to set up the Sinology NAS so that rsync is enabled. 
From the webinterface at http://tompouce.science.ru.nl:5000 go to the "Backup&Replication" section, and from there to "Backup services". Check "Enable network backup services", and click "Apply". 

Note, I've done this for you, so it should be not needed again for the time being. If the NAS software gets reinstalled, or the settings get somehow lost, you'll have to repeat this step.

Basic usage
-----------

Suppose that you have a folder on the NAS named testrsync. You want to make a copy of it on the cluster. From patron (or importantly, the peones), type `rsync -avz -e ssh  fpbatta@tompouce.science.ru.nl:/volume1/homes/fpbatta/testrsync ./testrsync` 
you will get as output something like this 
```
receiving file list ... done
created directory ./testrsync
testrsync/
testrsync/file1
testrsync/file2

sent 70 bytes  received 310 bytes  253.33 bytes/sec
total size is 110  speedup is 0.29
```

Now suppose that you add a file in the directory (file3) and now you want to archive on the NAS the current state of the directory.  
You do `rsync -avz -e ssh ./testrsync/  fpbatta@tompouce.science.ru.nl:/volume1/homes/fpbatta/testrsync`

and you get 
```
building file list ... done
./
file3

sent 207 bytes  received 48 bytes  510.00 bytes/sec
total size is 148  speedup is 0.58
```

Note that file1 and file2 *are not* transferred back because they haven't changed.