Button
======
- type이 button인 요소를 찾는것


문법
-----
~~~ 
$( ':button' )
~~~

type이 button인 모든 요소를 선택

~~~
$( '.xy:button' )
~~~

type이 button이면서 class 값으로 xy를 갖는 요소를 선택

예제
~~~
<!DOCTYPE html>
<html lang="ko">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width">
    <title>jQuery</title>
    <script src="//code.jquery.com/jquery-3.3.1.min.js"></script>
    <script>
      $( document ).ready( function() {
        $( ':button' ).css( 'font-style', 'italic' );
        $( '.ab:button' ).css( 'color', 'red' );
      });
    </script>
  </head>
  <body>
    <input type="button" value="Button 1" class="ab">
    <input type="button" value="Button 2" class="cd">
  </body>
</html>
~~~
