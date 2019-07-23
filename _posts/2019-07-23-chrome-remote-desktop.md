---
layout: post
title: "Ubuntu에서 크롬 원격 데스크톱 사용하기"
tags: [ubuntu, chrome, remote]
comments: true
---

[원본:Ubuntu에서 크롬 원격 데스크톱 사용하기-1](http://hdh7485.blog.me/221444127526)

[원본:Ubuntu에서 크롬 원격 데스크톱 사용하기-2](http://hdh7485.blog.me/221444142342)


이전 포스트에서 `ubuntu`에서 `chrome remote desktop` 설치 방법에 대해 알아보았습니다. 이제 원래 우분투 화면 그대로 제어하는 방법을 알아보겠습니다. 크롬 원격 데스크톱에서 원래 화면 그대로 원격 제어하는 방법은 코드를 수정해야합니다. 팀뷰어와 같이 다른 원격 제어 프로그램보다 귀찮지만 크롬 원격 제어 앱이 팀뷰어 보다 낫다고 생각해서 저는 `chrome remote destktop`을 사용하고 있습니다.


1. 사용자의 계정을 `chrome-remote-desktop` 그룹에 추가한다.

```bash
sudo usermod -a -G chrome-remote-desktop 계정이름
```


2. 실행중인 `크롬 원격 데스크톱(chrome remote desktop)`을 중지한다.

```bash
/opt/google/chrome-remote-desktop/chrome-remote-desktop --stop
```


3. 기존의 `chrome remote desktop` 설정 파일을 백업해 놓는다. 혹시 잘못되면 백업한 파일을 사용하면 된다.

```bash
sudo cp /opt/google/chrome-remote-desktop/chrome-remote-desktop /opt/google/chrome-remote-desktop/chrome-remote-desktop.orig
```


4. 원하는 편집 툴(gksudo gedit, sudo vim, sudo nano 등등)을 사용해서 `/opt/google/chrome-remote-desktop/chrome-remote-desktop` 파일을 연다.

```bash
sudo vi /opt/google/chrome-remote-desktop/chrome-remote-desktop
```


5. 현재 디스플레이 숫자를 `FRIST_X_DISPLAY_NUMBER`에 넣어준다. 보통 `18.04` 이하는 0 이다. 터미널에서 `echo $DISPLAY`명령어를 통해 숫자를 확인 할 수 있다.

```python
FIRST_X_DISPLAY_NUMBER = 0
```


6. 다음 코드를 찾아 주석 처리 한다. 주석 처리는 문장 앞에 #을 추가하면 된다.

```python
    #while os.path.exists(X_LOCK_FILE_TEMPLATE % display):
    #  display += 1
```


7. `launch_session` 함수를 찾아 수정한다. `_launch_x_server()`와 `_launch_x_session()`을 주석처리 해서 새로운 display가 생성되지 않게 한다. 그리고 두 줄의 코드를 추가하여 기존의 디스플레이를 사용한다. 이제 파일을 저장하고 편집 툴을 종료한다.

```python
def launch_session(self, x_args):
    self._init_child_env()
    self._setup_pulseaudio()
    self._setup_gnubby()
    #self._launch_x_server(x_args)
    #self._launch_x_session()
    display = self.get_unused_display_number()
    self.child_env["DISPLAY"] = ":%d" % display
```


8. `chrome-remote-desktop`을 다시 실행시킨다. 

```bash
/opt/google/chrome-remote-desktop/chrome-remote-desktop --start
```


다시 다른 기기에서 우분투를 원격 접속 해보시면 보이는 화면 그대로가 원격제어 될 것입니다. 잘 안되시면 자료 출처 링크를 들어가서 확인해보시거나 물어봐주세요.

자료 출처: https://superuser.com/questions/778028/configuring-chrome-remote-desktop-with-ubuntu-gnome-14-04