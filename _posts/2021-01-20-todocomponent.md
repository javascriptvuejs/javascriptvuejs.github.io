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