---
layout: post
title: Docker on Windows 설치기(레거시 노트북)
tags:
  - docker
  - setup
  - install
  - developer
---

> *systeminfo 또는 Coreinfo 명령어로 SLAT 사용가능 한지 체크해야 한다.*


## 프롤로그
> 항상 구석탱이에 있는 마음한켠에 있는 노트북을 꺼내들었다. UP 좀 해줄려고. 여전히 프로젝트는 수배중이고 시간은 많다_마음속의 시간은 없지만.

>2년전에 전자관에 가서 문의한 답변이 생각이 났다. "되기는 되는데예... 펑션키가 작동 안할 수도 있었예". SSD하고 win 10 작업을 문의했는 답변이다.
그냥 작업의뢰 안하고 그대로 들고 왔고 노트북은 그 길로 계속 쳐박혀 있었다.


![2009 Notebook](https://github.com/uphoon/uphoon.github.io/releases/download/20210314/img01_notebook.jpg "2009 Notebook")

>잠깐의 시간을 들여 SSD하고 메모리 업그레이드하고 windows 10까지 설치했다.


## 요약
BIOS에서 <u>SLAT(Second Level Address Translation)</u> 가능여부까지 확인하고 <u>WSL2</u> 나 <u>>Hyper-V</u> 환경에서 도커 설치여부 판단해야 합니다.

## 설치환경
아래 3가지 환경에서 Docker on Windows를 설치할 수 있습니다.

* WSL2(권장) : Windows 10
* Hyper-V : Windows 10
* Docker ToolBox : 레거시 플랫폼(Windows 7...)

(참조) https://docs.docker.com/docker-for-windows/troubleshoot/#virtualization

## Check VIRTUALIZATION MUST BE ENABLED

작업관리자 > 성능(Tab) > 가상화 : 사용 을 확인하고 설치 하였습니다.

![Task Manager](https://github.com/uphoon/uphoon.github.io/releases/download/20210314/img02_taskmanager.png "Task Manager")


## Docker Start
> WSL2로 환경으로 특별한 이슈없이 설치했다. 근데 스타트 중에 에러창이 뜨고, 테스트탑 Toast 창에는 계속 정상 Starting... 메세지가 살아있다.
  에러창 링크 정보의 가이드대로 했지만 계속 똑같은 현상만 나타났다. 원인을 몰랐다. 결국 윈도우 > 관리 > 이벤트 뷰어 를 확인하고 원인을 알았다.


![Event View error](https://github.com/uphoon/uphoon.github.io/releases/download/20210314/img03_eventerror.png "Event View error")

## 해결
<u>SLAT 지원되지 않아서</u> 입니다. 결국 **Docker ToolBox 환경으로 도커를 설치**했습니다. 
터미널 창에 docker 여러 명령어를 날리고 보니 내가 왜 "꼭 WSL2로 설치하고 말테야" 라고 생각했는지 모르겠습니다.

SLAT(Second Level Address Translation) 확인은 아래 명령어로 확인할 수 있습니다.

~~~ 
$ systeminfo
~~~
![systeminfo_Result](https://github.com/uphoon/uphoon.github.io/releases/download/20210314/img05_systeminfo.png "systeminfo Result")

or

~~~ 
$ Coreinfo.exe -v
~~~
![Coreinfo_Result](https://github.com/uphoon/uphoon.github.io/releases/download/20210314/img04_coreinfo.png "Coreinfo Result")


Codeinfo 실행은 아래 링크에서 다운받아야 합니다.

* [https://docs.microsoft.com/ko-kr/sysinternals/downloads/coreinfo](https://docs.microsoft.com/ko-kr/sysinternals/downloads/coreinfo)



## 에필로그
> 분명 어제보다 나아을꺼야...

인프라 관리자도 아니고 ... 중요한건 도커 명령어인데... 내 범위는 결국 이미지 잘 설계하고 빌드 스크립트 잘 만드는 일인데...
