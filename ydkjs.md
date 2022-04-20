# this
## this를 왜?
암시적인 객체 레퍼런스를 함께 넘기는 this 체계. 명확하고 재사용하기 쉽다.
함수가 적절한 콘텍스트(맥락, 상위객체?)를 자동 참조. 구조의 편리성.
```js
function identify() {
	return this.name.toUpperCase();
}

function speak() {
  console.log(this) //{ name: "Kyle" }, { name: "Reader" }
	var greeting = "Hello, I'm " + identify.call( this );
	console.log( greeting );
}

var me = {
	name: "Kyle"
};

var you = {
	name: "Reader"
};

speak.call( me ); // Hello, I'm KYLE
speak.call( you ); // Hello, I'm READER
```
## this는?
어떤 함수를 호출 시 만들어지는 실행 콘텍스트에는 함수 호출위치, 방법, 전달된 인자 등 정보가 담겨있고, this는 그 정보 증 하나로 함수가 실행되는 동안 이용할 수 있다.

this는 실제 함수 호출 시점에서 바인딩 되며 함수 호출 방법에 따라 결정된다.

## 기본 바인딩
전역 스코프에 변수를 선업하면 변수명과 같은 이름의 전역 객체 프로퍼티가 생성된다.

```js
function foo() {
	console.log( this.a );
}
var a = 2;
foo(); // 2
```

