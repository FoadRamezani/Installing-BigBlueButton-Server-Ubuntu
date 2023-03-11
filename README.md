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
and confirm you see LANG=en_US.UTF-8 in the output.<br /><br />
Next, check that your server has (at lest) 16G of memory using the command free -h. Here’s the output from one of
our test servers.<br />
`$ free -h`<br /><br />
Next, check that the server has Ubuntu is 18.04 as its operating system.<br />
`$ cat /etc/lsb-release`<br />
`DISTRIB_ID=Ubuntu`<br />
`DISTRIB_RELEASE=18.04`<br />
`DISTRIB_CODENAME=bionic`<br />
`DISTRIB_DESCRIPTION="Ubuntu 18.04.5 LTS"`<br /><br />
Next, check that your server is running the 64-bit version of Ubuntu 18.04.<br />
`$ uname -m`<br />
`x86_64`<br />
Next, check that your server supports IPV6.<br />
```
$ ip addr | grep inet6
inet6 ::1/128 scope host
inet6 fe80::f816:3eff:fe20:f0d8/64 scope link
inet6 fe80::42:edff:fec9:62f5/64 scope link
inet6 fe80::42:cfff:feb4:c517/64 scope link
inet6 fe80::46a:70ff:fef0:6ac6/64 scope link 
inet6 fe80::c4de:f2ff:fed5:651e/64 scope link
```
If you do not see the line inet6 ::1/128 scope host then after you install BigBlueButton you will need to modify the configuration for FreeSWITCH to
disable support for IPV6.<br /><br />
Next, check that your server is running Linux kernel 4.x.<br />
```
$ uname -r
4.15.0-147-generic
```

Note: BigBlueButton will not run on a 2.6 Kernel (such as Linux 2.6.32-042stab133.2 on x86_64 on OpenVZ VPS).<br /><br />
Next, check that your server has (at least) 8 CPU cores<br />
```
$ cat /proc/cpuinfo | awk '/^processor/{print $3}' | wc -l
Output >> 2
```
# 2 - Installing :
Running this command leads to install BigBlueButton with dependencies and required configurations.<br />
```
wget -qO- https://ubuntu.bigbluebutton.org/bbb-install.sh | bash -s -- -v bionic-23 -g -s bbb.example.com -e foadrm@gmail.com
```
Note : Prefer to do this command with root access , so using ** sudo -i  command for getting required privileges .<br />
You are able to check bbb-install.sh switches for usin :<br />
```
USAGE:
  bbb-install.sh [OPTIONS]
OPTIONS (install BigBlueButton):
      -v <version> Install given version of BigBlueButton (e.g. 'xenial-22') (required)
      -s <hostname> Configure server with <hostname>
      -e <email> Email for Let's Encrypt certbot
      -x Use Let's Encrypt certbot with manual dns challenges
      -a Install BBB API demos
      -g Install GreenLight
      -c <hostname>:<secret> Configure with coturn server at <hostname> using <secret>
      -p <host> Use apt-get proxy at <host>
      -r <host> Use alternative apt repository (such as packages-eu.bigbluebutton.org)
      -d Skip SSL certificates request (use provided certificates from mounted volume)
      -h Print help
```

After the bbb-install.sh script finishes, you can check the status of your server with # bbb-conf --check.
You must get some reports such as below :<br />
```
BigBlueButton Server 2.3.8 (2397)
        Kernel version: 4.15.0-147-generic
        Distribution: Ubuntu 18.04.5 LTS (64-bit)
        Memory: 4039 MB
        CPU cores: 2
/etc/bigbluebutton/bbb-web.properties (override for bbb-web)
/usr/share/bbb-web/WEB-INF/classes/bigbluebutton.properties (bbb-web)
        bigbluebutton.web.serverURL: https://bbb-test.diari.ir
        defaultGuestPolicy: ALWAYS_ACCEPT
        svgImagesRequired: true
/etc/nginx/sites-available/bigbluebutton (nginx)
        server_name: bbb-test.diari.ir
        port: 80, [::]:80
        port: 443 ssl
/opt/freeswitch/etc/freeswitch/vars.xml (FreeSWITCH)
        local_ip_v4: 185.194.78.46
        external_rtp_ip: 234.32.3.3185.194.78.46
        external_sip_ip: 185.194.78.46
/opt/freeswitch/etc/freeswitch/sip_profiles/external.xml (FreeSWITCH)
        ext-rtp-ip: $${local_ip_v4}$${external_rtp_ip}
        ext-sip-ip: $${external_sip_ip}$${local_ip_v4}
        ws-binding: 185.194.78.46:5066
        wss-binding: 185.194.78.46:7443
/usr/local/bigbluebutton/core/scripts/bigbluebutton.yml (record and playback)
        playback_host: bbb-test.diari.ir
        playback_protocol: https
        ffmpeg: 4.2.4-1ubuntu0.1bbb2~18.04
/etc/bigbluebutton/nginx/sip.nginx (sip.nginx)
        proxy_pass: 185.194.78.46
        protocol: https
/usr/local/bigbluebutton/bbb-webrtc-sfu/config/default.yml (Kurento SFU)
        kurento.ip: 185.194.78.46
        kurento.url: ws://127.0.0.1:8888/kurento
        kurento.sip_ip: 185.194.78.46
        localIpAddress: 185.194.78.46
        recordScreenSharing: true
        recordWebcams: true
        codec_video_main: VP8
        codec_video_content: VP8
/usr/share/meteor/bundle/programs/server/assets/app/config/settings.yml (HTML5 client)
        build: 1813
        kurentoUrl: wss://bbb-test.diari.ir/bbb-webrtc-sfu
        enableListenOnly: true
        sipjsHackViaWs: false
/usr/share/bbb-web/WEB-INF/classes/spring/turn-stun-servers.xml (STUN Server)
          stun: stun.freeswitch.org
```

```
# Potential problems described below
# Warning: The API demos are installed and accessible from:
#
# https://bbb-test.diari.ir
#
# and
#
# https://bbb-test.diari.ir/demo/demo1.jsp
#
# These API demos allow anyone to access your server without authentication
# to create/manage meetings and recordings. They are for testing purposes only.
# If you are running a production system, remove them by running:
#
# apt-get purge bbb-demo
# Warning: found only 2 cores, whereas this server should have (at least) 4 CPU cores
# to run BigBlueButton in production.
#
# https://docs.bigbluebutton.org/install/install.html#minimum-server-requirements
```
Any output that followed Potential problems may indicate configuration errors or installation errors.<br />
You can also use sudo bbb-conf --status to check that all the BigBlueButton processes have started and are running.<br />
```
$ sudo bbb-conf --status
nginx ————————————————— [ - active]
freeswitch ———————————— [ - active]
redis-server —————————— [ - active]
bbb-apps-akka ————————— [ - active]
bbb-fsesl-akka ———————— [ - active]
tomcat8 ——————————————— [ - active]
mongod ———————————————— [ - active]
bbb-html5 ————————————— [ - active]
bbb-webrtc-sfu ———————— [ - active]
kurento-media-server —— [ - active]
bbb-html5-backend@1 ——— [ - active]
bbb-html5-backend@2 ——— [ - active]
bbb-html5-frontend@1 —— [ - active]
bbb-html5-frontend@2 —— [ - active]
etherpad —————————————— [ - active]
bbb-web ——————————————— [ - active]
```
Also you can restart your server with this command :<br />
`$ sudo bbb-conf --restart`
after that we must config the DNS  , for this reason we must enter this command , 
sudo apt-g
After This steps


