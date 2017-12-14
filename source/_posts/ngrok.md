---
title: ngrok
date: 2017-12-14 09:59:59
tags: [ngrok, server]
---
ngrok설치
https://ngrok.com/download
환경변수 등록

/a/b/ngrok/ngrok.exe면 /a/b/ngrok/를 시스템path에등록한다.
그러면 cmd에서 다 호출가능
가입하면 authentication 키 등록해서 로컬대시보드아닌 페이지 서버에서
ngrok 서버 대시보드 제공해주는거 사용가능(팀단위 유료계정사용시 유용)
일단 가입하면 대시 보드에 나타나는 authtoken으로 ngrok를 구성해야합니다.
이렇게하면 계정 전용 기능에 대한 액세스 권한이 부여됩니다.
ngrok는 이것을 쉽게하기위한 간단한 'authtoken'명령을 가지고 있습니다.

https://dashboard.ngrok.com/auth에서 본인 키 확인가능
ngrok authtoken 5pHDE11bm7qPj5dfw14Ae_7rhFPQXXTYW9QkdPYEJKh

whitelist access to your tunnel endpoints.를 설정할 수 있지만 Business plan으로 변경해야한다.
Business Plan 사용시 subdomain도 사용가능하다. Custom domain도 등록해서 사용가능하다.

생성한 터널의 접근을 비밀번호로 제한할 수 있다.
ngrok http -auth="username:password" 8080

서버띄우고 아래 주소입력시 모든 http req/res 모니터링할 수 있는 UI를 제공한다.
ngrok http -inspect=false 80 옵션 사용시 비활성화 가능
http://localhost:4040/inspect/http
http://localhost:4040/status
같은 네트워크가 아니라도 접근가능->패스워드지정

req/res body의 xml, json의 구문검사도 실시한다.

host header를 rewriting 해준다.
// Rewrite the Host header to 'site.dev'
ngrok http -host-header=rewrite site.dev:80
//Rewrite the Host header to 'example.com'
ngrok http -host-header=example.com 80

https만 사용가능 false시 http만
ngrok http -bind-tls=true site.dev:80

Forwarding to servers on a different machine (non-local services)
다른 IP로 포워딩가능
ngrok http 192.168.1.1:8080

Configuration 파일을 설정하면 터널옵션을 일일이 치지않고
설정파일에 지정된 터널명만 치면 된다.
tunnels:
    httpbin:
        proto: http
        addr: 8000
        subdomain: alan-httpbin
    website:
        addr: 8888
        auth: bob:bobpassword
        bind_tls: true
        host_header: "myapp.dev"
        inspect: false
        proto: http
        subdomain: myapp

ngrok start httpbin

로케이션설정
ngrok터널은 전세계 데이터 센터에서 실행된다.
기본리전은 us로 설정된다.
도메인 예약시 해당리전에 예약되므로 다른 리전에서 도메인에 바인드할 수 없다

api 서버로도 사용가능하다
ngrok Client API
The ngrok client exposes a REST API that grants programmatic access to:

Collect status and metrics information
Collect and replay captured requests
Start and stop tunnels dynamically
