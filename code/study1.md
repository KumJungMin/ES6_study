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
# 2) ES6ㅡ
: 뒤로가기, 앞으로 가기를 할 때 저장된 이전 값을 불러와야 한다.
