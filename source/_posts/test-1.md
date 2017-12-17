---
title: test~!!
cover: images/cover_main.jpg
date: 2017-12-15 23:50:47
tags: [postmortem]
categories: Tech
---
### 스마일배송 프로젝트진행간 이슈
- css reset의 부재로 옥션/지마켓 양 사이트간 UI가 동일함에도 css작업을 다시 해줘야 했음
    - css 작업을 다시하는 과정에서 일괄 처리를 위해 태그 자체에 스타일을 주는 경우가 여럿 발생
    - 몇몇 UI들이 사라지거나 줄간격이 폰트사이즈 기준으로 처리되는 UI들에서 이슈 발생.
    - 이를 잡기위해 다시 개별 UI들에 스타일 오버라이딩.
    - 같은 사이트내에서도 유사한 UI가 잇어 복사하여 사용하는데 코어와/코너(스마일배송)이라 css를 가져도 붙여도 디폴트 상속되는 css의 리셋이 다르거나 없어 제대로 노출되지 않음. 문제가 생기는 UI들의 스타일을 다시 수정, 결국 같은 UI지만 스타일은 점점 달라짐.
    - 결국 사이트별로도 달라지고 사이트 내에서도 스타일이 달라져 디자인은 같지만 여러벌의 스타일이 나오게됨.
- 디자이너들이 개발이슈 임에도 퍼블리싱 이슈로 지라 등록함. 프로세스 가이드 필요.
    - 개발페이지에서 문제 발생시 목업페이지 기준으로 지라생성 가이드.
- 목업 QA기간 명시적으로 정의, 완료 후 개발 전달.
- 디자이너 QA 지라 작성 가이드 전달
    - 지라 등록시 dweb/mweb 이슈 확인한 OS환경과 브라우저 정보가 없는 경우가 많음
    - 최소 정보 가이드 필요(OS, OS버전, 브라우저 종류, 브라우저 버전)
    - 개발화면 경우 확인한 URL 첨부 요청
    - 목업경우 확인한 목업 URL 첨부 요청
- 제플린 사용간 불편했던 점들 취합하여 전달

<!-- 디자인 QA시에 옥션/지마켓 각각 스마일배송, 코어쪽의 UI 총 4벌을 동시에 줄간격 같은걸 수정해달라고함 똑같이 수정했지만 상속되는 font-size, margin, padding등 여러 값들로 인해 제각기 다르게 노출되어 개별 처리로 4번 작업을함
추가로 크로스 브라우징 이슈로 하위 브라우저를 맞추기 위해 작업을 하는데 4개의 UI대해 각각 개별 수정을 해줘서 최소 8번의 작업을 하게됨.
이런 패턴이 계속 반복되고 태그 선택자로 작업자간 스타일이 침범하는 경우가 생기게되고 이런 패턴이 반복되고 결국 우선순위가 더 높은 선택자를 쓰거나 !important같은 땜빵식 코드가 들어가게됨.
사용하지 않는 코드들이 생기게되고 css최적화시 땜빵과 원래 코드가 상송관계라 레거시 제거 불가.
-->


### HTML / CSS
- OS, Browser 구분없이 body태그에 서비스 id, 서비스 페이지 class 사용
> - CSS 작성시 페이지 단위 스타일 적용 편리
> - 타 서비스에 대한 영향도를 낮출 수 있다

- mweb OS분기를 위한 서비스 공통 룰을 정하여 관리(body태그 클래스로 처리 ios, android)
> - Android, IOS UI이슈 발생시 분기하여 처리할 수 있는 환경 제공

    ```html
    <!-- Service, Page, OS 식별자 사용 예 -->

    <!-- Android 홈쇼핑 VIP 페이지 경우 -->
    <body id="app--homeshopping" class="os--android page--vip">
    <!-- IOS 홈쇼핑 코너 베스트 페이지 경우 -->
    <body id="app--homeshopping" class="os--ios page--corner__best">
    ```

