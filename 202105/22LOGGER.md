LOGGER
======
Log란 시스템 동작 시 시스템 상태, 작동 정보를 시간의 경과에 따라 기록한 것
그리고 Logging이란 정보를 제공하는 일련의 기록인 Log를 생성하도록 시스템을 작성하는 활동
저장된 Log는 사용자의 패턴이나 시스템 동작 자체의 분석에 사용
해킹이나 침입 등의 사고가 발생한 경우 비정상 동작의 기록을 통해 감사 추적을 수행 가능

pom.xml에
~~~
  <dependencies>
    <dependency>
	    <groupId>log4j</groupId>
		  <artifactId>log4j</artifactId>
		  <version>1.2.17</version>
	  </dependency>
  </dependencies>
~~~
를 추가해준다.
검색해보고 <dependency>만 추가했더니 오류가 나서
<dependency>는 <dependencies> 하위로 추가해 줘야 한다는것을 알았다.
없으면 추가해줄것
  
pom.xml이 없으면 maven도 추가해서 설정을 해줘야 한다.
  

logger예문
------

<pre><code>
package LogTest;

import java.util.logging.Level;
import java.util.logging.Logger;

public class LogTest {
	private final static Logger LOGGER = Logger.getGlobal();
	
	public static void main(String[] args) {
		LOGGER.setLevel(Level.INFO);
		
		LOGGER.severe("severe Log");
		LOGGER.warning("waring Log");
		LOGGER.info("info Log");
	}
}
</code></pre>
  
- 출력

>5월 22, 2021 4:42:44 오후 LogTest.LogTest main
>
>SEVERE: severe Log
>
>5월 22, 2021 4:42:44 오후 LogTest.LogTest main
>
>WARNING: waring Log
>
>5월 22, 2021 4:42:44 오후 LogTest.LogTest main
>
>INFO: info Log


직접 만든 형식을 원한다면 Formatter 클래스를 상속받아 다음과 같은 클래스를 정의하여

직접 포멧을 만들어 log를 출력할 수 있다
  
예문
  
<pre><code>
package LogTest;

import java.text.SimpleDateFormat;
import java.util.Date;
import java.util.logging.Formatter;
import java.util.logging.Handler;
import java.util.logging.LogRecord;

public class LogTestForm extends Formatter {
	public String getHead(Handler h) {
		return "Start Log\n";
	}
	
	public String format(LogRecord rec) {
		StringBuffer buf = new StringBuffer(1000);
		
		buf.append(calcDate(rec.getMillis()));
		
		buf.append("[");
		buf.append(rec.getLevel());
		buf.append("]");
		
		buf.append("[");
		buf.append(rec.getSourceMethodName());
		buf.append("]");
		
		buf.append(rec.getMessage());
		buf.append("\n");
		
		return buf.toString();
	}
	
	public String getTail(Handler h) {
		return "END Log\n";
	}
	
	private String calcDate(long millisecs) {
		SimpleDateFormat date_format = new SimpleDateFormat("yyyy-mm-dd HH:mm");
		Date resultdate = new Date(millisecs);
		return date_format.format(resultdate);
	}
}
</code></pre>
  
<pre><code>
package LogTest;

import java.util.logging.ConsoleHandler;
import java.util.logging.Handler;
import java.util.logging.Level;
import java.util.logging.Logger;

public class LogTest2 {
	private final static Logger LOG = Logger.getGlobal();
	
	public static void main(String[] args) {
		Logger rootLogger = Logger.getLogger("");
		Handler[] handlers = rootLogger.getHandlers();
		if (handlers[0] instanceof ConsoleHandler) {
			rootLogger.removeHandler(handlers[0]);
		}
		
		LOG.setLevel(Level.INFO);
		
		Handler handler = new ConsoleHandler();
		LogTestForm formatter = new LogTestForm();
		handler.setFormatter(formatter);
		LOG.addHandler(handler);
		
		LOG.severe("severe log");
		LOG.warning("warning log");
		LOG.info("info log");
	}
}
</code></pre>
  
출력
  
>Start Log
>  
>2021-32-22 17:32[SEVERE][main]severe log
>  
>2021-32-22 17:32[WARNING][main]warning log
>  
>2021-32-22 17:32[INFO][main]info log
