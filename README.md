[linuxserverurl]: https://linuxserver.io
[forumurl]: https://forum.linuxserver.io
[ircurl]: https://www.linuxserver.io/irc/
[podcasturl]: https://www.linuxserver.io/podcast/
[appurl]: http://sabnzbd.org/
[hub]: https://hub.docker.com/r/lsioarmhf/sabnzbd/

[![linuxserver.io](https://raw.githubusercontent.com/linuxserver/docker-templates/master/linuxserver.io/img/linuxserver_medium.png)][linuxserverurl]

The [LinuxServer.io][linuxserverurl] team brings you another container release featuring easy user mapping and community support. Find us for support at:
* [forum.linuxserver.io][forumurl]
* [IRC][ircurl] on freenode at `#linuxserver.io`
* [Podcast][podcasturl] covers everything to do with getting the most from your Linux Server plus a focus on all things Docker and containerisation!

# lsioarmhf/sabnzbd
[![](https://images.microbadger.com/badges/version/lsioarmhf/sabnzbd.svg)](https://microbadger.com/images/lsioarmhf/sabnzbd "Get your own version badge on microbadger.com")[![](https://images.microbadger.com/badges/image/lsioarmhf/sabnzbd.svg)](https://microbadger.com/images/lsioarmhf/sabnzbd "Get your own image badge on microbadger.com")[![Docker Pulls](https://img.shields.io/docker/pulls/lsioarmhf/sabnzbd.svg)][hub][![Docker Stars](https://img.shields.io/docker/stars/lsioarmhf/sabnzbd.svg)][hub][![Build Status](https://ci.linuxserver.io/buildStatus/icon?job=Docker-Builders/armhf/armhf-sabnzbd)](https://ci.linuxserver.io/job/Docker-Builders/job/armhf/job/armhf-sabnzbd/)

SABnzbd makes Usenet as simple and streamlined as possible by automating everything we can. All you have to do is add an .nzb. SABnzbd takes over from there, where it will be automatically downloaded, verified, repaired, extracted and filed away with zero human interaction.

[![sabnzbd](https://raw.githubusercontent.com/linuxserver/docker-templates/master/linuxserver.io/img/sabnzbd-banner.png)][appurl]

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

## Tags

You can choose ,using a tag, to use the unstable branch.
Stable is the default and requires no tag

(at times both branches may have the same version, but the stable/latest branch should never contain an alpha/beta/rc version)

Add the tag, if required, to the lsioarmhf/sabnzbd line of the run/create command in the following format:-

`lsioarmhf/sabnzbd:unstable`

## Parameters

`The parameters are split into two halves, separated by a colon, the left hand side representing the host and the right the container side. 
For example with a port -p external:internal - what this shows is the port mapping from internal to external of the container.
So -p 8080:80 would expose port 80 from inside the container to be accessible from the host's IP on port 8080
http://192.168.x.x:8080 would show you what's running INSIDE the container on port 80.`


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

+ **10.04.17:** Bump to 2.0 Release.
+ **07.03.17:** Add multithread par2, use unstable branch for alpha/betas etc..
+ **08.02.17:** Add pythonioenconding=utf8 as env.
+ **14.10.16:** Add version layer information.
+ **13.09.16:** Intial Release. 