- dweb 경우 IE분기를 위한 서비스 공통 룰을 정하여 주석 관리(버전별 body태그 클래스명 픽스 .lt-ie9, .ie8, .ie7)
> - CSS 핵 사용 자제로 벨리데이션 이슈 없이 처리
> - 버전별 처리가 명확하여 수정시 영향성 고려하지 않아도 됨

  ```html
  <!DOCTYPE html>
  <!--[if IE 7]>         <html class="no-js lt-ie9 ie7"> <![endif]-->
  <!--[if IE 8]>         <html class="no-js lt-ie9 ie8"> <![endif]-->
  <!--[if gt IE 8]><!--> <html class="no-js"> <!--<![endif]-->
    <head></head>
    <body id="app--homeshopping" class="os--android page--vip">
        <!-- contents area -->
    </body>
  </html>
  ```

- 모든 서비스 공통 스타일 리셋 CSS 사용
> - 하나의 서비스에서 작성된 스타일을 다른 아무런 추가 스타일 정의 없이 사용 가능
> - 불필요한 리셋 코드들의 중복 줄임
> - 현재 개발된 페이지는 당장 적용불가, 신규 서비스에 적용

- CSS 선택자로 태그명 사용 지양
> - 글로벌한 스타일을 상속하기 위해서 태그명 자체가 스타일을 가진다면 신규로 추가되는 UIO에서 해당 스타일이 필요하지 않을 경우 매번 리셋 코드를 작성해 줘야한다.
> - 작업을 끝냈으나 나중에 누군가 상속을 주기 위하여 최종 선택자로 태그명이 사용된 스타일을 준다면, 다른 UIO의 영향성이 크기 때문에 위험하다.
> - 스타일과 문서과 분리되지 못하고 서로 디펜더시가 걸린다.
> - 컴포넌트 모듈화와 유지보수의 용이성이 낮아진다.
> - 개발에서 임의로 태그를 추가하는 경우 스타일에 영향이 있다. (개발 수정으로 퍼블리싱 QA가 올라오게된다. 개발에서 마크업을 추가하지 않도록 요청(산출물과의 싱크가 맞지 않게됨), 혹 추가되더라도 화면에 영향은 없어야 한다.)

- z-index 속성 값 정의
> - 서비스가 점점 커져감에 따라 공통요소들과 컨텐츠 영역의 z-index default range 지정 필요 (디테일한 정의는 불가, 공통영역과 컴포넌트 단위에서 가질 수 있는 max값 정도)

- id, class 네이밍 규칙 권고(강제 규칙 X)
> - BEM, OOCSS, SMACSS

- 스크립트용 id, class 명 규칙
> - 스크립트용 선택자 명에는 '\_'를 붙여서 사용
> - 동작이 스타일에 디펜던시가 걸림, 스크립트 바인딩 셀렉터로 tag를 사용했다면, 동작/스타일/문서가 서로서로 디펜던시가 걸리게됨. 유지보수시 영향성 파악이 힘들게되어 레거시코드로 남고 동일한 부분에 대한 새로운 코드들이 나오게됨. 향후 css, js 최적화를 불가능하게 하고 접근성을 위한 html tag 수정할 경우에도 마찬가지 영향을 미침

    ```html
    <div class="class_for_style _button--search">
    ```

- SCSS도입
> - 대규모 서비스를 개발할때 컴포넌트 난위로 쪼개서 개발이 가능하다.
> - 대규모 서비스를 여러명이 개발하거나 심지어 한 페이지를 여러명이서 개발하더라도 컴포넌트 단위로 스타일을 작성하므로 컴플릭이 발생할 일이 없다.
> - 러닝커브가 이슈가 되지 않는다. css문법 100% 그대로 사용할 수 있기 때문에 scss를 모르는 사람도 scss를 사용할 수 있다. 심지어 css를 그대로 scss에 붙여도 상관없다.
> - mixin(util성 스티일) 파일을 생성하여 자주 사용하거니 유용한 스타일을 정의할 수 있으며 함수처럼 사용할 수 있다.
> - 변수를 사용할 수 있다.
> - 변수, 함수(mixin), 컴포넌트 단위 개발로 생산성이 올라간다.(중복코드 작성하는 시간을 줄이고, 컴플릭 해소를 위해 애쓸 필요가 없다. 물론 브랜치가 다른 경우 동일파일 수정으로 발생은 있음, 적어도 하나의 서비스 개발시 이걸로 애쓸 필요는 없다.)
> - 스프라이트 이미지를 동적으로 생성할 수 있다(Front Task Tool과 같이 사용시)

