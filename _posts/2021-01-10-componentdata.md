---
layout: post
title: 뷰 컴포넌트 통신
---

# 컴포넌트 유효 범위

컴포넌트는 각각 자체적으로 고유한 유효 범위(Scope)를 갖고있습니다. 각 컴포넌트의 유효 범위는 독립적이기 때문에 다른 컴포넌트의 값을 직접적으로 참조할 수가 없습니다.

### 컴포넌트 유효 범위

```html
<html>
    <head>
        <title>Vue Component Scope</title>
    </head>
    <body>
        <div id="app">
            <my-component1></my-component1>
            <my-component2></my-component2>
        </div>

        <script src="https://cdn.jsdelivr.net/npm/vue@2.5.2/dist/vue.js"></script>
        <script>
           // 첫 번째 컴포넌트 내용
           var cmp1 = {
               template: '<div>첫 번째 지역 컴포넌트 : {% raw %}{{ cmp1Data }}{% endraw %}</div>',
               data: function() {
                   return {
                       cmp1Data: 100
                   }
               }
           };

           // 두 번째 컴포넌트 내용
           var cmp2 = {
               template: '<div>두 번째 지역 컴포넌트 : {% raw %}{{ cmp2Data }}{% endraw %}</div>',
               data: function() {
                   return {
                       cmp2Data: cmp1.data.cmp1Data
                    }
                }
            }

            new Vue({
                el: '#app',
                // 지역 컴포넌트 등록
                components: {
                    'my-component1': cmp1,
                    'my-component2': cmp2
                }
            });
        </script>
    </body>
</html>
```

2개의 지역 컴포넌트를 등록하고, 한 컴포넌트에서 다른 컴포넌트의 값을 참조하는 코드입니다.

my-component2 컴포넌트 내용에서 {% raw %}`{{ cmp2Data }}`{% endraw %} 는 my-component1 컴포넌트의 data.cmp1Data를 참조하고있습니다. 자바스크립트 객체 참조 방식을 생각해보면 해당 컴포넌트 영역에 참조값 100이 출력 되야 합니다.

하지만 코드를 실행해보면 {% raw %}`{{ cmp2Data }}`{% endraw %} 는 아무것도 출력하지 않습니다.

결과는 다음과 같습니다.

![componentscope](/images/componentscope.png)

{% raw %}`{{ cmp2Data }}`{% endraw %} 에 아무 값도 출력되지 않는 이유는 my-component2에서 my-component1의 값을 직접 참조할 수 없기 때문입니다. 컴포넌트의 유효 범위로 인해 다른 컴포넌트의 값을 직접 접근하지 못하기 때문에 나타난 결과입니다.

이것은 뷰가 가진 단방향 데이터 흐름(One-way Data Flow)의 특징입니다.

***

# 상하위 컴포넌트 관계

뷰 프레임워크에서는 자체적으로 정의한 컴포넌트 데이터 전달 방법을 따라야 합니다. 가장 기본적인 데이터 전달 방법은 상위(부모) -> 하위(자식) 컴포넌트 간의 데이터 전달 방법입니다. 즉, 트리 구조에서 부모 노드, 자식 노드처럼 컴포넌트 간의 관계가 부모, 자식으로 이루어져 있습니다.

인스턴스에 컴포넌트를 등록하면 인스턴스는 상위 컴포넌트(부모 컴포넌트), 등록된 컴포넌트는 하위 컴포넌트(자식 컴포넌트)가 됩니다.

![componenttree](/images/componenttree.png)

상위에서 하위로는 props 속성을 사용하여 데이터를 전달할 수 있고, 하위에서 상위로는 $emit을 사용하여 이벤트를 발생시킬 수 있습니다.

***

# 상위에서 하위 컴포넌트로 데이터 전달

## props 속성

props는 상위 컴포넌트에서 하위 컴포넌트로 데이터를 전달할 때 사용하는 속성입니다. props 속성을 사용하려면 먼저 하위 컴포넌트의 속성에 정의해야 합니다.

