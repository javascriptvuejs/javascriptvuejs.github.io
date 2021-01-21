---
layout: post
title: 할 일 관리 애플리케이션 튜토리얼(4/6) - 내용 구현
---

각 컴포넌트 역할에 맞춰 컴포넌트 기능을 구현해 보겠습니다. 컴포넌트별로 구현할 기능은 아래와 같습니다.

- TodoHeader: 애플리케이션 이름 표시
- TodoInput: 할 일 입력 및 추가
- TodoList: 할 일 목록 표시 및 특정 할 일 삭제
- TodoFooter: 할 일 모두 삭제

# 애플리케이션 제목을 보여주는 TodoHeader 컴포넌트

## 애플리케이션 제목 추가

어떤 애플리케이션인지 파악하기 쉽게 애플리케이션 제목을 추가하겠습니다. `<div>` 태그를 삭제하고 `<header>` 와 `<h1>` 태그를 활용하여 아래처럼 제목을 표시합니다.

### TodoHeader 컴포넌트의 `<template>` 내용

```html
<template>
    <header>
        <h1>TODO it!</h1>
    </header>
</template>
```

## CSS로 제목 꾸미기

제목 스타일링을 위해 최상위 컴포넌트의 App.vue와 TodoHeader.vue에 다음과 같이 CSS를 추가합니다.

### App.vue의 CSS 스타일

```css
<style>
    body {
        text-align: center;
        background-color: #F6F6F8;
    }
    input {
        border-style: groove;
        width: 200px;
    }
    button {
        border-style: groove;
    }
    .shadow {
        box-shadow: 5px 10px 10px rgba(0, 0, 0, 0.03)
    }
</style>
```

### TodoHeader.vue의 CSS 스타일

```css
<style scoped>
    h1 {
        color: #2F3B52;
        font-weight: 900;
        margin: 2.5rem 0 1.5rem;
    }
</style>
```

App.vue와 TodoHeader.vue에 추가한 스타일의 역할은 각각 다음과 같습니다.

---

**background-color**: 애플리케이션 전체의 배경 색을 꾸미기 위해 지정

**text-align**: 애플리케이션 전체에서 사용하는 텍스트의 정렬 방식을 선택

**border-style**: 할 일을 입력하는 인풋 박스의 테두리 모양을 정의

**box-shadow**: 할 일을 입력하는 인풋 박스와 할 일 아이템의 아래 그림자 정의

**color**: 애플리케이션 제목의 텍스트 색깔을 지정

**font-weight**: 애플리케이션 제목의 텍스트 굵기를 지정

**margin**: 애플리케이션 제목의 텍스트 여백을 지정

---

`<style>` 태그에 사용된 scoped는 뷰에서 지원하는 속성이며, 스타일 정의를 해당 컴포넌트에만 적용하겠다는 의미입니다.

앞 코그의 실행 결과는 아래와 같습니다.

### TodoHeader 컴포넌트 등록 결과

![todoheader](/images/todoheader.PNG)

***

# 할 일을 입력하는 TodoInput 컴포넌트

## 인풋 박스와 버튼 추가

텍스트 값을 입력받기 위한 `<input>` 태그와 텍스트 값을 저장하기 위한 `<button>` 태그를 추가합니다. `<button>` 태그의 이름은 "추가"로 지정합니다. 태그를 추가하면 화면에서 input 텍스트가 있던 자리에 인풋 박스와 버튼이 생깁니다. 인풋 박스에 텍스트를 입력했을 때 뷰 인스턴스에서 텍스트 값을 인식할 수 있게 v-model 디렉티브와 데이터 속성 newTodoItem을 다음과 같이 추가합니다.

### 인풋 박스에 v-model 디렉티브와 data 속성을 추가한 TodoInput 컴포넌트 코드

```
<template>
    <div>
        <input type="text" v-model="newTodoItem">
        <button>추가</button>
    </div>
</template>

<script>
export default {
    data() {
        return {
            newTodoItem: ''
        }
    }
}
</script>
```

위 코드를 실행한 후 뷰 개발자 도구를 엽니다. 뷰 개발자 도구에서 "components" 탭의 `<app>` 아래 있는 `<TodoInput>` 부분을 클릭하면 newTodoItem의 값이 ""로 되어 있습니다. 인풋 박스에 Hello라는 텍스트를 입력하면서 newTodoItem의 값을 지켜보면 텍스트를 입력함에 따라 newTodoItem의 값도 갱신되는 것을 확인할 수 있습니다.