<!--
그리고 같은 작업자는 같은 파일을 쓰지 않도록 하는게 좋을 것 같다.
scss와 task툴을 쓰지 않는 이상은 그게 최선일 듯
지마켓 모바일 개발자가 찾아와서 UI가 이상하다 카테고리 레이어 닫기버튼이 없는거같다고햇다.
원래있었는데 사라졌다. 커밋로그를 보니 누군가(8/1 deba2bc) common.css 에 smiledelivery.css에 있는 코드 일부를 옮겨서 중복 코드가 들어갔고 그사이 smiledelivery.css가 수정되었다.
일단 이슈가 발생한 상태에서 common.css에 추가된 코드를 제거할 수 없다.
smiledelivery에서 기존 코드를 지우고 새로운 코드로 스타일을 잡앗더라도 실제로는 기존 코드는 common.css에서 스타일에 영향을 주고 있고 지웠을경우 이슈를 확신할 수 없기 때문이다. 물론 개발자 본이이 잘 파악하고 확인하면 되지만 이 역시 휴먼에러를 유발할 가능성은 있기때문이다. 그럼 지우고 페이지가 잘 나오는지 확인이 필요하고 dweb이라면 브라우저별 다시 재확인과 디자인 검수를 다시 해야한다.
smiledelivery.css를 여려명이 같이 쓰고 있어서 발생한 문제다.

위 모든 부분은 휴먼 에러를 방지하기 위함이 첫번째이며 반복 혹은 불필요한 중복 코드 사용으로 개발 scope단축을 위함이며 마지막으로 관리를 편리하게 하기위함이다.
-->

### Javascript
- 글로벌 변수 사용 금지
- 로직과 뷰를 분리하기 위하여 컴파일 없이 사용가능한 JS 템플릿 도입, 복잡한 뷰일수록 로직과 얽히게되면 유지보수 어려움 (언더스코어 사용시 언더스코어 템플릿 사용가능)
- 모든 변수, 함수에 주석 작성, 주석 포맷(jsdoc3 -> Task Tool 사용시 가이드 문서 자동생성 가능)도입
- 네임스페이스 사용
- Util성 Common Script 네임스페이스 사용하여 추가
- 개발쪽과 협업 방식은 작성한 모듈 스크립트를 페이지 단위 네임스페이스에 담아 전달(불필요한 코드 메모리에 올라가지 않고, 다른페이지 코드의 오작동이 영향을 미칠일이 없음)
- 전달한 모듈은 pub/sub의 이벤트 버스 방식으로 처리, 추가 파라미터나 콜백은 이벤트 트리거에 object형식으로 태워보내면 처리 가능
- 언더스코어 도입(es6의 편리한 메소드들을 다른 방식으로 제공해 주어 babel같은 컴파일러 없이도 유사한 효과를 볼 수 있음)
- 모듈 코딩 패턴 가이드 작성하여 사용(앵귤러, 리액트, 뷰, 백본 프레임워크중 차용가능한 부분 적용)
    - 분산된 코드 모듈을 자기 실행 클로저에 집어넣어 전역 변수 사용을 최소화
    - 명확하고 간결한 모듈을 유지하기 위해 모든 외부 종속성을 전달
    - 로직과 데이터를 분리하여 제작
    ```javascript
    var UIModule = function(params){
        this.init(params);
    };

    UIModule.prototype = {
        // const for only this component
        STYLE: {
            ACTIVE: 'active',
            FIXED: 'fixed'
        },
        MESSAGE: {
            ERROR: {
                NETWORK: '네트워크 상태가 불안정합니다. \n잠시 후 다시 시도해 주세요.',
                DATA_TYPE: '숫자만 입력해 주세요.'
            },
            NOTI: {
                ALLOW_GPS: '해당 기능을 사용하기 위해서는 GPS를 활성화하셔야 합니다. \n기능을 활성화 하시겠습니까?',
                CONFIRM: '정말로 해당 목록을 제거하시겠습니까?'
            }
        },

        // variable for only this component
        $dom: {},
        // setting for only this component
        config: {
            version: '1.0.0',
            useLocalStorage: false
        },
        uio: {},
        util: {},

        /**
         * [생성자 메소드]
         * @param  {[type]} params [description]
         * @return {[type]}        [description]
         */
        init: function(params){
            this.params = $.extend(this.config, params);
            this.domCache();
            this.subscribeEvent();
            this.bindInnerEvent();
            this.setTemplate();

            // 디펜던시가 있는 모듈은 인스턴스 생성시 주입 받아 사용한다.
            this.util.Dependency1 = this.params.Dependency1;
            this.util.Dependency1 = this.params.Dependency1;

            // 하위 컴포넌트가 있다면 마찬가지로 주입 받아 각 인스턴스를 생성하여 사용한다.
            this.uio.ChildUIModule1 = new this.params.UIModule1();
            this.uio.ChildUIModule2 = new this.params.UIModule2();
            this.uio.ChildUIModule3 = new this.params.UIModule3();
        },

        /**
         * [DOM 캐시]
         * @return {[type]} [description]
         */
        domCache: function(){
            this.$dom.window = $(window);
            this.$dom.body = $('body');
            this.$dom.mainUIModule = $('#mainUIModule');
        },

        /**
         * [이벤트 등록(구독)]
         * @return {[type]} [description]
         */
        subscribeEvent: function(){
            this.$dom.body
                .on('customEvent', $.proxy(this.doSomethingForPublish1, this))
                // 네임스페이스 적용된 커스텀 이벤트 사용
                .on('customEvent:action', $.proxy(this.doSomethingForPublish2, this));
        },

        /**
         * [모듈내 재사용되는 엘리먼트 선언]
         * @return {[type]} [description]
         */
        setTemplate: function(){
            $.template(
                'UIModuleListItem',
                '<li class="item item__type-${listType} {{if isActive}} active {{/if}}">'+
                    '<span class="item__description">'+
                    '${keyword}'+
                    '</span>'+
                '</li>'
            );
        }
    };
    ```