### 하위 컴포넌트의 props 속성 정의 방식

```
Vue.component('child-component', {
    props: ['props 속성 이름']
});
```

html 코드에 등록된 child-component 컴포넌트 태그에 v-bind 속성을 입력합니다. v-bind 속성의 왼쪽 값으로 하위 컴포넌트에서 정의한 props 속성을 넣고, 오른쪽 값으로 하위 컴포넌트에 전달할 상위 컴포넌트의 데이터 속성을 지정합니다.

### 상위 컴포넌트의 HTML 코드

```
<child-component v-bind:props 속성 이름="상위 컴포넌트의 data 속성"></child-component>
```

### props 속성을 사용한 데이터 전달

```html
<html>
    <head>
        <title>Vue Props Sample</title>
    </head>
    <body>
        <div id="app">
            <!-- 팁: 오른쪽에서 왼쪾으로 속성을 읽으면 더 수월합니다. -->
            <child-component v-bind:propsdata="message"></child-component>
        </div>

        <script src="https://cdn.jsdelivr.net/npm/vue@2.5.2/dist/vue.js"></script>
        <script>
            Vue.component('child-component', {
                props: ['propsdata'],
                template: '<p>{{ propsdata }}</p>'
            });

            new Vue({
                el: '#app',
                data: {
                    message: 'Hello Vue! passed from Parent Component'
                }
            });
        </script>
    </body>
</html>
```

위 코드는 상위 컴포넌트(여기서는 인스턴스)의 message 속성을 하위 컴포넌트에 props로 전달하여 메시지를 출력하는 예제입니다. 데이터가 전달되는 순서는 다음과 같습니다.

1. new Vue()로 인스턴스를 하나 생성합니다.
2. Vue.component()를 이용하여 하위 컴포넌트인 child-component를 등록합니다.
3. child-component의 내용에 props 속성으로 propsdata를 정의합니다.
4. HTML에 컴포넌트 태그를 추가합니다. `<child-component>` 태그의 v-bind 속성을 보면, v-bind:propsdata="message"는 상위 컴포넌트의 message 속성 값인 Hello Vue! passed from Parent Component 텍스트를 하위 컴포넌트의 propsdata로 전달하였습니다.
5. child-component의 template 속성에 정의된 `<p>{% raw %}{{ propsdata }}{% endraw %}</p>`는 Hello Vue! passed from Parent Component로 랜더링됩니다.

간단히 정리하면 뷰 인스턴스의 data 속성에 정의된 message 속성을 하위 컴포넌트에 props로 전달하여 화면에 나타냅니다.

뷰 인스턴스 안에 child-component 컴포넌트를 전역으로 등록하면, 등록 함과 동시에 뷰 인스턴스가 상위 컴포넌트가 되고 child-component 컴포넌트는 하위 컴포넌트가 됩니다. 그리고 이렇게 새 컴포넌트를 등록한 인스턴스는 최상위 컴포넌트(Root Component)가 됩니다.

***

# 하위에서 상위 컴포넌트로 이벤트 전달

## 이벤트 발생과 수신

하위 컴포넌트에서 이벤트를 발생시켜(event emit) 상위 컴포넌트에 신호를 보냅니다. 상위 컴포넌트에서는 하위 컴포넌트의 특정 이벤트가 발생하기를 기다리고 있다가 하위 컴포넌트에서 특정 이벤트가 발생하면 상위 컴포넌트에서 해당 이벤트를 수신하여 상위 컴포넌트의 메서드를 호출합니다.

## 이벤트 발생과 수신 형식

이벤트 발생과 수신은 $emit()과 v-on: 속성을 사용하여 구현합니다.

### $emit()을 이용한 이벤트 발생

```
// 이벤트 발생
this.$emit('이벤트명');
```

