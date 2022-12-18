# Installing-BigBlueButton-Server-Ubuntu
Installing BigBlueButton Server Ubuntu Specially on 18.04

**1 - Minimum server requirements :**<br />
For production, we recommend the following minimum requirements<br />
* Ubuntu 18.04 64-bit OS running Linux kernel 4.x <br />
* Latest version of docker installed<br />
* 16 GB of memory with swap enabled<br />
* 8 CPU cores, with high single-thread performance<br />
* 500 GB of free disk space (or more) for recordings, or 50GB if session recording is disabled on the server.<br />
* TCP ports 80 and 443 are accessible<br />
* UDP ports 16384 - 32768 are accessible<br />
* 250 Mbits/sec bandwidth (symmetrical) or more<br />
* TCP port 80 and 443 are not in use by another web application or reverse proxy<br />
* A hostname (such as bbb.example.com) for setup of a SSL certificate<br />
* IPV4 and IPV6 addresv<br /><br />
**2 - Pre-installation checks :**<br />
First, check that the locale of the server is en_US.UTF-8 <br />
`$ cat /etc/default/locale`
`LANG="en_US.UTF-8"`<br /><br />
If you don’t see LANG="en_US.UTF-8", enter the following commands to set the local to en_US.UTF-8.<br />
`$ sudo apt-get install -y language-pack-en`<br />
`$ sudo update-locale LANG=en_US.UTF-8`<br /><br />
Then logout and login again to your SSH session – this will reload the locale configuration for your session. Run the above command `cat /etc
/default/locale` again. Verify you see only the single line LANG="en_US.UTF-8".<br /><br />
Note: If you see an additional line **LC_ALL=en_US.UTF-8**, then remove the entry for LC_ALL from **/etc/default/locale** and logout and then log
back in once more.<br /><br />
Next, do **sudo systemctl show-environment** and ensure you see **LANG=en_US.UTF-8** in the output.<br />
`$ sudo systemctl show-environment`<br />
`LANG=en_US.UTF-8`<br />
`PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin`<br />
If you don’t see this, do sudo systemctl set-environment LANG=en_US.UTF-8 and run the above sudo systemctl show-environment again
and confirm you see LANG=en_US.UTF-8 in the output.<br />