### 디자이너 요구 사항
- __UI 요소들간 정확한 간격 측정__
- __이미지 사이즈 레티나가 아닌 실제 디바이스 사이즈로 노출될 경우 짝수로 나오도록 가이드 필요__
    - (86px짜리 레티나 아이콘을 전달 받으면 실제로 43px로 작업됩니다. 홀수 경우 매끄럽지 않게 랜더링 될 수 있습니다.)
- __UI 요소 가로/세로 위치 잡을 경우 짝수로 처리 되도록 가이드.__
    - (세로 중앙에 정렬할 요소가 있으면 top에서 50%에 위치시키고 요소의 절반 높이값 만큼 -로 끌어 올리는 방법을 많이 사용합니다. 높이가 43px인 요소의 경우 -21.5px의 -값으로 위치를 잡아야 하는데 이경우 브라우저 랜더링 엔진에 따라 내림 처리를 하거나, 확대할 경우 뭉개짐 이슈가 나올 수 있습니다.)
- __opacity값은 면적이 있는 UI에만 사용 요청__
    - font, border 속성에 opacity값을 먹여서 색상값을 전달 받는데 PC경우 IE하위버전 때문에 opacity 적용이 어렵습니다. 이경우 제플린에서 opacity먹은 색상값을 알 수 없어 이를 알기위해 디자이너와 커뮤니케이션 비용 증가하는 이슈가 있습니다. Mobile/PC 구분없이 opacity는 font, border 같은 속성 외 면적이 있는 디자인에 적용 부탁드립니다.)


### 개발자 요구 사항
- 임의로 html 요소 추가 금지


___

### <span style="color:#4078c0">정리내용</span>
- 목업 QA / 개발 일정 물림
    - 리소스 손실
    - 개발에 물려 DOM이나 디자인 추가 수정 이슈
    > QA 날짜 픽스

-  기획, 디자인에 따라 퍼블리싱이 계속 바뀜
> 이슈발생시 관련자 호출해서 지라나 메일에서 픽스 히스토리 남기도록
