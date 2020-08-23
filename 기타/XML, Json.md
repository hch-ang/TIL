# XML, Json

### `XML`

`Markup Language` : 태그 등을 이용하여 문서나 데이터의 구조를 서술하는 언어로 SGML, HTML 등이 있다. SGML은 국제표준이지만 복잡하여 조금 더 쉽게 데이터의 표현에 집중하도록 나온 것이 HTML이다.

SGML은 너무 복잡하고 HTML은 Web 환경에서 데이터의 표현 등 이미 확장이 어려운 고유 영역의 언어로서, 보다 범용적으로 데이터의 저장, 교환 등에 목적을 둔 언어가 필요했다.

그래서 나온 언어가 `XML(eXtensible Markup Language)`이다. XML은 언어와의 독립성을 유지하며, 어느 언어에서도 사용이 가능하다.

HTML과 달리 미리 정의된 tag도 없고, 표현에 의존적이지 않다.필요에 따라 tag를 정의하여 사용할 수 있으므로 확장에 용이하고, 다양한 데이터를 표현할 수 있다. 약속된 tag 및 구조를 미리 정의한 후, 그에 맞게 XML 문서를 만들고 공유한다.

ebXML(Electronic Business XML / 전자상거래), MathML(수학공식 표현), SVG(Scalable vector Graphics / 이미지 표현) 등이 XML 기반의 확장된 Markup Language이다.

XML 문서 구조 및 간단한 문법을 잘 따른 XML문서를 `well formed XML`이라고 한다.

XML에서는 중복되는 tag가 발생할 수 있다. 이를 해결하기 위해 사용 tag에 대한 preffix와 그 prefix에 대한 Namespace를 사용한다.

<tag></tag> 와 <tag></tag> 중복

-> <prefix1 :tag> </prefix1 :tag>와 <prefix2 :tag> </prefix2 :tag>



이클립스에서 빨간색 경고 : well-formed 유무

노란색 경고 : valid 유무

xml문서가 Valid 문서가 되깅 위해서는 각 tag 및 각 해당 tag의 유효값을 별도의 문서로 구성한 후 명시적으로 지정해 주어야 한다. 이 파일을 `DTD(Document Type Definition)`이라고 한다.

xml문서에 DTD를 명시하고 문서의 모든 요소가 DTD를 만족하면 비로소 문서가 `valid`하다.

```xml
<!DOCTYPE <파일명> SYSTEM "<파일명.dtd>">
```



### XML Parsing

일정한 구조로 작성되어 있는 XML문서에서 원하는 정보를 찾아내는 것을 `parsing`이라고 한다. Java에서는 `javax.xml` package, `org.w3c` package를 활용하여 xml파일을 파싱한다.

파싱의 종류는 `SAX`, `DOM` 두가지가 있다.

- `SAX`

  문서를 처음부터 끝까지 순서대로 읽으면서 tag를 만날 때마다 실행되는 event를 기반으로 처리한다. event를 처리하기 위해 별도의 `Handler`를 가지고 있다. 한번 쭉 순서대로 읽으면 끝나기 때문에 빠른 탐색이 가능하다. 단순한 구조를 가진 많은 양의 xml 데이타를 빠르게 처리하는데 유용하다. (코드가 마치 stasck처럼 작용하는 느낌, pop되면서 해당 tag의 내용을 저장한다)

- `DOM`

  문서 전체를 JVM에 올려서 여기저기 탐색하면서 필요한 정보를 처리한다. 다양한 탐색이 가능하지만 느리고 무겁다. 문서의 양은 적지만 tag가 많고 depth가 깊은 복잡한 xml 파일에 유용하다.



### `Json(JavaScript Object Notation)`

XML파일 특성상, 정보를 제대로 전달하기 위해서는 많은 `<tag>`가 필요하다. 원하는 데이터의 크기에 비해 부수적인 태그데이터가 많이 필요하기 때문에 비효율적이다. 이를 해결하기 위해 나온 형식이 json이다. 

