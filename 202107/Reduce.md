Reduce()
=======
reduce()메서드는 배열의 각 요소에 대해 주어진 reducer 함수를 실행하고, 하나의 결과값을 반환


구성인자
-----
- 누산기 Accumulator(acc)
- 현재갑 (cur)
- 현재 인덱스 (idx)
- 원본 배열 (src)


매개변수
------
- callback
  - accumulator : callback의 반환값을 누적, callback의 이전 반환값 또는 callback의 첫 번째 호출이며 initialValue를 제공한경우 initialValue의 값
  - currentValue : 처리할 현재 요서
  - currentIndex(Optional) : 처리할 현재 요소의 인덱스. initialValue를 제공한 경우 0 아니면 1부터 시작
  - array(Optional) : reduce()를 호출한 배열
  
- initialValue(Optional) : callback의 최초 호출에서 첫 번째 인수에 제공하는 값 초기값을 제공하지 않으면 배열의 첫번째 요소 사용 빈 배열에서 초기값 없이 reduce()를 호출하면 오류발생


구동원리
----
reduce()는 빈 요소를 제외하고 배열 내에 존재하는 각 요소에 대해 callback 함수를 한 번씩 실행 하는데 callback 함수는 다음 네 인수를 받음

- accumulator
- currentValue
- currentIndex
- array

callback의 최초 호출 시 accumulator와 currentValue는 다음 두가지 값중 하나를 가질 수 있음

1. 만약 reduce()함수 호출에서 initialValue를 제공한 경우, accumulator는 initialValue와 같고 currentValue는 배열의 첫 번째 값과 같음
2. 만약 reduce()함수 호출에서 initialValue를 제공하지 않은 경우, accumulator는 배열의 첫 번째 값과 같고 currentValue는 배열의 두 번째 값과 같음


reduce() 예문
-----
1.
<pre>
var res = [0, 1, 2, 3, 4].reduce(function(accumulator, currentValue, currentIndex, array){
    console.log(`currentIndex : ${currentIndex}`);
    console.log(`accumulator : ${accumulator}`);
    console.log(`currentValue : ${currentValue}`);
    console.log(`array : ${array}`);
    console.log("                              ");
    return accumulator + currentValue;
});

console.log("res:", res);
</pre>

출력
>currentIndex : 1, accumulator : 0, currentValue : 1, array : 0,1,2,3,4
                              
>currentIndex : 2, accumulator : 1, currentValue : 2, array : 0,1,2,3,4
                              
>currentIndex : 3, accumulator : 3, currentValue : 3, array : 0,1,2,3,4
                              
>currentIndex : 4, accumulator : 6, currentValue : 4, array : 0,1,2,3,4
                              
>res: 10

2.
<pre>
var res = [0, 1, 2, 3, 4].reduce(function(accumulator, currentValue, currentIndex, array){
    console.log(`currentIndex : ${currentIndex}`);
    console.log(`accumulator : ${accumulator}`);
    console.log(`currentValue : ${currentValue}`);
    console.log(`array : ${array}`);
    console.log("                              ");
    return accumulator + currentValue;
}, 10);

console.log("res:", res);
</pre>

출력
>currentIndex : 0, accumulator : 10, currentValue : 0, array : 0,1,2,3,4
                              
>currentIndex : 1, accumulator : 10, currentValue : 1, array : 0,1,2,3,4
                              
>currentIndex : 2, accumulator : 11, currentValue : 2, array : 0,1,2,3,4
                              
>currentIndex : 3, accumulator : 13, currentValue : 3, array : 0,1,2,3,4
                              
>currentIndex : 4, accumulator : 16, currentValue : 4, array : 0,1,2,3,4
                              
>res: 20


예제 - 배열의 모든 값 합산
-------
<pre>
var sum = [0, 1, 2, 3].reduce(function (accumulator, currentValue) {
  return accumulator + currentValue;
}, 0);
</pre>

화살표 함수로도 작성 가능
<pre>
var total = [ 0, 1, 2, 3 ].reduce(
  ( accumulator, currentValue ) => accumulator + currentValue,
  0
);
</pre>

출력
> 6


객체 배열에서의 값 합산
-------
객체로 이루어진 배열에 들어있는 값을 합산하기 위해서는 반드시 초기값을 주어 각 항목이 여러분의 함수를 거치도록 해야합

