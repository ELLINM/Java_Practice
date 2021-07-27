Class
=======
Javascript에서 Class는 함수의 한 종류

예제 코드
<pre>
class User{
  constructor(name) { this.name = name; }
  sayHi() { alert(this.name)}
}

//class User의 type check
alert(typeof User); //function
</pre>

class User의 문법 구조

1. User라는 이름을 가진 함수 생성 -> 함수 본문은 생성자 메서드 constructor에서 가져옴

-> 생성자 메서드가 없으면 본문이 비워진 채로 함수 생성

2. sayHi같은 class내에서 정의한 메서드를 User.prototype에 저장

3. new User를 호출 -> 객체 생성 -> 객체의 메서드 호출 -> 메서드를 프로토타입에서 가져옴 -> 객체에서 클래스 메서드에 접근


코드
<pre>
class User{
  constructor(name) { this.name = name; }
  sayHi() { alert(this.name)}
}

//class User의 type check
alert(typeof User); //function

// 정확히는 생성자 메서드와 동일
alert(User === User.prototype.constructor); // true

// 클래스 내부에서 정의한 메서드는 User.prototype에 저장
alert(User.prototype.sayHi); // alert(this.name);

// 현재 프로토타입에는 메서드가 두 개
alert(Object.getOwnPropertyNames(User.prototype)); // constructor, sayHi
</pre>


Class와 Function 차이
---------
1. Class로 만든 함수엔 특수 내부 프로퍼티 [[FunctionKind]]:"classConstructor"가 이름표 처럼 붙음

Javascript는 다양한 방법을 사용해 함수에 [[FunctionKind]]:"classConstructor"가 있는지 확인

이러한 검증 과정으로 클래스 생성자를 new와 함께 호출 하지 않으면 에러 발생

2. 클래스 메서드는 열거 할 수 없음 

클래스의 prototype 프로퍼티에 추가된 메서드 전테의 enumerable 플래그는 false

for..in으로 객체를 순회할 때, 메서드는 순회 대상에서 제외 하고자 하는 경우가 많으므로 유용

3. 클래스는 항상 use strict로 사용 클래스 생성자 안 코드 전체에는 자동으로 적용


클래스 표현식
----------
함수처럼 클래스도 다른 표현식 내부에서 정의, 전달, 반환, 할당 가능

예제 코드
<pre>
let User = class{
  sayHi() {
    alert("Hello");
  }
};
</pre>

클래스 표현식에도 이름을 붙일 수 있음

클래스 표현식에 이름을 붙이면, 이 이름은 오직 클래스 내부에서만 사용 가능

예제 코드
<pre>
let User = class MyClass {
  sayHi() {
    alert(MyClass); // MyClass라는 이름은 오직 클래스 안에서만 사용 가능
  }
};

new User().sayHi(); // 제대로 동작 -> MyClass의 정의를 보여줌

alert(MyClass); // ReferenceError: MyClass is not defined, MyClass는 클래스 밖에서 사용 불가
</pre>

<pre>
function makeClass(phrase) {
  // 클래스를 선언하고 이를 반환함
  return class {
    sayHi() {
      alert(phrase);
    };
  };
}

// 새로운 클래스를 만듦
let User = makeClass("Hello");

new User().sayHi(); // Hello
</pre>

getter & setter
--------
리터럴을 사용해 만든 객체처럼 클래스도 getter 또는 setter, 계산된 프로퍼티를 포함 할 수 있음

예제 코드
<pre>
class User {

  constructor(name) {
    // setter를 활성화
    this.name = name;
  }

  get name() {
    return this._name;
  }

  set name(value) {
    if (value.length < 4) {
      alert("이름이 너무 짧습니다.");
      return;
    }
    this._name = value;
  }

}

let user = new User("John");
alert(user.name); // John

user = new User(""); // 이름이 너무 짧습니다.
</pre>


바인딩 메서드
---------
함수 바인딩과 마찬가지로 동적인 this를 가짐

따라서 객체 메서드를 여기저기 전달해 전혀 다른 컨텍스트에서 호출하게 되면

this는 원래 객체를 참조 하지 않음

예제 코드
<pre>
class Button {
  constructor(value) {
    this.value = value;
  }

  click() {
    alert(this.value);
  }
}

let button = new Button("hello");

setTimeout(button.click, 1000); // undefined
</pre>

해결 방법

1. setTimeout(() => button.click(), 1000) 같이 래퍼 함수를 전달

2. 생성자 안 들에서 메서드를 객체 바인딩

3. 클래스 필드
<pre>
class Button {
  constructor(value) {
    this.value = value;
  }
  click = () => {
    alert(this.value);
  }
}

let button = new Button("hello");

setTimeout(button.click, 1000); // hello
</pre>

클래스 필드 click = () => {...}는 각 Button객체마다 독립적인 함수를 만들고 함수의 this를 해당 객체에 바인딩

따라서 button.click을 아무곳에나 전달 할 수 있고, this엔 항상 의도한 값이 들어가게됨
