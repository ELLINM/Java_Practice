has
=====
특정 요소를 포함하는 요소를 선택할 때 사용하는 선택자


문법
---

~~~
$( ':has(selector)' )
~~~
~~~
$( 'p:has(span)' )
~~~
span 요소를 가지고 있는 p 요소를 선택

예문

~~~
<!doctype html>
<html lang="ko">
  <head>
    <meta charset="utf-8">
    <title>jQuery</title>
    <script src="//code.jquery.com/jquery-3.3.1.min.js"></script>
    <script>
      $(document).ready(function(){
        $('li:has(a)').css('border', 'lpx solid red');
      });
    </script>
  </head>
  <body>
    <h1>jQuery Selector - :has()</h1>
    <ul>
      <li>Lorem</li>
      <li><a href="#">Ipsum</a></li>
      <li>Dolor</li>
      <li>Sit</li>
      <li><a href="#">Amet</a></li>
    </ul>
  </body>
</html>
~~~
