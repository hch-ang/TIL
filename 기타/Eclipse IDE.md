# Eclipse IDE

### 기본 설정

- `Workspace` : 작업 공간. 처음 이클립스를 시작하면 workspace를 설정하며, workspace로 설정된 폴더 안에는 `.metadata` 폴더가 새로 생긴다. `.metadata`는  해당 `preference`, `plug-in`, 등 해당 workspace에서 설정한 환경들을 저장한다. workspace를 옮기거나 새로 만들면 이러한 설정들이 초기화된다.
- `preference` : General, Ant, Java, Java EE, Server, Terminal, 등 많은 환경과 항목에 대한 설정을 할 수 있다. `encoding`을 검색하여 적당한 인코딩 방식을 설정할 수 있다. 이클립스의 `Window`-`preference`로 들어가서 설정할 수 있다.
- `perspective` : 용도에 맞는 editor, view, project browser, 등을 설정할 수 있다. 자바 프로그래밍을 위한 perspective를 설정하려면, `window`-`perspective`-`open perspective`에 들어가서 Java를 찾아서 누르면 된다. 



### 단축키

- `Ctrl` + `F11`

  Run

- `Alt` + `S` `R` `A` `R`

  클래스 내에서 각 변수들을 캡슐화를 목적으로(private) 만든 후, get메서드와 set메서드를 자동으로 생성시켜주는 단축키

- `ctrl` + `shift` + `O` 

  전체 코드에 필요한 import를 다 해주는 단축키.

- `ctrl` + `D`

  줄 지우기

- `ctrl`+`alt`+(`위`또는`아래`)

  코드 복사

- (원하는 코드 드래그 후) `shift` + `alt` + `m`

  해당 코드를 메서드로 따로 추출(보통 출력같은 반복되는 코드를 메서드로 빼기위해 활용)



### Plugin

Eclipse에서는 기존의 기능 뿐만 아니라 활용할 수 있는 다양한 기능들이 있는데, 이를 Plugin이라고 한다. Plugin을 사용하기 위해서는,

1. Help -> Eclipse Marketplace에서 다운받기

2. 인터넷에서 해당 Plugin을 다운받은 후

   2-1 eclipse 설치 폴더 -> Plugins폴더에 넣기

   2-2 Help -> Install new software -> 다운받은 Plugin 검색 후 설치