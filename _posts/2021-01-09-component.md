---
layout: post
title: 뷰 컴포넌트
---

# 컴포넌트란?

조합하여 화면을 구성할 수 있는 블록(화면의 특정 영역)을 의미합니다.

뷰에서는 웹 화면을 구성할 때 흔히 사용하는 내비게이션 바(navigation bar), 테이블(table), 리스트(list), 인풋 박스(input box) 등과 같은 화면 구성 요소들을 잘게 쪼개어 컴포넌트로 관리합니다.

### 컴포넌트로 구분한 화면 영역 간의 관계도

![component](/images/component.png)

이처럼 뷰의 컴포넌트간 관계는 트리(Tree)구조를 가지고 있습니다.

---

트리란 컴퓨터 자료구조 중 하나로, 노드끼리의 연결이 부모 - 자식 구조를 따릅니다. 전체적인 모양이 나무와 비슷해서 트리라고 부릅니다.

***

# 컴포넌트 등록

컴포넌트를 등록하는 방법은 전역과 지역의 두 가지가 있습니다. 지역(Local) 컴포넌트는 정해진 특정 구역에서만 사용할 수 있는 컴포넌트이고, 전역(Global) 컴포넌트는 모든 인스턴스에서 공통으로 사용할 수 있는 컴포넌트입니다.

## 전역 컴포넌트

전역 컴포넌트는 뷰 라이브러리를 로딩하고 나면 접근 가능한 Vue 변수를 이용하여 등록합니다. 전역 컴포넌트를 모든 인스턴스에 등록하려면 Vue 생성자에 .component()를 호출하여 수행하면 됩니다.

### 전역 컴포넌트 등록 형식

```
Vue.component('컴포넌트 이름', {
    //컴포넌트 내용
});
```

컴포넌트 이름은 template 속성을 사용할 html 사용자 정의 태크(custom tag) 이름을 의미합니다.

#### template 속성

화면에 표시될 HTML 코드를 담고 있는 속성입니다.

> **사용자 정의 태크**: HTMl 표준 태그들 이외에도 웹 개발자가 직접 정의하여 사용할 수 있는 태크

즉, 컴포넌트 태그가 실제 화면의 html 요소로 변환될 떄 표시될 template 등의 속성들을 컴포넌트 내용에 작성합니다.

### 전역 컨포넌트 등록

```html
<html>
    <head>
        <title>Vue Component Registration</title>
    </head>
    <body>
        <div id="app">
            <button>컴포넌트 등록</button>
            <my-component></my-component>
        </div>

        <script src="https://cdn.jsdelivr.net/npm/vue@2.5.2/dist/vue.js"></script>
        <script>
            Vue.component('my-component', {
                template: '<div>전역 컴포넌트가 등록되었습니다!</div>'
            });

            new Vue({
                el: '#app'
            });
        </script>
    </body>
</html>
```

위 코드를 실행하면 아래와 같은 결과 화면이 나타납니다.

![globalcomponent](/images/globalcomponent.png)

전역 컴포넌트를 등록하기 위해 html에서 사용할 태그 이름을 컴포넌트 이름으로 작성하고, 중괄호 { } 안에 컴포넌트 내용을 작성합니다.

컴포넌트 이름을 my-component로 지정 하였으므로 html에 작성된 `<my-component></my-component>` 에 컴포넌트가 등록됩니다.

컴포넌트 내용으로는 template 속성을 정의하고 template에 `<div>전역 컴포넌트가 등록되었습니다!</div>` 라는 div 태그를 등록하였습니다.

따라서 등록된 `<my-component></my-component>` 컴포넌트는 실제로 화면에 `<div>전역 컴포넌트가 등록되었습니다!</div>` 와 같이 랜더링됩니다.

결과적으로 인스턴스가 생성된 후 화면에 컴포넌트가 랜더링 될때 실제 html 코드는 다음과 같습니다.

![globalcomponentconclusion](/images/globalcomponentconclusion.png)

