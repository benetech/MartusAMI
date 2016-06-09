# MartusAMI

* The best way to create an AMI is for me to make a full backup as an AMI, then zero out the keypair, then let the volunteers delete all the server-specific Martus data to create an empty server image (I may already have documented how to do this; if I haven't, I can do it).

* One of the most important security measures needed when creating Martus Server releases or managing servers is to only login to server (resources) from a highly-secure workstation.  It's doubtful the volunteers' workstations qualify (unless they operate from bootable CDs or memory sticks, and they carefully checked the signature of the .iso/whatever used to create the media).

* So, we probably can't directly use an AMI they create.  But we can use the knowledge they create.  The ideal would be for them to document the process (which especially involves editing/augmenting my docs as they go along; I'd review/edit then checkin the changes).  A bonus feature would be for them to do all their work after running 'script', so we have a history of what they did (this won't capture edits, unless they 'diff' the modified file against the original file after each edit).

* The best way to facilitate this is to configure their editor to backup each file before saving.

  AMI ID: ami-f421da94
  AMI Name: martus-server-backup-160607
  Name: 2016-06-07 backup
  

* To get started, the volunteers should create an instance from the AMI, boot it, and change the root passwd (which is currently the volunteers' AWS account's ID).  'sudo' is not used (too many security vulnerabilities over the years), you have to use 'su' and get a shell.
 
* They should then follow item A.21) in /usr/local/martus/doc/MartusServerFAQ.txt to rebuild the AMI, perhaps after perusing the whole A) section.
  
  
* I just realized that the tarball in /usr/local/martus/src/ is missing the directory that contains documentation on upgrading the jar and the java, and sysadmin scripts to manage/diagnose the server. Perhaps at some point they should enable the 'scott' account on their instance (my public key is stored in /etc/martus/pki/), and I can replace the tarball.


* I forgot to mention, then easiest way to do this is: 'faq a.21'
* If you just run 'faq' you'll see a bunch of examples.  [I need to modify the Usage message to tell the user that multiple keywords are ANDed, eg: faq clone ami]

* As I mentioned earlier, my backup AMI includes the full 'packets' volume (which holds all the bulletins), the largest volume.  And the procedure in A.21 says to just mkfs this volume, i.e. wipe out its data.

* Instead, when the backup AMI is created, the 'packets' volume should not be included (item A.2 describes how to leave out volumes when creating a backup AMI), and then at installation time the appropriate EBS volume should be created and attached to the instance and a filesystem should be built on it (the mkfs and tunefs commands for this are included in A.21).  And, of course, this procedure needs to be documented a little more carefully than this stream of consciousness paragraph :->

* FYI, I just stumbled across sysadmin/cloud-notes-AWS.txt (which was left out of the src tarball).  It has incredibly cryptic notes (one big pseudo shell script) on creating an AMI from scratch.

* And it has cryptic notes on creating an empty AMI to launch a new SL1 host, and how to launch a new SL1 host.  It had a few commands that were left out of A.21:

rm /etc/ssh/*key
rm -rf /var/local/martus/MartusServer/cache

* I updated A.21 to reflect the above. If the volunteers let me log in to their instance (I think I've documented how to do this), I could update stuff.



