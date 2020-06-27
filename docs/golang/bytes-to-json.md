# bytes-to-json

## Keyword

- **encoding/json** package
- nested struct
- struct tag
- Marshal / Unmarshal

Go언어로 백엔드를 개발하다가 HTTP 요청으로 반환된 JSON 데이터를 가공해서 response 해야 하는 상황이 생겼다.
JavaScript 환경에서는 JSON 이 이름 그대로 **J**ava**S**cript**O**bject**N**otation 이기 때문에 원하는대로 조작하기 쉬운데 반해, Go 환경에서는 JSON 을 기본으로 지원하고 있지 않기 때문에 반환된 response 의 구조대로 struct 를 선언하여 HTTP response bytes 를 Unmarshal 해서 사용하는 방식을 이용했다.

## JSON string JSON converting

사용한 HTTP Client 는 resty/v2 이고, 약간 복잡한 GraphQL 요청을 response 받는 것이라서 response json 에 depth 가 좀 있었다. 단일 depth 라면 response 형태대로 struct 를 정의해서 쓰면 되겠으나, 여러 depth 라 struct 를 너무 많이 정의해서 써야 해서 처음부터 nested struct 를 정의해서 쓸 수 있는지 알아봤다.

### Nested struct

아래는 내가 정의한 struct 의 예시이다.

```go
type ResponseType struct {
    Data struct {
        Viewer struct {
            PinnedItems struct {
                Nodes []struct {
                    Name        string `json:"name"`
                    Url         string `json:"url"`
                    Description string `json:"description"`
                } `json:"nodes"`
            } `json:"pinnedItems"`
        } `json:"viewer"`
    } `json:"data"`
}
```

여기서 눈여겨 봐야 할 부분은, struct 의 tag 다.
struct 안에 json 으로 변환을 원하는 field 에 \`json:"propertyName"`\ 형식으로 tag 를 달아줘야 Unmarshal 이 원하는 대로 된다.
처음에 tag 를 달아주지 않아서 json unmarshal 이 원하는 대로 되지 않았는데, [문서](https://golang.org/pkg/encoding/json/#Marshal)를 보니 tag 를 달아줘야 한다고 되어 있었다. 그리고 tag 를 통해 생각보다 많은 일을 할 수 있었다. (Marshal 을 할 때도 tag 의 역할은 동일)