## 지역 컴포넌트

지역 컴포넌트 등록은 전역 컴포넌트 등록과는 다르게 인스턴스에 components 속성을 추가하고 등록할 컴포넌트 이름과 내용을 정의합니다.

### 지역 컴포넌트 등록 형식

```
new Vue({
    components: {
        '컴포넌트 이름': 컴포넌트 내용
    }
});
```

컴포넌트 이름은 전역 컴포넌트와 마찬가지로 html에 등록할 사용자 정의 태그를 의미하고, 컴포넌트 내용은 태그가 실제 화면 요소로 랜더링될 때의 내용을 의미합니다.

### 지역 컴포넌트 등록

```html
<html>
    <head>
        <title>Vue Component Registration</title>
    </head>
    <body>
        <div id="app">
            <button>컴포넌트 등록</button>
            <my-local-component></my-local-component>
        </div>

        <script src="https://cdn.jsdelivr.net/npm/vue@2.5.2/dist/vue.js"></script>
        <script>
            var cmp = {
                //컴포넌트 내용
                template: '<div>지역 컴포넌트가 등록되었습니다!</div>'
            };

            new Vue({
                el: '#app',
                components: {
                    'my-local-component': cmp
                }
            });
        </script>
    </body>
</html>
```

뷰 인스턴스에 components 속성을 추가하고 컴포넌트 이름에는 my-local-component를, 컴포넌트 내용에는 컴포넌트 내용을 정의한 변수 cmp를 지정합니다. 변수 cmp에는 template 속성을 이용하여 화면에 나타낼 컴포넌트의 내용을 정의했습니다. 그리고 html에 `<my-local-component></my-local-component>`를 추가하여 컴포넌트를 화면에 랜더링합니다.

아래와 같은 결과화면이 나타납니다.

![localcomponent](/images/localcomponent.png)

***

# 지역 컴포넌트와 전역 컴포넌트의 차이

지역 컴포넌트와 전역 컴포넌트의 차이는 인스턴스 유효범위와 밀접한 관련이 있습니다.

### 인스턴스 유효 범위와 지역 컴포넌트, 전역 컴포넌트 간 관계

```html
<html>
    <head>
        <title>Vue Local and Global Components</title>
    </head>
    <body>
        <div id="app">
            <h3>첫 번째 인스턴스 영역</h3>
            <my-global-component></my-global-component>
            <my-local-component></my-local-component>
        </div>
        <hr>
        <div id="app2">
            <h3>두 번째 인스턴스 영역</h3>
            <my-global-component></my-global-component>
            <my-local-component></my-local-component>
        </div>

        <script src="https://cdn.jsdelivr.net/npm/vue@2.5.2/dist/vue.js"></script>
        <script>
           // 전역 컴포넌트 등록
           Vue.component('my-global-component', {
               template: '<div>전역 컴포넌트입니다.</div>'
           });

           // 지역 컴포넌트 내용
           var cmp = {
               template: '<div>지역 컴포넌트입니다.</div>'
           };

           new Vue({
               el: '#app',
               // 지역 컴포넌트 등록
               components: {
                   'my-local-component': cmp
               }
           });

           // 두 번째 인스턴스
           new Vue({
               el: '#app2'
           });
        </script>
    </body>
</html>
```

위 코드의 결과는 다음과 같습니다.

![componentconclusion](/images/componentconclusion.png)

전역 컴포넌트는 인스턴스를 새로 생성할 때마다 인스턴스에 components 속성으로 등록할 필요 없이 한 번 등록으로 어느 인스턴스에서든지 사용할 수 있습니다. 반대로 지역 컴포넌트는 새로 인스턴스를 생성할 때마다 등록해 줘야 합니다.

첫 번째 인스턴스의 유효 범위는 첫 번째 인스턴스 영역으로 제한됩니다. 따라서 첫 번째 인스턴스에서 정의한 컴포넌트는 두 번째 인스턴스에서 인식되지 않습니다.

***