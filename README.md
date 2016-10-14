[linuxserverurl]: https://linuxserver.io
[forumurl]: https://forum.linuxserver.io
[ircurl]: https://www.linuxserver.io/irc/
[podcasturl]: https://www.linuxserver.io/podcast/

[![linuxserver.io](https://raw.githubusercontent.com/linuxserver/docker-templates/master/linuxserver.io/img/linuxserver_medium.png)][linuxserverurl]

The [LinuxServer.io][linuxserverurl] team brings you another container release featuring easy user mapping and community support. Find us for support at:
* [forum.linuxserver.io][forumurl]
* [IRC][ircurl] on freenode at `#linuxserver.io`
* [Podcast][podcasturl] covers everything to do with getting the most from your Linux Server plus a focus on all things Docker and containerisation!

# lsioarmhf/sabnzbd
[![](https://images.microbadger.com/badges/version/lsioarmhf/sabnzbd.svg)](https://microbadger.com/images/lsioarmhf/sabnzbd "Get your own version badge on microbadger.com")[![](https://images.microbadger.com/badges/image/lsioarmhf/sabnzbd.svg)](http://microbadger.com/images/lsioarmhf/sabnzbd "Get your own image badge on microbadger.com")[![Docker Pulls](https://img.shields.io/docker/pulls/lsioarmhf/sabnzbd.svg)][hub][![Docker Stars](https://img.shields.io/docker/stars/lsioarmhf/sabnzbd.svg)][hub][![Build Status](http://jenkins.linuxserver.io:8080/buildStatus/icon?job=Dockers/LinuxServer.io-armhf/lsioarmhf-sabnzbd)](http://jenkins.linuxserver.io:8080/job/Dockers/job/LinuxServer.io-armhf/job/lsioarmhf-sabnzbd/)
[hub]: https://hub.docker.com/r/lsioarmhf/sabnzbd/

SABnzbd makes Usenet as simple and streamlined as possible by automating everything we can. All you have to do is add an .nzb. SABnzbd takes over from there, where it will be automatically downloaded, verified, repaired, extracted and filed away with zero human interaction.

[![sabnzbd](https://raw.githubusercontent.com/linuxserver/docker-templates/master/linuxserver.io/img/sabnzbd-banner.png)][saburl]
[saburl]: http://sabnzbd.org/

## Usage

```
docker create --name=sabnzbd \
-v <path to data>:/config \
-v <path to downloads>:/downloads \
-v <path to incomplete downloads>:/incomplete-downloads \
-e PGID=<gid> -e PUID=<uid> \
-e TZ=<timezone> \
-p 8080:8080 -p 9090:9090 \
lsioarmhf/sabnzbd
```

**Parameters**

* `-p 8080` - http port for the webui
* `-p 9090` - https port for the webui *see note below*
* `-v /config` - local path for sabnzbd config files
* `-v /downloads` local path for finished downloads
* `-v /incomplete-downloads` local path for incomplete-downloads - *optional*
* `-e PGID` for GroupID - see below for explanation
* `-e PUID` for UserID - see below for explanation
* `-e TZ` for setting timezone information, eg Europe/London

It is based on ubuntu xenial with s6 overlay, for shell access whilst the container is running do `docker exec -it sabnzbd /bin/bash`.

### User / Group Identifiers

Sometimes when using data volumes (`-v` flags) permissions issues can arise between the host OS and the container. We avoid this issue by allowing you to specify the user `PUID` and group `PGID`. Ensure the data volume directory on the host is owned by the same user you specify and it will "just work" ™.

In this instance `PUID=1001` and `PGID=1001`. To find yours use `id user` as below:

```
  $ id <dockeruser>
    uid=1001(dockeruser) gid=1001(dockergroup) groups=1001(dockergroup)
```

## Setting up the application 
`IMPORTANT... THIS IS THE ARMHF VERSION`
Initial setup is done from the http port.
Https access for sabnzbd needs to be enabled in either the intial setup wizard or in the configure settings of the webui, be sure to use 9090 as port for https.
See here for info on some of the switch settings for sabnzbd http://wiki.sabnzbd.org/configure-switches


## Info

* Shell access whilst the container is running: `docker exec -it sabnzbd /bin/bash`
* To monitor the logs of the container in realtime: `docker logs -f sabnzbd`

* container version number 

`docker inspect -f '{{ index .Config.Labels "build_version" }}' sabnzbd`

* image version number

`docker inspect -f '{{ index .Config.Labels "build_version" }}' lsioarmhf/sabnzbd`

## Versions

+ **14-10-16:** Add version layer information.
+ **13.09.16:** Intial Release. 
