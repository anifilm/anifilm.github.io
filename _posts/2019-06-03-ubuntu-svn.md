---
layout: post
title: "[Subversion] Ubuntu에 svn 설치하기"
tags: [ubuntu, svn]
comments: true
---

![이미지](https://subversion.apache.org/images/svn-name-banner.jpg){: .center-image}

아래 명령으로 우분투에 subversion을 설치한다.

```bash
$ sudo apt-get install subversion
```


설치가 완료되면 적당한 곳에 폴더를 만들고 repository를 생성한다.

```bash
$ sudo mkdir -p /home/svn
$ sudo svnadmin create /home/svn/repo
```


그룹을 만들고 권한 설정을 한다.

```bash
$ sudo groupadd svn
$ sudo chgrp svn /home/svn/
$ sudo chmod g+w /home/svn/
$ sudo usermod -a -G svn anifilm
```


svn 프로토콜(svn://)을 사용하기 위해서 계정과 비번 설정등을 한다.


먼저 `authz` 파일을 열어 계정과 사용 권한을 추가한다.

```bash
$ cd /home/svn/repo
$ sudo vim authz
```


먼저 경로에 대한 권한을 설정한다. `[/]` 일경우 repository의 전체 경로에 대해 권한을 부여한다.


계정과 권한은 `<계정명>=rw` 형식으로 추가한다. r을 읽기, w는 쓰기다. 둘다 권한을 주려면 rw로 하면 된다.

```bash
[/]
anifilm=rw
```


이렇게 추가하면 `anifilm` 이라는 계정은 해당 repository의 전체 경로에 대해 쓰기와 읽기 권한을 가진다.


이번엔 비번을 설정해 보자. 비번은 passwd 파일을 열어서 추가한다.

```bash
sudo vim passwd
```


`<계정명>=<비번>` 형식으로 작성하면 된다.

```bash
[users]
anifilm=1234
```


마지막으로 svn 설정을 한다. `svnserve.conf`열어서 아래 내용을 추가한다. 주석을 제거해 주면 된다.

```bash
[general]
anon-access = none
auth-access = write
password-db = passwd
authz-db = authz
```


설정한 `svnserve`를 적용한다.

```bash
$ sudo svnserve -d --foreground -r /home/svn/repo
```


서버가 재시작되거나 하면 수동으로 svn을 시작해야 하는 불편함이 있으므로 initialization script를 적용해 보자.


먼저 `/etc/init.d`로 가서 svnserve라는 파일을 하나 만든다.

```bash
$ cd /etc/init.d
$ sudo touch svnserve
$ sudo vim svnserve
```


[svnserve.txt](https://dejavuqa.tistory.com/attachment/cfile21.uf@9916BA3359AFB65F2F7B6E.txt) 파일을 다운받아 script 내용을 svnserve에 복사한다.


script 내용중 `DAEMON_ARGS="-d -r /home/svn"`에 해당하는 경로는 실제 경로로 조정한다.


svnserve파일이 작성되었으면 권한 설정을 한다. 그리고 서버 재시작할때 svn도 자동 실해하도록 설정한다.

```bash
$ sudo chmod +x /etc/init.d/svnserve
$ sudo update-rc.d svnserve defaults
```


이제 svn을 시작하거나 중지시킬때 아래 명령으로 사용할 수 있다.

```bash
$ sudo service svnserve start
```


svn 서버 상태 확인

```bash
$ sudo service svnserve status
```
