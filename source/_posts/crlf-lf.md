---
title: 'crlf,lf'
date: 2017-12-15 21:56:45
tags:
---
익히 알다시피 Windows 에서는  line ending으로 CR(Carriage-Return, \r)과 LF(Line Feed, \n)을 사용하고 Unix 나 Mac OS 는 LF 만 사용한다.
이는 상당히 골치아픈 문제를 발생시킨다. 실제 코드는 변경된 게 없는데 소스의 CR/LF 때문에 변경으로 착각하여 commit 을 하게 될 수 있으며 변경 로그를 보거나 merge 마다 문제가 될 소지가 있다.
이런 문제를 방지하기 위해 OS 가 달라도 문제가 없도록 crlf 처리 방법을 결정해야 한다.

git 설정
CORE.EOF
git 이 line ending을 어떻게 처리하는지 관련된 항목이다. 세 가지 설정을 할 수 있다.
core.eol = native. 기본 설정. 시스템에서 line ending 을 처리하는 방법에 따른다. windows에서는 CRLF 를 사용하고 Linux, OS X 는 LF 만 사용한다.
core.eol = crlf CRLF 를 line ending 으로 사용한다.
core.eol = lf LF를 line ending 으로 사용한다.
설정은 다음 명령어로 수행할 수 있다.
## 설정
$  git config --global core.eol native

## 설정 확인
$ git config --global --list|grep core.eol

CORE.AUTOCRLF
git 은 저장소 메타 데이타 디렉터리인 .git 폴더에 모든 이력 데이타를 갖고 있다. 이력 데이타는 key/value 형식의 데이타베이스이며 core.autocrlf 는 text file 을 git object database 에 checkin, checkout 할 때 어떻게 처리할지를 설정하는 변수이다.
다음 세 가지 설정이 가능하다.
core.autocrlf = false. 기본 설정이다. 파일에 CRLF 를 썼든 LF 를 썼든 git 은 상관하지 않고 파일 그대로 checkin, checkout 한다. 이 설정은 line ending 이 다른 OS 에서는 text file 이 변경되었다고 나오므로 위에서 언급한 여러 가지 문제가 발생할 수 있다.
core.autocrlf = true text file을 object database 에 넣기전에 CRLF 를 LF 로 변경한다.
core.autocrlf = input LF를 line ending 으로 사용한다.

방법 #1 - autocrlf 설정 사용
OS 별 CRLF 차이로 인한 문제를 막기 위해 OS 별로 다음과 같이 crlf 처리 방법을 설정하는 걸 권장한다.
WINDOWS
윈도에서는 CRLF 를 사용하므로 저장소에서 가져올 때 LF 를 CRLF 로 변경하고 저장소로 보낼 때는 CRLF 를 LF 로 변경하도록 true 로 설정한다.
git config --global core.autocrlf true
LINUX, MAC OS
리눅스, 맥, 유닉스는 LF 만 사용하므로 input 으로 설정한다.
git config --global core.autocrlf input

https://www.lesstif.com/pages/viewpage.action?pageId=20776404
