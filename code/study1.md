# 1. ES6

## 1) scope

(scope)변수 영역은 변수가 유효성을 갖는 영역을 가리킨다. 프로그램은 영역을 벗어난 변수를 가리킬 수 없다

### (1) let

: `var`의 경우, 함수 내부에서 정의한 변수가 밖에서 호출이 가능하다.

```js
var name = "global var " ;

function home(){
  var homevar = "homevar";
  for(var i=0; i<100; i++){
  }
  console.log(i);
}

home();

//100
```

: `let`의 경우, 전역이나 다른 함수에서 사용할 수 없다.

: JS는 기본적으로 함수 단위의 스코프를 가졌는데, JS가 발전하면서 block 단위 스코프를 가진다.

: 스코프는 중첩이 가능하며, 하위 스코프가 상위 스코프에 접근이 가능하다.
```js
var name = "global var " ;

function home(){              //블록1
  let homevar = "homevar";
  for(var i=0; i<100; i++){   //블록2
  }
  console.log(i);
}

home();

//i is not defined
```

```js
function foo() {              //블록1
  let a = 'apple';
  function foo2() {           //블록2
    let b = 'banana'
    console.log(a);           //하위스코프에서 상위스코프 접근가능
    console.log(b);
  }
  foo2();
}
foo();
```

<br/>

### (2) closure
- 표 안에 js, java, python이 들어 있다.
- 이제 글씨를 클릭했을 때, 이벤트를 발생시키는 js를 작성하여 closure에 대해 알아보자.
```html
<ul>
  <li>js</li>
  <li>java</li>
  <li>python</li>
</ul>
```
: 내부함수는 외부함수의 지역변수에 접근 할 수 있는데, 외부함수의 실행이 끝나서 외부함수가 소멸된 이후에도 내부함수가 외부함수의 변수에 접근 할 수 있다.

: 내부함수에서 외부함수에서 선언된 변수를 사용한다면 그 내부함수는 `클로저`이다.


: `var`를 사용하게 되면, 올바른 실행이 불가하다.

: 콜백은 나중에 실행되는 것이며, 콜백함수가 가지지 않은 `i`값은 콜백밖에 있는 변수값으로 참조한다.

: 콜백 함수는 이 `i`값을 기억(i값은 클로저 변수)

: `for문`으로 인해 `i`값이 계속 변경이 되면서, 3가지 이벤트 콜백함수가 존재하게 된다.

: 그래서 `콜백함수`들은 밖에서 선언한 `i`값을 참조하면서 쉐어중이다.

: 그러다보니, `for`문의 `i`값이 마지막에 2가되고(외부함수실행끝), `콜백함수`가 다 2값을 공유하면서 문제가 생긴다.

```js
var list = document.querySelectorAll("li");
for(var i=0; i<list.length; i++){                
  list[i].addEventListener("click", function(){   //콜백함수
    console.log(i + "번째 리스트입니다.")           
    //뭐를 눌러도 2번째 리스트입니다만 출력
  })
}
```

: `let(블록단위)`을 사용하게 되면 이 문제가 없어진다.

: 블록단위로 실행되므로 `i`값을 쉐어하는 현상이 발생되지 않는다.

```js
var list = document.querySelectorAll("li");
for(let i=0; i<list.length; i++){                 //외부(i선언한 for문)
  list[i].addEventListener("click", function(){   //내부(콜백함수)
    console.log(i + "번째 리스트입니다.")        
  })
}
```

<br/>

### (3) const
- `const`, 수정하지 않는 변수를 선언한다.

: 그래서 `js`코드를 작성할 때, 기본적으로 `const`변수로 선언하고 수정될 수 있는 변수는 `let`을 사용한다.

: `var`는 사용하지 않는 게 좋다.

```js
function homeHi(){
  const home = "my home";
  home = "change";
  console.log(home);
}

homeHi();  //error, const변수는 재정의할 수 없습니다.
```

<br/>

### (4) const특성과 immutable array
- const는 재참조만 불가!

: `const`를 사용해도 `배열`과 `오브젝트`의 값을 변경하는 게 가능하다.

: 그래서 배열에서 `push`, `pop()`을 사용하여 원소를 추가, 삭제하는 게 가능하다.

: 결국, `const`는 재할당만 불가능한 변수라고 볼 수 있다.

```js
function home(){
  const list = ['a', 'b', 'c'];
  list.push('d');
  console.log(list);
}

home();  // [a, b, c, d]

```

<br/>
- `immutable array`는 어떻게 만들지?

: 뒤로가기, 앞으로 가기를 할 때 저장된 이전 값을 불러와야 한다.

: 이전 데이터를 기억하기 위해 `immutable array`를 사용한다.

: `concat(이전값, 추가값)`을 사용하여 `immutable array`를 만들 수 있다.
```js
const list = ['a', 'b', 'c']

list2 = [].concat(list, "d")

console.log(list)     // ['a', 'b', 'c']
console.log(list2)    // ['a', 'b', 'c', 'd']

```


<br/> <br/>

## 2) string

### (1) string의 새로운 메소드

- 시작문자열 혹은 끝 문자열이 특정문자열인지 아닌지 알아보자.

: es2015에서는 정규표현식없이 메소드로 판단이 가능하다.

```js
let str = 'hello world ^^~~';
let matchstr = 'hello';

//str이 hello로 시작하는 지 어떻게 알 수 있을까?
console.log(str.startsWith(matchstr));      //true

//str이 ~~로 끝나는 보려면 endsWith()
console.log(str.endsWith(matchstr));        //false

//특정 문자열이 들어있어? includes()
console.log(str.includes("^^"));            //true

```

<br/><br/>

## 3) Array

### (1) for of, 순회하기

: `for in`문의 경우, 자신이 가진 객체 이외의 상위객체도 출력하는 경우가 있다.

: 이러한 문제를 예방하 위해 `for of`문을 사용한다.

```js
var data = [1,2,3];


//for문
for(var i=0; i<data.length; i++){
  console.log(i);                 //1 2 3
}


//forEach문
data.forEach(function(value){
  console.log(value);             //1 2 3
})


//for in문(비선호)
//자신이 가진 객체 이외에, prototype과 같이 상위객체도 출력함;;
Array.prototype.getIndex = function(){};
for(let idx in data){
  console.log(data[idx]);        //1 2 3 function(){}
}
  

//for of문[array의 경우]
for(let value of data){
  console.log(value);             //1 2 3
}


//for of문[string의 경우]
var str = "hello";
for(let value of str){
  console.log(value);           //'h', 'e', 'l', 'l', 'o'
}
```

<br/>

### (2) spread operator, 배열의 복사
- spread operator?

: `...변수명`을 하면 해당 변수의 값을 그대로 복사할 수 있다.

```js
let pre = ['a', 'b', 100];
let newData = [...pre];
console.log(newData);           // a, b, 100

```

<br/>

- spread operator의 활용

: spread operator는 원소를 펼치는 형식(더보기 같은 느낌)을 전달한다.

```js
//newData 중간에 pre값을 넣고 싶다면?
let pre = [100, 200, 'hello', null];
let newData = [0,1,2,3, ...pre ,4];
console.log(newData);       //0, 1, 2, 3, 100, 200, 'hello', null, 4



//매개변수를 한번에 전달하고 싶다면?
function sum(a, b, c){
  return a+b+c;
}
let pre2 = [100, 200, 300];
sum.apply(null, pre);     //방법1
sum(...pre2);             //방법2

```

<br/>

### (3) from(), 배
