Empty
=====
empty는 내용이 없는 빈 요소를 선택하는 선택자

문법

~~~
$( ':empty' )
~~~

~~~
$( 'div:empty' )
~~~
div 요소 중 내용이 없는 것을 선택


예문

~~~
<!doctype html>
<html lang="ko">
<head>
<meta charset="utf-8">
<title>jQuery</title>
<style>
table {
	width: 100%;
}

td {
	border: 1px solid #bcbcbc;
	padding: 20px;
	text-align: center;
}
</style>
<script src="//code.jquery.com/jquery-1.11.0.min.js"></script>
<script>
    	$(document).ready(function(){
    		$('td:empty').append('N/A');
    	});
    </script>
</head>
<body>
	<h1>jQuery Selector - :empty</h1>
	<table>
		<tr>
			<td>Lorem</td>
			<td></td>
			<td>Ipsum</td>
		</tr>
		<tr>
			<td></td>
			<td>Dolor</td>
			<td></td>
		</tr>
	</table>
</body>
</html>
~~~
