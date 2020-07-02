---
layout: post
title: "Golang에서 Chrome Webdriver + Selenium 사용하기"
tags: [go, golang, webdriver, seleium]
comments: true
---

#### Golang에서 Chrome Webdriver + Selenium 사용하기
<br>
Golang에 막 입문한 뉴비 고퍼 입니다.
주로 파이썬에서 셀레니움을 이용한 작업들을 진행해오다 Go에서도
구현 해보고자 이런저런 자료를 찾아보다가 꽤나 막혔었는데 방법을 찾았습니다.
<br>
- webdriver

https://github.com/fedesog/webdriver

Install:
```bash
$ go get github.com/fedesog/webdriver
```
<br>
- selenium for golang

https://github.com/tebeka/selenium

Install:
```bash
$ go get -t -d github.com/tebeka/selenium
```
<br>
적당한데 폴더 만드시고 프로젝트 생성 대략
```bash
$ mkdir go/src/selenium_test
```
요기에 크롬 드라이버를 다운 받아서 넣어 줍니다.
https://chromedriver.storage.googleapis.com/index.html?path=83.0.4103.39/
<br>
에디터에서 main.go 작성
```bash
$ atom main.go 또는 code main.go
```
소스 코드는 webdriver 샘플 소스 기반으로 셀레니움을 적당히 버무립니다.
```go
package main

import (
    "log"
    "time"

    "github.com/fedesog/webdriver"
    "github.com/tebeka/selenium"
)

func main() {
    chromeDriver := webdriver.NewChromeDriver("./chromedriver.exe")
    err := chromeDriver.Start()
    if err != nil {
        log.Println(err)
    }
    desired := webdriver.Capabilities{"Platform": "Windows"}
    required := webdriver.Capabilities{}
    session, err := chromeDriver.NewSession(desired, required)
    if err != nil {
        log.Println(err)
    }
    err = session.Url("http://golang.org")
    if err != nil {
        log.Println(err)
    }

    // 셀레니움 관련 제어 부분
    btn, _ := session.FindElement(selenium.ByCSSSelector, "#page > div > div.HomeContainer > section.HomeSection.Playground > div.Playground-controls > div > button")
    btn.Click()

    time.Sleep(60 * time.Second)
    session.Delete()
    chromeDriver.Stop()
}
```
<br>
꽤 잘 동작 합니다. 리눅스에서도 잘 되네요
파이썬 너무 좋지만 Golang도 넘 재밌습니다^^
