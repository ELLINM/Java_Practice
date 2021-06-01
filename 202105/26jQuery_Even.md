Even
====
even은 짝수 인덱스 요소를 선택하는 선택자


문법
-----
~~~
$( ':even' )
~~~

~~~
$( 'p:even' )
~~~
짝수 인덱스 문단을 선택
주의할 점은 첫번째 요소 인덱스가 0부터 시작

예제

~~~
<!doctype html>
<html lang="ko">
  <head>
    <meta charset="utf-8">
    <title>jQuery</title>
    <script src="//code.jquery.com/jquery-1.11.0.min.js"></script>
    <script>
    	$(document).ready(function() {
    		$('li:even').css('color', 'red');
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
~~~
