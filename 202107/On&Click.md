On(click)과 Click()
=======
1. 같은점
 - 클릭시 이벤트 발생
 
2. 차이점
 - 찾아보니 메모리에 차이가 있다고 한다
 - 아마 가장 중요할것 같은데 동적 생성 처리

예제코드

<pre>
<ul id=TestAll>
  <li>Test1</li>
  <li>Test2</li>
  <li>Test3</li>
</ul>
</pre>

위와 같이 작성된 코드가 있고

<pre>
$("#TestAll").children.click(function(){
  $(this).remove();
});
</pre>

위처럼 코드가 실행 된다고 가정하면

클릭시 클릭된 <li>가 remove될것이다
  
<pre>
$("#TestAll").append('<li>New Test</li>');
</pre>
  
이후에 만약 새로운 <li>를 위와 같이 추가하면 최초 선언될 때 생성되지 않았기 때문에 click이벤트가 발생하지 않는다
  
여기서 우리는 .on("click") 이벤트를 사용할 수 있다
  
<pre>
$("#TestAll").on('click', 'li', function(event){
  $(event.target).remove()
});
</pre>

TestAll에 추가되는 태그는 모두 부모의 on('click') 이벤트를 물려 받게 되는 것

다시 추가를 한다면 click 이벤트가 

