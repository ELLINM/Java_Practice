Contains
=====
- :contains()는 특정 문자열을 포함한 요소를 선택하는 선택자


문법
-----
~~~
$( ':contains(text)' )
~~~

문자열 포함 여부를 따질 때 대소문자를 구분한다는 점에 주의

~~~
$( 'p:contains("ab")' )
~~~
ab 문자열을 포함한 p 요소를 선택

예문
-----
~~~
<!DOCTYPE html>
<html lang="ko">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width">
    <title>jQuery</title>
    <script src="//code.jquery.com/jquery-3.3.1.min.js"></script>
    <script>
      $(document).ready(function() {
        $('p:contains("jb")').css('color', 'red');
      });
    </script>
  </head>
  <body>
    <p>Lorem ipsum dolor sit amet.</p>
    <p>Aenean nec mollis jb nulla.</p>
    <p>Phasellus JB lacinia tempus mauris eu laoreet.</p>
    <p>Proin gravida velit dictum jbdui consequat.</p>
  </body>
</html>
~~~

jb를 포함한 p 요소를 선택해서 글자를 빨간색으로 작성
첫번째 문단은 jb가 없어서, 세번째 문단은 jb가 있지만 대문자여서 선택되지 

