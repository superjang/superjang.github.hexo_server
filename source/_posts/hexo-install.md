---
title: hexo install
date: 2017-12-14 17:10:49
tags: [hexo]
---
몇년간 호스팅 업체를 통해 블로그를 운영하고 있다. 1년 단위로 결제하는데 월 단위로 보면 큰돈이 아니지만 요즘같이 무료 호스팅이 많고 휴대폰으로도 서버를 돌리는 시대에 아깝다는 생각이 자꾸만 든다. 그래서 이사를 가야지 생각만 1년넘게 하다가  github page를 알게되었고 정적페이지 생성을 도와주는 서비스중 hexo라는 서비를 새로운 블로그 플랫폼으로 사용하기로 했다.
https://pages.github.com/
기존 블로그가 워드프레스인데 xml파일로 포스트 추출하여 hexo 마이그레이션이 된다고 하니 가능하면 옮기고 아니면 낡은 컨텐츠는 버리려고 한다. 달려있는 댓글들이 아깝지만.. ㅜㅜ 영원히 돈을 낼 순 없으니.. 빨리빨리 이사를!
{% youtube 2MsN8gpT6jY %}
Hexo?
정적 파일을 생성(Markdown -> HTML)해주는 블로그 프레임워크이다.

설치
노드설치
git 설치
hexo 설치
github 저장소 생성
    포스트 관리용
    호스팅 서버용

```
npm install -g hexo-cli
```
설정
init 명령으로 블로그를 운영할 폴더를 생성한다.
hexo init 블로그명 (포스트 관리용 저장소 이름하고 같이하자)

cd 블로그 명으로 블로그 폴더로 진입하여 패키지 파일을 설치한다.
cd 블로그명
npm install
npm install hexo-deployer-git --save
손쉬운 배포를 위해 위 모듈을 설치해 준다.

설치가 완료되면 hexo 기본 디렉토리 구조를 볼 수 있을 것이다.
_config.yml은 환경설정파일 (각 테마도 설정 파일인 _config.yml을 가진다)
자세한 내용은 https://hexo.io/ko/docs/configuration.html 참조
scaffold hexo 포스팅용 템플릿 md 파일이 위치한다.
source는 작성되는 포스트들이 위치한다 (_로 시작하는 파일과 숨김파일은 hexo에서 무시하고 해당 디렉토리 컨텐츠를 정적파일로 생성한다. hexo generate명령어를 통해 작성된 md파일을 정적파일로 생성하여 public 디렉토리에 위치시킨다.)
themes는 hexo 테마들이 있는 디렉토리

사용하기
hexo 디렉토리에서
hexo new post 포스트제목
post는 scaffold 디렉토리 내에있는 템플릿중 사용할 템플릿의 파일명을 사용한다.
포스트 제목은 띄워쓰기가 필요할 경우 ""로 묶어준다 그럼 공백은 -로 치환되어 포스트제목 기준으로 source디렉토리에 포스트 md파일이 생성된다.
생성된 md파일에 내용을 입력하고
hexo generate를 사용하면 public디렉토리가 생성되고
source 디렉토리의 포스팅 파일이 public 디렉토리에 html 형태로 생성되어 위치한다.
hexo server 명령어를 입력해 보면 localhost:4000으로 로컬 환경에서 블로그 포스팅을 미리 확인해 볼 수 있다.
이상이 없으면 배포를 한다.
hexo deploy 명령어를 입력하면 _config.yml에서 설정한 깃헙저장소(호스팅서버용 저장소)로 배포된다.
배포관련 설정은 https://hexo.io/ko/docs/deployment.html 참조한다.
종종 배포가 정상적으로 되어도 반영이 안될 경우가 있다(로컬, 리모트 둘다 발생하는 것 같다. 아무래도 정작파일이니 캐시되어 그런 것 같다.) 그럴경우
hexo clean으로 public 디렉토리를 제거하고 hexo generate로 다싱 정적파일을 생선하여 배포해본다.

그외 자세한 명령어는
https://hexo.io/ko/docs/commands.html 참조한다.

hexo 문서는 https://hexo.io/ko/docs/ 에 아주 잘 정리되어 있다.
풍부한 마크다운 사용하기, RSS 사용하기, 커스텀 도메인 적용하기
https://juhojuho.github.io/2017/03/27/hexo-tip/

asset 활성화 설정
포스트명과 동일한 asset디렉토리 생성됨 방법정리

syntax highlight 적용
https://github.com/ele828/hexo-prism-plugin

hexo 템플릿 문법
http://futurecreator.github.io/2016/06/19/hexo-tag-plugins/

카테고리, 태그 만들기
hexo new page "categories"
hexo new page "tags"

source/categories
source/tags 페이지 안에 index.md 템플릿 파일 상단에

type: "categories"
type: "tags" 를 추가해 준다.

그리고 각 post 타입 작성시 아래와 같이 작성하고 발행하면 테그와 카테고리 페이지에서 따로 리스팅되어 볼 수 있다.
---
title: test~!!
cover: images/cover_main.jpg
tags: [es6, javascript]
categories: Tech
---
여러개의 카테고리는
categories:
    - a
    - b

http://futurecreator.github.io/2016/06/21/hexo-basic-usage/


https://github.com/klugjo/hexo-theme-clean-blog
language
https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes
timezone
https://en.wikipedia.org/wiki/List_of_tz_database_time_zones


```java
// test.java
String s = "abc";
System.out.println(s);
```

{% post_link merge-tool-set  "머지툴 링크" %}


블라블라 `asdfas` 하하d
{% asset_path asset/images/cover_main.jpg %}
{% asset_img asset/images/cover_main.jpg [title] %}
{% asset_link slug [title] %}

hexo generate -d 생성->배포
