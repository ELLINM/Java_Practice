Set
=====
js 표준 내장 객체

Set 객체는 자료형에 관계 없이 원시 값과 객체 참조 모두 유일한 값을 저장 가능

어떤 값은 그 Set 콜렉션 내에서 유일


객체 생성
-----
set객체는 set 생성자 함수로 생성

생성자 함수는 iterable 객체를 인수로 받아 set객체를 생성

중복값은 저장되지 않는다

<pre>const set = new Set();</pre>


요소 개수 확인
----
set 객체의 요소 개수를 확인할 때는 .size 프로퍼티를 사용

size 프로퍼티는 setter 함수없이 getter 함수만 존재하는 접근자 프로퍼티

size 프로퍼티에 숫자를 할당하여 set 객체의 요소개수를 변경할수 없음

<pre>console.log(set.size);</pre>

요소 추가
----
set 객체에 요소를 추가할때는 .add 메서드를 사용

add 메서드는 새로운 요소가 추가된 Set 객체를 반환

<pre>set.add(1).add(2).add(2);</pre>


요소 존재 여부 확인
-----
Set 객체에 특정 요소가 존재하는지 확인하려면 .has 메서드를 사용

.has는 요소가 있는지 없는지에 따라 불리언값을 반환

<pre>
const set = new Set([1, 2, 3]);

console.log(set.has(2)); // true
console.log(set.has(4)); // false
</pre>


요소 삭제
------
set 객체의 특정 요소를 삭제하려면 .delete 메서드를 사용

.delete 메서드는 삭제 성공여부를 나타내는 boolean값을 반환

.delete 메서드에는 인덱스가 아니라 삭제하려는 요소값을 인수로 전달

.delete는 boolean값을 반환하기 때문에 add처럼 여러개를 붙여서 사용할수 없음

<pre>
const set = new Set([1, 2, 3]);

// 요소 2를 삭제한다.
set.delete(2);
console.log(set); // Set(2) {1, 3}
</pre>


요소 일괄 삭제
-----
set 객체의 모든 요소를 일괄삭제하려면 .clear메서드를 사용

clear는 언제나 undefined를 반환

<pre>
const set = new Set([1, 2, 3]);

set.clear();
console.log(set); // Set(0) {}
</pre>


요소 순회
-----
set객체의 요소를 순회하려면 .forEach메서드를 사용

forEach메서드에 콜백함수를 인수로 넣어주게 되는데, 콜백함수는 3개의 인수를 전달받음

인수 - 현재 순회중인 요소값, 현재 순회중인 요소값, 현재 순회중인 set 객체 자체

첫번째와 두번째 인수가 같은 이유는 .forEach메서드와 인터페이스를 통일하기 위함

.forEach는 두번째 인수로 인덱스를 전달받지만, set은 순서에 의미가없기때문에 인덱스를 받지않음

set 객체는 이터러블이기 때문에 for ..of문으로 순회할수있고, 스프레드문법과, 배열 디스트럭처링의 대상이 될 수도 있음

<pre>
const set = new Set([1, 2, 3]);

set.forEach((v, v2, set) => console.log(v, v2, set));
</pre>


집합 연산
----
set 객체는 교집합, 차집합, 합집합 등을 구현

- 교집합

intersection메서드 내의 this는 생성자 함수가 생성한 인스턴스

<pre>
const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA와 setB의 교집합
console.log(setA.intersection(setB)); // Set(2) {2, 4}
// setB와 setA의 교집합
console.log(setB.intersection(setA)); // Set(2) {2, 4}
</pre>

<pre>
Set.prototype.intersection = function (set) {
  const result = new Set();

  for (const value of set) {
    // 2개의 set의 요소가 공통되는 요소이면 교집합의 대상이다.
    if (this.has(value)) result.add(value);
  }

  return result;
};
</pre>

<pre>
Set.prototype.intersection = function (set) {
  return new Set([...this].filter(v => set.has(v)));
};
</pre>

여러 방식으로 표현 가능


- 합집합

합집합은 result를 인스턴스의 set으로 초기화 하고 result에 다른 집합을 합침

set은 동일한 값을 가지지않으므로 값이 중복되지않게 합집합을 만들 수 있음

<pre>
const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA와 setB의 합집합
console.log(setA.union(setB)); // Set(4) {1, 2, 3, 4}
// setB와 setA의 합집합
console.log(setB.union(setA)); // Set(4) {2, 4, 1, 3}
</pre>

<pre>
Set.prototype.union = function (set) {
  // this(Set 객체)를 복사
  const result = new Set(this);

  for (const value of set) {
    // 합집합은 2개의 Set 객체의 모든 요소로 구성된 집합이다. 중복된 요소는 포함되지 않는다.
    result.add(value);
  }

  return result;
};
</pre>

<pre>
Set.prototype.union = function (set) {
  return new Set([...this, ...set]);
};
</pre>

- 차집합

차집합 A-B는 집합 A에는 존재하지만 B에는 존재하지 않는 요소로 구성

여기서는 delete 메서드를 사용

<pre>
const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA에 대한 setB의 차집합
console.log(setA.difference(setB)); // Set(2) {1, 3}
// setB에 대한 setA의 차집합
console.log(setB.difference(setA)); // Set(0) {}
</pre>

<pre>
Set.prototype.difference = function (set) {
  // this(Set 객체)를 복사
  const result = new Set(this);

  for (const value of set) {
    result.delete(value);
  }

  return result;
};
</pre>

<pre>
Set.prototype.difference = function (set) {
  return new Set([...this].filter(v => !set.has(v)));
};
</pre>


- 부분 집합과 상위 집합

집합 A가 집합 B에 포함되는 경우 집합 A는 B의 부분집합이며 집합 B는 A의 상위집합

<pre>
const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA가 setB의 상위 집합인지 확인한다.
console.log(setA.isSuperset(setB)); // true
// setB가 setA의 상위 집합인지 확인한다.
console.log(setB.isSuperset(setA)); // false
</pre>

<pre>
// this가 subset의 상위 집합인지 확인한다.
Set.prototype.isSuperset = function (subset) {
  for (const value of subset) {
    // superset의 모든 요소가 subset의 모든 요소를 포함하는지 확인
    if (!this.has(value)) return false;
  }

  return true;
};
</pre>

<pre>
Set.prototype.isSuperset = function (subset) {
  const supersetArr = [...this];
  return [...subset].every(v => supersetArr.includes(v));
};
</pre>


공부하게된 이유
------
hackerrank 문제를 풀다가 Set을 활용한 코드를 보게됨

<pre>
function getSecondLargest(nums) {
    // Complete the function
    let largest = Math.max(nums);
    let sec = nums.splice(nums.indexOf(largest),1);
    let seclargest = Math.max(sec);
    
    return seclargest;
}
</pre>

내 코드는 위와 같았고 문제는 해결이 되었지만 제출 후 테스트 코드는 통과되지 않음
비슷한 스타일의 코트가 있을까 확인하다가 Set을 찾게됨

<pre>
function getSecondLargest(nums) {
    // Complete the function
    nums = [...new Set(nums)];
    nums.splice(nums.indexOf(Math.max(...nums)), 1);
    return(Math.max(...nums))
}
</pre>
discussion을 보다가 이런식으로 코드를 작성하면 되는구나
알게됨 Set을 통해서 저장하고 값을 빼는 식으로 