$emit()을 호출하면 괄호 안에 정의된 이벤트가 발생합니다. 일반적으로 $emit()을 호출하는 위치는 하위 컴포넌트의 특정 메서드 입니다. 따라서 $emit()을 호출할 때 사용하는 this는 하위 컴포넌트를 가리킵니다.

### v-on: 속성을 이용한 이벤트 수신

```
// 이벤트 수신
<child-component v-on:이벤트명="상위 컴포넌트의 메서드명"></child-component>
``` 

호출한 이벤트는 하위 컴포넌트를 등록하는 태크에서 v-on:으로 받습니다. 하위 컴포넌트에서 발생한 이벤트명을 v-on: 속성에 지정하고, 속성의 값에 이벤트가 발생했을 때 호출될 상위 컴포넌트의 메서드를 지정합니다.

### 이벤트 발생 및 수신

```html
<html>
    <head>
        <title>Vue Event Emit Sample</title>
    </head>
    <body>
        <div id="app">
            <child-component v-on:show-log="printText"></child-component>
        </div>

        <script src="https://cdn.jsdelivr.net/npm/vue@2.5.2/dist/vue.js"></script>
        <script>
            Vue.component('child-component', {
                template: '<button v-on:click="showLog">show</button>',
                methods: {
                    showLog: function() {
                        this.$emit('show-log');
                    }
                }
            });

            var app = new Vue({
                el: '#app',
                data: {
                    message: 'Hello Vue! passed from Parent Component'
                },
                methods: {
                    printText: function() {
                        console.log('received an event');
                    }
                }
            });
        </script>
    </body>
</html>
```

위 코드는 child-component의 [show] 버튼을 클릭하여 이벤트를 발생시키고, 발생한 이벤트로 상위 컴포넌트(여기서는 루트 컴포넌트)의 printText() 메서드를 실행합니다. 소스 처리 과정은 다음과 같습니다.

1. [show] 버튼을 클릭하면 클릭 이벤트 v-on:lick="showlog"에 따라 showLog() 매서드가 실행됩니다.
2. showLog() 메서드 안에 this.$emit('show-log')가 실행되면서 show-log 이벤트가 발생합니다.
3. show-log 이벤트는 `<child-component>`에 정의한 v-on:show-log에 전달되고, v-on:show-log의 대상 메서드인 최상위 컴포넌트의 메서드 printText()가 실행됩니다.
4. printText()는 recevied an event라는 로그를 출력하는 메서드이므로 마지막으로 콘솔에 로그가 출력됩니다.

***

# 같은 레벨의 컴포넌트 통신

뷰는 상위에서 하위로만 데이터를 전달해야 하는 기본적인 통신 규칙을 따르기 떄문에 바로 옆 컴포넌트에 값을 전달하려면 하위에서 공통 상위 컴포넌트로 이벤트를 전달한 후 공통 상위 컴포넌트에서 다른 하위 컴포넌트에 props를 내려 보내야 합니다. 

이런 방식으로 통신해야 하는 이유는 컴포넌트 고유의 유효 범위 때문입니다. 다른 컴포넌트의 값을 직접 참조하지 못하므로 기본적인 데이터 전달 방식을 활용하여 같은 레벨 같에 통신이 가능하도록 구조를 갖춰야 합니다.

***

# 이벤트 버스

앞서 설명된 내용을 종합해보면 컴포넌트 간의 통신은 상하위 관계가 없으면 통신이 불가능 한 구조로 설명되었습니다.

하지만 서로 다른 컴포넌트 간의 통신이 불가능한 것은 아닙니다. 이벤트 버스(Event Bus)를 이용하면 2개의 컴포넌트 간에 데이터를 주고받을 수 있습니다. 즉, 상위 - 하위 관계를 유지하고 있지 않아도 데이터를 한 컨포넌트에서 다른 컴포넌트로 전달할 수 있습니다.

![componentstruture](/images/componentstructure.png)

