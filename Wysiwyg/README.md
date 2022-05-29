# Wizwig?
- 노션 메모 기능
- 네이버 이메일 작성칸

# How to make?
Hint! 네이버 이메일 하단에 <html>코드를 볼 수 있는 탭이 있다.

텍스트가 들어가는 공간은 &lt;div>이다. 원래 div에는 커서가 동작하지 않음(이 문제를 해결 하는 방법을 알아야 함)
아이콘을 많이 사용할 예정 : 
[fontawesome](https://fontawesome.com/)
- 회원가입 후 kit 만들어야 함
- &lt;head>안에 스크립트 넣기
- [익스텐션](https://marketplace.visualstudio.com/items?itemName=Janne252.fontawesome-autocomplete)

# ToDoList
1. 기본 UI를 제작합니다.
2. 요소가 편집 가능해야합니다.
3. 버튼을 눌렀을 때 뭔가 기능이 동작해야합니다.
    1. 드래그를 통해 우리가 선택한 노드가 무엇인지 파악할 수 있어야합니다. (우리가 선택한 공간이 어딘지 알 수 있어야 한다.)
    2. 기능1 : 선택한 노드의 한 줄이 통으로 스타일이 바뀌는 기능. (커서를 줄에 놓고 효과 버튼을 누르면 한 줄 전체에 스타일이 적용되어야 한다.)
    3. 기능2 : 선택한 노드만 바뀌는 기능(선택한 부분만 효과가 적용되어야 한다)

# Cross-Origin Resource Sharing
[MDN문서 참고](https://developer.mozilla.org/ko/docs/Web/HTTP/CORS)

> 다른 출처의 리소스를 사용하는 것
면접에서 많이 나옴!

crossorigin="anonymous" : 이렇게 가져다 써도 상관없다. 안심하고 사용하라는 뜻

원래는 다른 출처의 리소스를 사용하기 어려웠다. 해커가 해킹 리소스를 만들어서 뿌렸기 때문이다. 그래서 등장한 것이 CORS를 만들었다.

# data-속성
[MDN문서 참고](https://developer.mozilla.org/ko/docs/Learn/HTML/Howto/Use_data_attributes)
> 다른 조작을 하지 않고도, 의미론적 표준 HTML 요소에 추가 정보를 저장할 수 있도록 해준다,

"data-"는 prefix이고 속성 부분은 surfix이다.

```html
<button type="button" data-command="h1"></button>
```
command변수에 &lt;html>에서 선언한 "data-command" 값으로 초기화 한다.
```js
const command = item.dataset.command;
```

# execCommand()
[MDN문서 참고](https://developer.mozilla.org/ko/docs/Web/API/Document/execCommand)
```js
document.execCommand('명령코드','브라우저에서 제공하는 UI를 사용할 것인가?(true/false)','실행할 값(추가로 변수가 필요할 때 사용)')
```
- 두번째 기본UI제공여부를 묻는 매개변수는 불리언 값으로 준다. 다만 IE 기준이기 때문에 false를 준다.

```js
document.querySelectorAll('.options button').forEach(item => item.addEventListener('click', function () {
    const command = item.dataset.command;
    if (command === 'h1' || command === 'h2' || command === 'h3' || command === 'p') {
        document.execCommand('formatBlock', false, command);
    } else {
        document.execCommand(command); //data-command에서 이름을 맞춰주었기 때문에 '명령코드'에 command 이름이 들어가서 바로 적용이 된다. 추가로 변수가 필요하지 않기 때문에 명령어만 보냄
    }
}));
```
아쉽게도 **"execCommand() 지원을 하지 않겠다"**고 말해서 execCommand()를 이용해 직접 만든 Wysiwyg가 언제 망가질지 모른다.

https://w3c.github.io/editing/#execcommand()를 확인하면 수정을 하고는 있다.

# execCommand()를 사용하지 않고 Wysiwyg 만들기
[no_execComend.html에서 확인 가능](./no_execComend.html)
- data-command를 &lt;tag> 이름으로 바꾼다.

## Window.getSelection()
[MDN문서 참고](https://developer.mozilla.org/ko/docs/Web/API/Window/getSelection)

> 유저가 선택한 영역만 selection 객체로 반환

- document.getSelection()과 같다.
- Selection객체를 만드는게 핵심!!

```js
const selectedTxt = window.getSelection();
```

## .anchorNode

> 선택한 영역의 첫 번째 Node를 반환한다.

- anchorNode : 선택한 부분의 시작점이 어떤 노드에 있는지
- focuseNode : 선택한 부분의 끝부분이 어떤 노드에 있는지

아래의 코드로 정보 확인
```js
console.log(selectedTxt);
```

## Selection.getRangeAt()
[](https://developer.mozilla.org/en-US/docs/Web/API/Selection/getRangeAt)

> 선택한 range의 index를 반환

중복 선택이 되지 않음
### selection.rangeCount
보통 0이 반환
음수거나 selection.rangeCount보다 같거나 큰 숫자를 반환하면 에러가 난다.

# 주의할 점
- 사용자가 뭘 넣을지 모르고 아주 자유롭기 때문에 예외처라하기가 엄청 힘들다.
- 시중에 JS로 구현한 것들이 많다.