Nth-child
====

nth-child()는 형제 요소 중 an+b번째 요소들을 선택하는 선택자


문법
----
~~~
$( ':nth-child(an+b)' )
~~~
- a와 b는 상수, n은 변수
- n에는 음이 아닌 정수(0, 1, 2, 3, ···)가 차례대로 대입
- an+b 대신에 even, odd를 사용가능


예문
------
~~~
<!doctype html>
<html lang="ko">
  <head>
    <meta charset="utf-8">
    <title>jQuery</title>
    <style>
      .jb-red {color: red;}
    </style>
    <script src="//code.jquery.com/jquery-1.12.4.min.js"></script>
    <script>
      jQuery(document).ready(function(){
        $('ol li:nth-child(2)').addClass('jb-red');
      });
    </script>
  </head>
  <body>
    <ol>
      <li>One</li>
      <li>Two</li>
      <li>Three</li>
      <li>Four</li>
      <li>Five</li>
      <li>Six</li>
      <li>Seven</li>
      <li>Eight</li>
      <li>Nine</li>
      <li>Ten</li>
    </ol>
  </body>
</html>
~~~~
