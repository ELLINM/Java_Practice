Transient
======
Transient는 보안정책 상 패스워드, 주민등록 번호와 같은 개인정보를 직렬화(Serialize) 과정에서 제외하고 싶은 경우에 적용합

그 외에 직렬화 과정 후 데이터를 전송을 하고 싶지 않을 때 선언하여 예외처리

예제

<pre><code>
package com.serialize.test;

import java.io.Serializable;

class Member implements Serializable {
	private String name;
	private String email;
	private int age;
	
	public Member(String name, String email, int age) {
		this.name = name;
		this.email = email;
		this.age = age;
	}
	
	@Override
	public String toString() {
		return String.format("My name is %s, email is %s, age is %d", name, email, age);
	}
}

</code></pre>

<pre><code>
package com.serialize.test;

import java.io.ByteArrayInputStream;
import java.io.ByteArrayOutputStream;
import java.io.IOException;
import java.io.ObjectInputStream;
import java.io.ObjectOutputStream;
import java.io.Serializable;
import java.util.Base64;

public class TransientTest {
	public static void main(String[] args) throws IOException, ClassNotFoundException {
		Member member = new Member("LYN", "first@gmail.com", 33);
		String serialData = toString( member);
		
		System.out.println("Encode serialized version ");
		System.out.println(serialData);
		Member desirializeData = (Member)fromString(serialData);
		System.out.println("Member deSerialized version ");
		System.out.println(desirializeData);
	}
	
	private static Object fromString( String s ) throws IOException, ClassNotFoundException {
		byte [] data = Base64.getDecoder().decode(s);
		ObjectInputStream ois = new ObjectInputStream(new ByteArrayInputStream(data));
		Object o = ois.readObject();
		ois.close();
		return o;
	}
	
	private static String toString(Serializable o) throws IOException {
		ByteArrayOutputStream baos = new ByteArrayOutputStream();
		ObjectOutputStream oos = new ObjectOutputStream(baos);
		oos.writeObject(o);
		oos.close();
		return Base64.getEncoder().encodeToString(baos.toByteArray());
	}
}
</code></pre>

출력

Encode serialized version 
rO0ABXNyABljb20uc2VyaWFsaXplLnRlc3QuTWVtYmVyc/4yojGsmTACAANJAANhZ2VMAAVlbWFpbHQAEkxqYXZhL2xhbmcvU3RyaW5nO0wABG5hbWVxAH4AAXhwAAAAHnQAFWRldi5ub3RmZWFyQGdtYWlsLmNvbXQAA0xZTg==
Member deSerialized version 
My name is LYN, email is first@gmail.com, age is 33


transient 추가

<pre><code>
package com.serialize.test;

import java.io.Serializable;

class Member implements Serializable {
	private transient String name;
	private String email;
	private int age;
	
	public Member(String name, String email, int age) {
		this.name = name;
		this.email = email;
		this.age = age;
	}
	
	@Override
	public String toString() {
		return String.format("My name is %s, email is %s, age is %d", name, email, age);
	}
}
</code></pre>



출력

Encode serialized version 
rO0ABXNyABljb20uc2VyaWFsaXplLnRlc3QuTWVtYmVyEVHKKL8fwmkCAAJJAANhZ2VMAAVlbWFpbHQAEkxqYXZhL2xhbmcvU3RyaW5nO3hwAAAAHnQAFWRldi5ub3RmZWFyQGdtYWlsLmNvbQ==
Member deSerialized version 
My name is null, email is first@gmail.com, age is 33


name에 null값이 출려되지만 필드는 유지되는걸 확인 할수 있음
