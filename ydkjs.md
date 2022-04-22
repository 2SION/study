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

엄격(strict)모드일 때는 전역 객체가 기본 바인딩 대상에서 제외된다. 그래서 this는 undefined가 된다.
```js
function foo() {
	"use strict";

	console.log( this.a );
}
var a = 2;
foo(); // TypeError: `this` is `undefined`
```

this는 함수 호출 시점에 바인딩 되며 함수 호출 방법에 따라 결정 결정되지만, 호출부의 엄격모드 여부는 상관없다.
```js
function foo() {
	console.log( this.a );
}
var a = 2;
(function(){
	"use strict";
	foo(); // 2
})();
```
## 암시적 바인딩
호출부에서 obj 콘텍스트로 foo()를 참조하므로 obj 객체는 함수호출 시점에 함수의 레퍼런스는 '소유'하거나 '포함한다.
```js
function foo() {
	console.log( this.a );
}

var obj2 = {
	a: 42,
	foo: foo
};

var obj1 = {
	a: 2,
	obj2: obj2
};

obj1.obj2.foo(); // 42
```

### 암시적 소실
```js
function foo() {
	console.log( this.a );
}
var obj = {
	a: 2,
	foo: foo
};
var bar = obj.foo; // function reference/alias!
var a = "oops, global"; // `a` also property on global object
console.log(bar)
bar(); // "oops, global"
```
```js
function foo() {
	console.log( this.a );
}
function doFoo(fn) {
	// `fn` is just another reference to `foo`

	fn(); // <-- call-site!
}
var obj = {
	a: 2,
	foo: foo
};

var a = "oops, global"; // `a` also property on global object
doFoo( obj.foo ); // "oops, global"
```
```js
function foo() {
	console.log( this.a );
}

var obj = {
	a: 2,
	foo: foo
};

var a = "oops, global"; // `a` also property on global object

setTimeout( obj.foo, 100 ); // "oops, global"
```

## 명시적 바인딩
call(), apply()는 this에 바인딩할 객체를 첫째 인자로 받아 함수 호출 시 this를 지정한 객체로 직업 바인딩한다.
```js
function foo() {
	console.log( this.a );
}

var obj = {
	a: 2
};

foo.call( obj ); // 2
```

객체 대신 원시 값(문자열, 불리언, 숫자)을 인자로 전달하면 원시 값에 대응하는 객체(new String(), new Boolean(), new Number())로 래핑된다. 이 과정을 박싱(Boxing)이라고 한다.