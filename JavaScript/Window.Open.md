### **Window.Open**

: Window.Open은 지정한 URL을 새로운 창 혹은 현재 창에 불러오고 해당 Window 객체를 반환한다.

: Javascript "Window" 객체의 open() 함수를 사용한다.

 

**문법**

> **window.open(url, name[, specs][, replace]);
> *** 대괄호의 속성은 선택사항
>
> 1. url : 새로운 창에 보여질 URL 주소, 값을 비워두면 빈창(about:blank)
>
> 
>
> 2. name : 새로운 창의 속성을 지정하거나 창의 이름을 지정한다. 기본값은 "_blank"
>
>   \* 속성 종류
>     \- _blank : 새로운 창에 열린다. 기본값이다.
>     \- _parent : window.open을 호출한 부모창에서 열린다.
>     \- _self : 현재 페이지를 대체한다.
>     \- _top : 로드된 프레임셋을 대체한다.
>     \- 특정 이름(문자열) : 새창이 열리고 해당 창의 이름을 지정한다.
>
> 
>
> 3. specs : 새로 열릴 창의 크기와 기능 속성을 지정한다.
>
>   \* 속성 종류
>      \- channelmode=yes|no|1|0 : 전체화면으로 창이 열립니다. IE에서만 동작합니다.
>     \- directories=yes|no|1|0 : (사용되지 않습니다.) 디렉토리 버튼의 표시여부
>     \- fullscreen=yes|no|1|0 : 전체 화면 모드. IE에서만 동작합니다.
>     \- height=pixels : 창의 높이를 지정합니다.(height=600)
>     \- width=pixels : 창의 너비를 지정합니다.(width=500)
>     \- left=pixels : 창의 화면 왼쪽에서의 위치를 지정합니다. 음수는 사용할 수 없습니다.
>     \- top=pixels : 창의 화면 위쪽에서의 위치를 지정합니다. 음수는 사용할 수 없습니다.
>     \- location=yes|no|1|0 : 주소 표시줄 사용여부를 지정합니다. Opera에서만 동작합니다.
>     \- menubar=yes|no|1|0 : 메뉴바 사용여부를 지정합니다.
>     \- resizable=yes|no|1|0 : 창의 리사이즈 가능 여부를 지정합니다. IE에서만 동작합니다.
>     \- scrollbars=yes|no|1|0 : 스크롤바 사용여부를 지정합니다. IE, Firefox, Opera에서 동작합니다.
>     \- status=yes|no|1|0 : 상태바를 보여줄지 지정합니다.
>     \- titlebar=yes|no|1|0 : 타이틀바를 보여줄지 지정합니다.
>           호출 응용 프로그램이 HTML 응용 프로그램이거나 신뢰할 수있는 대화 상자가 아니면 무시됩니다.
>     \- toolbar=yes|no|1|0 : 툴바를 보여줄지 지정합니다. IE, Firefox에서 동작합니다.
>
> 
>
> 4. replace : 히스토리 목록에 새 항목을 만들지 현재 항목을 대체할지 지정한다.
>      \* 속성 종류
>        \- true : 현재 히스토리 내용을 새 히스토리로 대체한다.
>        \- false : 히스토리에 새 항목을 추가합니다.

 

**예)**

var newWindow = window.open('/newWindow.html', '_blank', 'width=500, height=600, location=no');

 