### 인풋 박스의 입력 값에 따라 갱신되는 newTodoItem 데이터

![todoinput](/images/todoinput.PNG)

## 텍스트를 저장하기 위한 버튼 이벤트

뷰에서 인식한 인풋 박스에 입력된 텍스트 값을 데이터 저장소인 로컬 스토리지에 저장하겠습니다. [추가] 버튼을 클릭했을 때 특정 동작을 수행할 수 있게 v-on:click에 버튼 이벤트 addTodo를 지정합니다. 버튼 이벤트 addTodo()의 로직은 methods에 정의합니다.

### 버튼 클릭 이벤트 addTodo를 추가한 코드

```
<template>
    <div>
        <input type="text" v-model="newTodoItem">
        <button v-on:click="addTodo">추가</button>
    </div>
</template>

<script>
export default {
    data() {
        return {
            newTodoItem: ''
        }
    },
    methods: {
        addTodo() {
            console.log(this.newTodoItem);
        }
    }
}
</script>
```

버튼이 정상적을 동작하는지 확인하기 위해 인풋 박스에 입력된 텍스트 데이터(newTodoItem)를 console.log()로 출력해 보겠습니다. 인풋 박스에 Do it을 입력하고 [추가] 버튼을 클릭하면 다음과 같이 콘솔에 텍스트 값이 표시됩니다.

여기서 this.newTodoItem의 this는 해당 컴포넌트를 가리킵니다. 즉, 여기서는 인스턴스 자체로 컴포넌트인 TodoInput.vue를 가리킵니다.

### 콘솔 로그로 입력된 텍스트를 출력한 화면

![todoconsole](/images/todoconsole.PNG)

## 입력받은 텍스트를 로컬 스토리지에 저장

버튼 이벤트가 제대로 동작하는 것을 확인했으니 입력받은 텍스트를 로컬 스토리지의 setItem() API를 이용하여 저장합니다. setItem()는 로컬 스토리지에 데이터를 추가하는 API입니다. API 형식은 키, 값 형태입니다.

### localStorage.setItem() 코드

```javascript
methods: {
    addTodo() {
        localStorage.setItem(this.newTodoItem, this.newTodoItem);
    }
}
```