하위 컴포넌트 D에서 A로의 통신을 하기 위해선 위 그림처럼 [상위 컴포넌트 B > 최상위 컴포넌트 > 상위 컴포넌트 A] 와 같은 경로로 가야 합니다. 하지만 특정 컴포넌트끼리의 관계는 없지만 서로 데이터를 주고받거나 서로의 기능을 호출하기 위해서는 불필요한 상위 컴포넌트가 필요합니다. 이럴 경우 이벤트 버스를 활용하면 중간 컴포넌트들을 거치지 않고 하위 컴포넌트 D에서 상위 컴포넌트 A로 바로 데이터를 전달할 수 있습니다.

![eventbus](/images/eventbus.png)

## 이벤트 버스 형식

### 이벤트 버스 구현 형식

```
// 이벤트 버스를 위한 추가 인스턴스 1개 생성
var eventBus = new Vue();
```

```
// 이벤트를 보내는 컴포넌트
methods: {
    메서드명: function() {
        eventBus.$emit('이벤트명', 데이터)
    }
}
```

```
// 이벤트를 받는 컴포넌트
methods: {
    created: function() {
        eventBusd.$on('이벤트명', function(데이터) {
            ...
        });
    }
}
```

이벤트 버스를 위한 뷰 인스턴스를 생성해줍니다. 이 인스턴스를 컴포넌트간 통신 매개체로 씁니다.

보내는 컴포넌트에서는 .$emit()을, 받는 컴포넌트에서는 .$on()을 구현합니다.

### 이벤트 버스 구현

```html
<html>
    <head>
        <title>Vue Event Bus Sample</title>
    </head>
    <body>
        <div id="app">
            <child-component></child-component>
        </div>

        <script src="https://cdn.jsdelivr.net/npm/vue@2.5.2/dist/vue.js"></script>
        <script>
            var eventBus = new Vue();

            Vue.component('child-component', {
                template: '<div>하위 컴포넌트 영역입니다.<button v-on:click="showLog">show</button></div>',
                methods: {
                    showLog: function() {
                        eventBus.$emit('triggerEventBus', 100);
                    }
                }
            });

            var app = new Vue({
                el: '#app',
                created: function() {
                    eventBus.$on('triggerEventBus', function(value) {
                        console.log('이벤트를 전달받음. 전달받은 값 : ', value);
                    })
                }
            })
        </script>
    </body>
</html>
```

위 코드는 하위 컴포넌트의 [show] 버튼을 클릭했을 때 이벤트 버스를 이용하여 상위 컴포넌트로 데이터를 전달합니다.

1. 이벤트 버스로 활용할 새 인스턴스를 1개 생성하고 eventBus 변수에 참조합니다.
2. 하위 컴포넌트에 template 속성과 methods 속성을 정의합니다. template 속성에는 텍스트와 [show] 버튼을 추가합니다. methods 속성에는 showLog() 메서드를 정의하고, 메서드 안에는 eventBus.$emit()을 선언하여 triggerEventBus 이벤트를 발생 시킵니다. 이 이벤트는 발생할 때 수신하는 쪽에 인자 값으로 100을 함께 전달합니다.
3. 상위 컴포넌트의 created 라이프 사이클 훅에 eventBus.$on()으로 이벤트를 받는 로직을 선언합니다. 발생한 이벤트 triggerEventBus는 수신할 때 앞에서 전달된 인자 값 100을 콘솔에 출력합니다.

이렇게 서로 상하관계가 아닌 컴포넌트끼리 통신을 가능하게 해주는 것을 뷰에서는 이벤트 버스라 부릅니다.

이벤트 버스를 활용하면 props 속성을 이용하지 않고도 원하는 컴포넌트 같에 직접적으로 데이터를 전달할 수 있어 편리하지만 컴포넌트가 많아지면 어디서 어디로 보냈는지 관리가 되지 않는 문제가 발생합니다. 이 문제를 해결하기 위해서는 뷰엑스(Vuew) 상태 관리도구가 필요합니다.

***