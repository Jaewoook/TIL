# Apply dark mode

웹에 다크모드를 적용해보고 싶어서 관련 내용을 찾아봤다. 어떻게 작동할지 원리가 궁금했는데, meta 태그나 javascript API 가 존재할 줄 알았지만 생각 외로 CSS Media Query 를 이용해서 system color mode 를 알아내는 방법을 사용하고 있었다.

## dark mode Keyword

키워드는 **prefers-color-scheme** media query 이다.

```css
@media (prefers-color-scheme: dark) {
    /* dark mode css */
}

@media (prefers-color-scheme: light) {
    /* light mode css */
}
```

브라우저가와 시스템이 prefers-color-scheme media query 를 지원한다면 위 CSS 안에 다크모드 컬러를 추가하면 된다.

## 컬러 모드 유연하게 적용하기

prefers-color-scheme 관련 내용을 찾아보다가 나름 Best Practice 같은 것을 발견했다.
바로 <html> 이나 <body> 태그의 class 로 darkmode 같은 class 를 추가하는 것이다.

### JavaScript MediaQuery API

```js
window.matchMedia("(prefers-color-scheme: dark)")
```

자바스크립트에서도 `matchMedia` 라는 함수로 Medai Query 결과를 알 수 있다. 이것으로 페이지가 로드될 때 동적으로 color scheme 을 적용해주고, dark mode enable/disable 처리를 해주면 그때그때 최상위 태그의 class 를 바꿔주면 유연하게 웹의 컬러 모드를 변경할 수 있을 것 같다.