methods의 내용을 위와 같이 변경합니다. 인풋 박스에 Do it을 입력하고 [추가] 버튼을 클릭하면 로컬 스토리지에 텍스트 값이 저장됩니다. 로컬 스토리지에 저장된 것을 확인하려면 크롬 개발자 도구의 [Application -> Local Storage -> http://localhost:8080]를 클릭해 확인합니다.

### 로컬 스토리지에 인풋 박스의 텍스트를 저장한 화면

![todosetitem](/images/todosetitem.PNG)

## addTodo() 안에 예외 처리 코드 넣기

인풋 박스에 입력된 텍스트가 없을 경우 로컬 스토리지에 데이터가 저장되지 않게 예외 처리 코드를 추가하겠습니다. 기존의 addTodo() 메서드를 아래와 같이 변경합니다.

### addTodo()와 clearInput() 코드

```javascript
methods: {
    addTodo() {
        if(this.newTodoItem !== '') {
            var value = this.newTodoItem && this.newTodoItem.trim();
            localStorage.setItem(value, value);
            this.clearInput();
        }
    },
    clearInput() {
        this.newTodoItem = '';
    }
}
```

변경 전의 addTodo()는 만약 인풋 박스에 텍스트를 입력하지 않은 상태에서 [추가] 버튼을 클릭하면 빈 데이터가 로컬 스토리지에 저장되는 문제가 있었습니다. 하지만 위와 같이 예외 처리를 함으로써 빈 데이터나 공백 문자열은 저장되지 않습니다.

### 디자인 패턴: 단일 책임 원칙

단일 책임 원칙(Single Responsibility Principle)이란 함수가 하나의 기능만 담당하도록 설계하는 객체 지향 프로그래밍의 디자인 패턴입니다. clearInput()의 `this.newTodoItem = '';` 코드를 addTodo() 메서드에 넣어도 되지만 단일 책임 원칙에 따라 할 일 텍스트를 저장하는 코드는 addTodo()에 넣고, 인풋 박스의 내용을 비우는 코드는 clearInput()에 넣습니다. 다른 메서드에서 인풋 박스의 내용을 비우는 코드가 필요할 경우 `this.newTodoItem = '';` 코드 대신 `this.clearInput()` 을 호출하면 됩니다.

## 아이콘 이용한 직관적인 버튼 모양

현재 버튼은 `<button>` 태그와 "추가"라는 텍스트를 사용하고 있습니다. 앞에서 설치한 어썸 아이콘의 + 아이콘을 이용해서 더 직관적인 버튼 모양을 만들겠습니다.

`<template>` 태그에 기존 `<button>` 태그를 삭제하고 `<span>`, `<i>` 태그를 아래와 같이 추가합니다.

### TodoInput 컴포넌트의 template 코드

```html
<template>
    <div class="inputBox shadow">
        <input type="text" v-model="newTodoItem" placeholder="Type what you have to do" v-on:keyup.enter="addTodo">
        <span class="addContainer" v-on:click="addTodo">
            <i class="addBtn fas fa-plus" aria-hidden="true"></i>
        </span>
    </div>
</template>
```

`<template>` 변경 내용은 다음과 같습니다.

1. placeholder: 인풋 박스의 힌트 속성
2. v-on:keyup.enter: 인풋 박스에서 엔터를 눌렀을 때 동작하는 속성
3. `<span>` : `<button>` 대신 클릭 이벤트를 받을 태그
4. `<i class="fa fa-plus">` : 어썸 아이콘의 + 아이콘을 추가

### TodoInput 컴포넌트의 CSS 코드

```css
<style scoped>
    input:focus {
        outline: none;
    }
    .inputBox {
        background: white;
        height: 50px;
        line-height: 50px;
        border-radius: 5px;
    }
    .inputBox input {
        border-style: none;
        font-size: 0.9rem;
    }
    .addContainer {
        float: right;
        background: linear-gradient(to right, #6478FB, #8763FB);
        display: block;
        width: 3rem;
        border-radius: 0 5px 5px 0;
    }
    .addBtn {
        color: white;
        vertical-align: middle;
    }
</style>
```

`<style>` 변경 내용은 다음과 같습니다.

---

**outline**: 할 일을 입력하는 인풋 박스의 선 스타일 지정

**background**: 인풋 박스의 배경색 지정

**height**: 인풋 박스의 높이 설정

**line-height**: 인풋 박스에 입력되는 텍스트의 중앙 정렬을 위해 설정

**border-radius**: 인풋 박스의 둥근 테두리 속성 설정

**float**: 할 일 추가 버튼이 표시될 위치 정의

**vertical-align**: 할 일 추가 아이콘의 수직 정렬 정의

---

### TodoInput 컴포넌트의 CSS 스타일링이 완료된 화면

![todoinputcss](/images/todoinputcss.PNG)

***

# 저장된 할 일 목록을 표시하는 TodoList 컴포넌트

## 할 일 목록 만들기

HTML에서 일반적으로 목록 아이템(list item)을 나타낼 떄 사용하는 기본 태그는 `<ul>`입니다. `<ul>` 태그 안에 `<li>` 태그를 추가하면 추가한 숫자만큼 목록에 아이템이 추가됩니다.

### 목록을 나타내는 `<ul>` 코드

```html
<template>
    <section>
        <ul>
            <li>할 일 1</li>
            <li>할 일 2</li>
            <li>할 일 3</li>
        </ul>
    </section>
</template>
```

### `<ul>` 태그와 `<li>` 태그로만 나타낸 할 일 목록

![todolist](/images/todolist.png)

## 로컬 스토리지 데이터를 뷰 데이터로 저장

로컬 스토리지의 데이터를 담을 todoItems 데이터 속성을 빈 배열로 선언합니다. 그리고 created() 라이프 사이클 훅에 for 반복문과 push()로 로컬 스토리지의 모든 데이터를 todoItems에 담는 로직을 추가합니다. push()는 배열의 끝 요소에 배열 아이템을 하나씩 추가하는 자바스크립트 내장 API입니다.

### 로컬 스토리지 데이터를 뷰 데이터에 저장하는 코드

```javascript
...
export default {
    data() {
        return {
            todoItems: []
        }
    },
    created() {
        if(localStorage.length > 0) {
            for(var i=0; i<localStorage.length; i++) {
                this.todoItems.push(localStorage.keu(i));
            }
        }
    }
...
```

## 뷰 데이터의 아이템 개수만큼 화면에 표시

아래와 같이 v-for 디렉티브로 구현합니다.

### v-for 디렉티브를 이용한 할 일 목록 랜더링

```html
<template>
    <section>
        <ul>
            <li v-for="todoItem in todoItems" :key="todoItem">{% raw %}{{ todoItem }}{% endraw %}</li>
        </ul>
    </section>
</template>
```

여기서 v-for 디렉티브는 뷰 데이터 속성 todoItems의 내용물 개수만큼 반복해서 `<li>` 태그를 출력하는 디렉티브입니다. todoItems의 타입은 배열이기 때문에 배열의 요소 숫자만큼 반복해서 아래와 같이 출력합니다.

### 로컬 스토리지에 저장된 아이템을 모두 목록에 출력한 화면

![todolistdata](/images/todolistdata.PNG)

***

# TodoList.vue에 할 일 삭제 기능 추가

## 할 일 목록 & 삭제 버튼 마크업

### TodoList 컴포넌트의 template 코드

```html
<template>
    <section>
        <ul>
            <li v-for="todoItem in todoItems" :key="todoItem" class="shadow">
                <i class="checkBtn fas fa-check" aria-hidden="true"></i>
                {% raw %}{{ todoItem }}{% endraw %}
                <span class="removeBtn" type="button">
                    <i class="far fa-trash-alt" aria-hidden="true"></i>
                </span>
            </li>
        </ul>
    </section>
</template>
```

`<i>` 태그로 할 일 체크 버튼과 삭제 버튼에 사용할 체크 아이콘과 휴지통 아이콘을 추가합니다. 각 아이콘의 영역이 좁기 때문에 아이콘을 클릭했을 때 이벤트를 잘 잡을 수 있게 `<i>` 태그에 상위 태그 `<span>` 을 두어 클릭할 수 있는 영역을 키웁니다.

### TodoList 컴포넌트의 CSS 코드

```css
<style scoped>
    ul {
        list-style-type: none;
        padding-left: 0px;
        margin-top: 0;
        text-align: left;
    }
    li {
        display: flex;
        min-height: 50px;
        height: 50px;
        line-height: 50px;
        margin: 0.5rem 0;
        padding: 0 0.9rem;
        background: white;
        border-radius: 5px;
    }
    .checkBtn {
        line-height: 45px;
        color: #62acde;
        margin-right: 5px;
    }
    .removeBtn {
        margin-left: auto;
        color: #de4343;
    }
</style>
```

### TodoList 컴포넌트의 CSS 스타일링을 완료한 화면

![todolistcss](/images/todolistcss.PNG)

## 할 일 삭제 버튼에 클릭 이벤트 추가

휴지통 아이콘을 클릭했을 떄 삭제하는 기능이 실행되도록 클릭 이벤트를 추가하겠습니다. 휴지통 아이콘의 근처 영역을 클릭해도 클릭 이벤트가 실행될 수 있게 `<span>` 태그에 클릭 이벤트를 추가합니다. @click 은 v-on:click과 동일하게 동작합니다.

### 클릭 이벤트 removeTodo를 추가한 코드

`<span class="removeBtn" type="button" @click="removeTodo">`

이벤트가 잘 실행되는지 확인하는 console.log()를 추가합니다.

### 메서드에 정의한 removeTodo 코드

```javascript
methods: {
    removeTodo() {
        console.log('clicked');
    }
}
```

### 휴지통 버튼을 클릭해 실행한 화면

![todolistclicked](/images/todolistclicked.PNG)

## 선택한 할 일 뷰에서 인식

휴지통 아이콘을 클릭햇을 때 선택한 할 일의 텍스트 값과 인덱스를 가져오는 코드를 추가하겠습니다. 여기서 할 일 목록의 인덱스는 뷰에서 내부적으로 관리하고 있습니다. template 코드와 removeTodo() 코드를 수정하여 텍스트 값과 인덱스(목록에서 순서, 배열 인덱스와 동일)를 반환합니다.

### 선택한 할 일 아이템을 인식하기 위한 template 코드

```html
<template>
    <section>
        <ul>
            <li v-for="(todoItem, index) in todoItems" :key="todoItem" class="shadow">
                <i class="checkBtn fas fa-check" aria-hidden="true"></i>
                {% raw %}{{ todoItem }}{% endraw %}
                <span class="removeBtn" type="button" @click="removeTodo(todoItem, index)">
                    <i class="far fa-trash-alt" aria-hidden="true"></i>
                </span>
            </li>
        </ul>
    </section>
</template>
```

### 선택한 할 일을 인식하는 removeTodo 코드

```javascript
methods: {
    removeTodo(todoItem, index) {
        console.log(todoItem, index);
    }
}
```

v-for 디렉티브에 index가 추가되었습니다. index는 임의로 정의한 변수가 아니라 v-for 디렉티브에서 기본적으로 제공하는 변수입니다. v-for 디렉티브로 반복한 요소는 모두 뷰에서 내부적으로 인덱스를 부여합니다. 특정 할 일의 휴지통 아이콘을 클릭하면 할 일의 텍스트 값과 인덱스 값이 콘솔에 표시됩니다.

### 각각의 할 일을 클릭했을 때의 결과 화면

![todolistindex](/images/todolistindex.PNG)

## 선택한 할 일을 로컬 스토리지와 뷰 데이터에서 삭제

### 로컬 스토리지와 뷰 데이터에서 할 일을 삭제하는 코드

```javascript
methods: {
    removeTodo(todoItem, index) {
        localStorage.removeItem(todoItem);
        this.todoItems.splice(index, 1);
    }
}
```

위 코드는 로컬 스토리지의 데이터를 삭제하는 removeItem() API와 배열의 특정 인덱스를 삭제하는 splice() API로 할 일을 삭제하는 코드입니다.

removeItem() API는 todoItem 인자를 사용하여 로컬 스토리지에서 할 일 텍스트를 삭제합니다. splice() API는 인자로 받은 index를 이용하여 배열의 해당 인덱스에서 1만큼 삭제합니다. splice()는 자바스크립트 기본 내장 API입니다. 배열의 특정 인덱스에서 부여한 숫자만큼의 인덱스에 해당하는 데이터를 삭제합니다.

할 일 목록에서 특정 할 일의 휴지통 아이콘을 클릭하면 목록에서 사라짐과 동시에 로컬 스토리지에 있던 데이터도 삭제됩니다.

### 첫 번째 아이템 JQuery를 삭제한 화면

![todolistdelate](/images/todolistdelate.PNG)

여기서 주목할 부분은 뷰 데이터 속성인 todoItems의 배열 요소를 제거하자마자 바로 뷰에서 자동으로 화면을 다시 갱신한다는 점입니다. 이는 데이터의 속성이 변하면 화면에 즉시 반영하는 뷰의 반응성 때문입니다.

***

# 모두 삭제하기 버튼을 포함하는 TodoFooter 컴포넌트

이 컴포넌트에는 등록된 모든 할 일을 삭제하는 버튼이 들어갑니다.

## 모두 삭제하기 버튼 추가

[Clear All] 버튼을 추가하기 위해 다음과 같이 코딩합니다.

### todoFooter 컴포넌트 코드

```
<template>
    <div class="clearAllContainer">
        <span class="clearAllBtn" @click="clearTodo">Clear All</span>
    </div>
</template>

<script>
export default {
    methods: {
        clearTodo() {
            localStorage.clear();
        }
    }
}
</script>

<style scoped>
    .clearAllContainer {
        width: 8.5rem;
        height: 50px;
        line-height: 50px;
        background-color: white;
        border-radius: 5px;
        margin: 0 auto;
    }
    .clearAllBtn {
        color: #e20303;
        display: black;
    }
</style>
```

`<template>` 태그에 버튼 역할을 하는 태그 `<span>` 을 정의하고, clearTodo 클릭 이벤트를 추가합니다. 버튼의 이름은 "Clear All"이며, 클릭했을 때 메서드에 정의한 clearTodo() 로직이 실행됩니다. clearTodo() 메서드에는 로컬 스토리지의 데이터를 모두 삭제하는 localStorage.clear()를 정의합니다. CSS 코드에는 `<span>` 태그가 버튼 모양을 가질 수 있도록 간단한 속성을 추가합니다.

애플리케이션을 실행하면 다음과 같은 결과 화면이 나옵니다.

### todoFooter가 구현된 화면

![todofooter](/images/todofooter.PNG)

여기서 [Clear All] 버튼을 클릭하면 아래와 같이 로컬 스토리지의 데이터는 삭제되지만 화면이 자동으로 갱신되지는 않습니다.

### [Clear All] 버튼을 클릭했을 때의 결과 화면

![todofooterclear](/images/todofooterclear.PNG)

따라서 다시 브라우저를 새로 고침해야만 할 일 목록이 로컬 스토리지에 저장되어 있는 데이터를 반영하여 표시합니다. 할 일 목록 데이터는 TodoFooter 컴포넌트가 아닌 TodoList 컴포넌트에 있기 때문입니다.

***