예제
<pre>
var initialValue = 0;
var sum = [{x: 1}, {x:2}, {x:3}].reduce(function (accumulator, currentValue) {
    console.log(`accumulator : ${accumulator}`);
    console.log(`currentValue : ${currentValue.x}`);
    return accumulator + currentValue.x;
},initialValue)

console.log(sum)
</pre>

화살표 함수 작성
<pre>
var initialValue = 0;
var sum = [{x: 1}, {x:2}, {x:3}].reduce(
    (accumulator, currentValue) => accumulator + currentValue.x
    ,initialValue
);

console.log(sum)
</pre>

출력
>accumulator : 0, currentValue : 1

>accumulator : 1, currentValue : 2

>accumulator : 3, currentValue : 3

>6


중첩 배열 펼치기
--------
<pre>
var flattened = [[0, 1], [2, 3], [4, 5]].reduce(
  function(accumulator, currentValue) {
    console.log(`accumulator : ${accumulator}`);
    console.log(`currentValue : ${currentValue}`);
    return accumulator.concat(currentValue);
  },
  []
);
console.log(flattened)
</pre>

화살표 함수
<pre>
var flattened = [[0, 1], [2, 3], [4, 5]].reduce(
  ( accumulator, currentValue ) => accumulator.concat(currentValue),
  []
);
</pre>

출력
>accumulator : , currentValue : 0,1

>accumulator : 0,1, currentValue : 2,3

>accumulator : 0,1,2,3, currentValue : 4,5

>[ 0, 1, 2, 3, 4, 5 ]


객체 내의 값 인스턴스 개수 세기
----------
<pre>
var names = ['Alice', 'Bob', 'Tiff', 'Bruce', 'Alice'];

var countedNames = names.reduce(function (allNames, name) {
  if (name in allNames) {
    allNames[name]++;
  }
  else {
    allNames[name] = 1;
  }
  return allNames;
}, {});

console.log(countedNames);
</pre>


출력
>{ Alice: 2, Bob: 1, Tiff: 1, Bruce: 1 }

for문 과 같은 원리인데 간단하다고 느껴진다


속성으로 객체 분류하기
--------
<pre>
var people = [
  { name: 'Alice', age: 21 },
  { name: 'Max', age: 20 },
  { name: 'Jane', age: 20 }
];

function groupBy(objectArray, property) {
  return objectArray.reduce(function (acc, obj) {
    var key = obj[property];
    if (!acc[key]) {
      acc[key] = [];
    }
    acc[key].push(obj);
    return acc;
  }, {});
}

var groupedPeople = groupBy(people, 'age');

console.log(groupedPeople);
</pre>


출력
>{
  '20': [ { name: 'Max', age: 20 }, { name: 'Jane', age: 20 } ],
  '21': [ { name: 'Alice', age: 21 } ]
}


확장 연산자와 초기값을 이용하여 객체로 이루어진 배열에 담긴 배열 연결하기
-----------
<pre>
var friends = [{
  name: 'Anna',
  books: ['Bible', 'Harry Potter'],
  age: 21
}, {
  name: 'Bob',
  books: ['War and peace', 'Romeo and Juliet'],
  age: 26
}, {
  name: 'Alice',
  books: ['The Lord of the Rings', 'The Shining'],
  age: 18
}];

var allbooks = friends.reduce(function(accumulator, currentValue) {
  return [...accumulator, ...currentValue.books];
}, ['Alphabet']);

console.log(allbooks);
</pre>


출력
> [
  'Alphabet',
  'Bible',
  'Harry Potter',
  'War and peace',
  'Romeo and Juliet',
  'The Lord of the Rings',
  'The Shining'
]


배열의 중복 항목 제거
------
<pre>
let arr = [1, 2, 1, 2, 3, 5, 4, 5, 3, 4, 4, 4, 4];
let result = arr.sort().reduce((accumulator, current) => {
    const length = accumulator.length
    if (length === 0 || accumulator[length - 1] !== current) {
        accumulator.push(current);
    }
    return accumulator;
}, []);
console.log(result);
</pre>


출력
> [ 1, 2, 3, 4, 5 ]


