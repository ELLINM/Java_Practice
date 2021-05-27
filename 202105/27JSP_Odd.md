Odd
=====

홀수 인덱스 요소를 선택하는 선택자


문법
---
~~~
$( ':odd' )
~~~

~~~
$( 'p:odd' )
~~~

홀수 인덱스인 p 요소를 선택 주의할 점은 인덱스가 0부터 시작


예문
-----

~~~
<!doctype html>
<html lang="ko">
  <head>
    <meta charset="utf-8">
    <title>jQuery</title>
    <script src="//code.jquery.com/jquery-3.3.1.min.js"></script>
    <script>
      $(document).ready(function(){
        $('li:odd').css('color', 'red');
      });
    </script>
  </head>
  <body>
    <ul>
      <li>0. Lorem</li>
      <li>1. Ipsum</li>
      <li>2. Dolor</li>
      <li>3. Sit</li>
    </ul>
  </body>
  ~~~~
