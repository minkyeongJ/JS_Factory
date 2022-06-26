# Canvas API?
[MDN문서](https://developer.mozilla.org/ko/docs/Web/API/Canvas_API)
- canvas : 도화지, 도화지 위에다 2차원의 비프맵 그림을 그리는 API
- DOM을 이용하지 않음
- 처음부터 웹 표준이 아니었음
- Apple에서 처음 만듦
- flash player와 유사함
  - flash의 단점은 브라우저에 따로 설치를 했어야 한다는 것인데 canvas는 설치가 필요없음
## SVG VS Canvas
  - 복잡해 질 수록 용량이 커지기때문에 용량이 작은 Canvas 사용 추천
  - XML, CSS, JS 3개를 다 이용해야 하지만 Canvas는 JS만 있으면 된다

## 기존에 존재하는 이미지 요소로 웹 브라우저에 이미지를 표현할 수 있는데 Canvas는 왜 필요한가?
- 이미지 요소로 이미지가 렌더링 되게 되면 CSS를 이용해 이미지의 크기나 Border를 추가할 수 있고, 경로를 바꾸면 다른 이미지를 불러올 수 있지만... 안에 있는 이미지 자체를 수정할 수 없다.
- Canvas는 동적으로 이미지를 생성하고 수정할 수 있는 환경을 제공한다는 점에서 이미지 같은 태그와 확실히 결이 다르다.
- 즉, 이미지 파일이 없어도 원하는 이미지를 나타낼 수 있다.

# How to make?
- ```<canvas></canvas>``` 태그를 사용
- 오직 JS만 지원
- 2d: ```canvas.getContext('2d')```
- 3d: ```canvas.getContext('webgl')```
  - [WebGL MDN](https://developer.mozilla.org/ko/docs/Web/API/WebGL_API/Tutorial/Getting_started_with_WebGL)
  - 별도의 설치가 필요없음
  - 복잡한 3D게임도 동작가능
- reflow가 발생하지 않기 때문에 애니매이션을 나타낼때 좋다!
- 비트맵 기반 API이기 때문에 기본 단위는 px임. 그래서 단위 생략해서 작성.

# 함수
## 사각형 만들기
- ```ctx.fillRect(x좌표, y좌표, x축에서 몇픽셀 넓이, y축에서 몇픽셀 넓이)```
- 사각형을 하나 더 만들고 싶으면? ```ctx.fillRect(x좌표, y좌표, x축에서 몇픽셀 넓이, y축에서 몇픽셀 넓이)```를 다시 써주면 된다.
- 스타일 변경: ```ctx.fillStyle = 'red'```
  - 스타일은 도형을 그리기 전에 선언해줘야함

## 선 그리기
  ``` js
      ctx.strokeStyle = "blue";
      ctx.lineWidth = "30";
      ctx.beginPath(); //나 이제 선 그릴거임
      ctx.moveTo(x좌표, y좌표);//시작 지점 지정
      ctx.lineTo(x좌표, y좌표);//끝 지점 지정
      ctx.stroke();//그려라!
  ```

## 원그리기
  - 시작 각도, 끝 각도를 알려줘야 함
  - 단위: radian
    - 0부터 시작
    - 3.14 == 180도
    - 3.14 * 2 == 360도
    - 3.14 / 2 == 90도
  - 오른 쪽 가장자리가 defalt 시작지점
  ``` js
      ctx.lineWidth = 30;//선 굵기
      ctx.beginPath();//나 선그릴거임~
      ctx.arc(원의 중심 x축, 원의 중심 y축, 반지름의 길이, 시작 radian 값, 끝나는 radian 값, 어느 방향으로 그릴것인가? 반시계방향 : 시계방향);//원의 좌표
      ctx.stroke();//그려라!
      ctx.fill();//채우기
      ctx.fillStyle = "black";//채우기 색 설정
  ```
## 애니메이션 : 로딩 바
- 부드러운 애니매이션을 위한 ```requestAnimationFrame```
  - 어떤 동작이 있을지 미리 알려주어 동작을 부트럽게 해주는 메소드
  - 재귀함수를 이용해 동작하기