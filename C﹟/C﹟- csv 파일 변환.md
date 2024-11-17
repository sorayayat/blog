문자 형태의 데이터를 다루기 위해 xlsx, csv 등 다양한 파일 형식을 읽고 써보자


1. 클라이언트 요청
2. DB에 저장되어 있는 데이터를 조회한다
3. 서버에 메소드 구현으로 조회한 데이터는 쓴다.
4. 클라이언트에게 전송 및 다운로드가 가능하게 한다

---

```Generator class
public async Task<string> CreateCSVfile(List<data> data)
{
	var writer = new StringBuilder();

	writer.AppendLine("속성1, 속성2"); // 첫줄에 표현할 애트리뷰트를 한줄 넣자

	foreach (var c in data)
	{
		writer.AppendLine($"{data.a},{data.b}");
	}

	return writer.ToString();
}
```

매개 값으로는 조회해서 넘겨주는 데이터가 들어올 예정이다.
스트링빌더를 통해 데이터를 쓴다
foreach 반복문을 통해 데이터를 쓴다. 다루는 데이터에 따라서 중첩 반복문 등을 사용해서 쓰자!
그리고 string 형으로 리턴해준다.

초반에 메소드 호출부에서 타입 문제를 다루느라 꽤 애먹었다.
또한 같은 문자열이나 리스트 형식도 Threading이니 뭐니....고생을 좀 했다.

자료 구조에 대한 얇은 지식이 문제인것 같다ㅋ....
전체 로직파악, 프레임워크에서 제공하는 메소드의 동작원리, 자료 구조를 중점으로 생각하면서 구현하도록 하자
