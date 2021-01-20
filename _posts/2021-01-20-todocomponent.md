---
layout: post
title: 할 일 관리 애플리케이션 튜토리얼(3/6) - 컴포넌트
---

프로젝트 초기 구성을 완료하였으니 애플리케이션 구동에 필요한 컴포넌트들을 생성하고 등록합니다. 대상 컴포넌트는 TodoHeader, TodoInput, TodoList, TodoFooter 총 4개입니다.

# 컴포넌트 생성

프로젝트에서 src 폴더 밑에 components 폴더를 생성하고 그 아래에 TodoHeader.vue, TodoInput.vue, TodoList.vue, TodoFooter.vue를 생성합니다.

### 컴포넌트 생성 후 프로젝트 폴더 구조

![todocomponent](/images/todocomponent.PNG)

각 컴포넌트에 아래와 같이 간단한 코드를 삽입합니다.

### TodoHeader.vue

```
<template>
    <div>header</div>
</template>

<script>
export default {

}
</script>

<style>
</style>
```

### TodoInput.vue

```
<template>
    <div>input</div>
</template>

<script>
export default {

}
</script>

<style>
</style>
```

### TodoList.vue

```
<template>
    <div>list</div>
</template>

<script>
export default {

}
</script>

<style>
</style>
```

### TodoFooter.vue

```
<template>
    <div>footer</div>
</template>

<script>
export default {

}
</script>

<style>
</style>
```

.vue 파일의 기본 구조에서 `<template>` 영역에 `<div>` 태그를 추가하고 컴포넌트 이름을 텍스트로 삽입하였습니다.

***

# 컴포넌트 등록

생성한 4개의 컴포넌트를 등록하여 화면에 나타냅니다. 애플리케이션에서 사용할 컴포넌트는 모두 최상위 컴포넌트인 App.vue에 등록합니다. src/App.vue의 기존 코드를 아래와 같이 수정합니다.

### 불필요한 코드를 제거한 App.vue

```
<template>
    <div id="app"></div>
</template>

<script>
export default {

}
</script>

<style>
</style>
```

우선 App.vue 파일에 아래와 같이 각 4개의 컴포넌트를 지역 컴포넌트로 등록하겠습니다.

### App.vue에 등록한 지역 컴포넌트

```javascript
<script>
export default {
    components: {
        'TodoHeader': TodoHeader,
        'TodoInput': TodoInput,
        'TodoList': TodoList,
        'TodoFooter': TodoFooter
    }
}
</script>
```

싱글 파일 컴포넌트 체계(.vue 파일 체계)에서는 특정 컴포넌트에서 다른 위치에 있는 컴포넌트의 내용을 불러올 때 아래 형식을 사용합니다.

### 컴포넌트 내용을 불러오기 위한 ES6 import 구문

`import 불러올 파일의 내용이 담길 객체 from '불러올 파일 위치';`

App.vue 파일에서 다른 컴포넌트의 내용을 import from 구문으로 받아와서 components 속성에 연결해 줍니다.

### import 구문으로 컴포넌트 내용을 불러와서 등록하는 코드

```javascript
<script>
import TodoHeader from './components/TodoHeader.vue'
import TodoInput from './components/TodoInput.vue'
import TodoList from './components/TodoList.vue'
import TodoFooter from './components/TodoFooter.vue'
...
</script>
```

컴포넌트 등록을 완료하였으니 컴포넌트 태그 4개를 App.vue의 `<div id="app">` 태그 안에 추가합니다.

### 등록한 컴포넌트 4개를 HTML에 표시

```html
<template>
    <div id="app">
        <TodoHeader></TodoHeader>
        <TodoInput></TodoInput>
        <TodoList></TodoList>
        <TodoFooter></TodoFooter>
    </div>
</template>
```

명령 프롬프트에서 npm run dev를 이용해 서버를 실행키시면 아래와 같은 화면이 나옵니다. 만약 이미 서버가 실행 중이라면 변경된 코드를 저장했을 때 자동으로 화면이 새로 고침됩니다.

### 컴포넌트가 모두 등록된 결과 화면과 뷰 개발자 도구를 실행한 화면

![todocomponents](/images/todocomponents.PNG)

크롬 개발자 도구를 열어 뷰 개발자 도구를 확인하면 App이라는 최상위 컴포넌트 아래에 TodoHeader, TodoInput, TodoList, TodoFooter가 각각 하위 컴포넌트로 생성되었습니다.

---

## 뷰 개발자 도구 설치

뷰 개발자 도구(뷰 크롬 플러그인)는 뷰로 개발할 때 도움을 주는 유용한 도구로, 뷰로 만든 웹 애플리케이션의 구조를 간편하게 디버깅하거나 분석할 수 있습니다. 크롬 브라우저와 파이어폭스(Firefox), 사파리(Safari)에서 모두 지원합니다.

1. Chrome Web Store에 접속합니다.
2. Vue.js devtools를 검색하여 찾습니다.
3. [CHROME에 추가] 버튼을 클릭합니다.
4. 설치 여부를 묻는 팝업 창에서 [확장 프로그램 추가] 버튼을 클릭하여 설치합니다.
5. 설치가 완료 되면 브라우저 주소 창 오른쪽에 뷰 로고 모양의 아이콘이 생기고, 뷰 개발자 도구를 사용할 수 있습니다. 

***