# Pagenation

데이터 API 사이트 : [RANDOM USER GENERATOR](https://github.com/youth-gallery/youth-gallery/pull/51)

주소: https://randomuser.me/api/

Requesting Multiple Users : https://randomuser.me/api/?results=5000

## 여러가지 기능들

Seeds: https://randomuser.me/api/?seed=foobar

- 랜덤하지 않고 원하는 데이터를 고정해서 사용함
- 난수에서 사용하는 초기값?
- 시드 값을 기준으로 랜덤한 값을 가져옴
- 해당 데이터를 특정
- 난수를 생성하는 알고리즘은 종류가 정말 많은데 어떤 알고리즘도 완벽하지 않아서 난수를 엄청많이 생성했을 때 분석해보면 특정한 패턴이 보이게 된다. 즉, 완전히 랜덤한 값을 주기 어렵다는 말이다. 이때, 외부에서 특정할 수 없는 나만의 시드값을 만들어 둔다면 정말 랜덤한 수를 만들 수 있는 것이다.
- 디버깅 때 많이 사용: 내가 돌려봤을 때 이러한 에러가 났는데 그때 값이 뭐였지?할 때 다시 그 값을 불러 올 수 있다.

Pagination: https://randomuser.me/api/?page=3&results=10&seed=abc

## 우리가 알아야 할 데이터

1. 첫 번째로 보여질 데이터는 무엇인가? `startPost`
2. 전체의 페이지는 몇 개인가?
3. 보여지는 데이터가 몇 번째 데이터인가?

- 데이터가 헛돌지 않게 해야한다~

### 데이터는 페이지에 어떻게 나타나는가?

- 첫번째 페이지의 첫번째 데이터 번호는 1
- 두번째 페이지의 첫번째 데이터 번호는 11
- 세번째 페이지의 첫번째 데이터 번호는 21

### 페이지마다 화면에 마지막으로 보여질 사람의 데이터가 몇번째인지 특정

```js
let endPost =
  startPost + dataObj.datasPerPage > dataObj.datas.length
    ? dataObj.datas.length
    : startPost + dataObj.datasPerPage;
```

### pagenation Maker

```js
function paginationMaker(totalPages) {
  pagenation.innerHTML = " ";
  const container = document.createDocumentFragment();

  for (let i = 0; i < totalPages; i++) {
    const item = document.createElement("li");
    item.textContent = i + 1; //pagenation은 숫자 1부터 시작하기 때문에 1을 더해줌
    item.addEventListener("click", () => {
      loadPage(i + 1);
    });

    container.append(item);
  }
  pagenation.append(container);
}
```

> 페이지네이션 라이브러리: https://pagination.js.org/
