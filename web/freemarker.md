# WEB 해보자

## FreeMarker

Java 서블릿을 위한 오픈소스 HTML 템플릿 엔진

`[*.java]Servlet + Template[*.ftl] = Output[*.html]`

데이터 모델을 그대로 템플릿으로 떨어트려서 사용하므로 직관적이며 명확하다. 그 외의 템플릿 엔진은 안써봐서 모르겠다.

## WebJar

포스팅에 다 적혀 있음 <http://millky.com/@origoni/post/1147?language=ko_kr>

- <http://www.webjars.org/bower>

## BootStrap

예제보면서 하면 될듯... <https://www.w3schools.com/bootstrap>

## 자동 리로드 설정

JLabel 없어도 되는건가? <http://haviyj.tistory.com/11>  
생각보다 잘되서 놀랍다. 원래 좀 막히고 잘 안되고, 뭔가는 되고 안되고 그래야 되는데... 

## Ajax how to

대략 이런 식인데, 더 많이 써봐야겠음

```javascript
        function callLoginAPI() {
            var user = $('#user').val();
            var password = $('#pwd').val();
            var json = {"user": user, "password": password};
            $.ajax({
                url: "/login",
                type: 'POST',
                data: JSON.stringify(json),
                cache: false,
                beforeSend: function (xhr) {
                    xhr.setRequestHeader("Accept", "application/json");
                    xhr.setRequestHeader("Content-Type", "application/json");
                },
                success: function (response) {
                    // do
                },
                error: function (jqXhr, textStatus, errorThrown) {
                    // do
                }
            });
            return true;
        }
```

webpack 했더니만, `Error: Cannot find module 'webpack'` 이렇게 나온다면? `npm link webpack` 해주면 됨 

제대로 된다면 아래처럼 나온다.

```shell
Hash: 102f97f2a736f72019db
Version: webpack 2.6.0
Time: 555ms
                         Asset     Size  Chunks             Chunk Names
              sample.bundle.js  2.31 kB       0  [emitted]  sample
               index.bundle.js  2.34 kB       1  [emitted]  index
         domain.list.bundle.js  3.44 kB       2  [emitted]  domain.list
    domain.dmsDetail.bundle.js  5.18 kB       3  [emitted]  domain.dmsDetail
              vendor.bundle.js     6 kB       4  [emitted]  vendor
          sample.bundle.js.map  3.95 kB       0  [emitted]  sample
           index.bundle.js.map     4 kB       1  [emitted]  index
     domain.list.bundle.js.map  6.66 kB       2  [emitted]  domain.list
domain.dmsDetail.bundle.js.map  10.6 kB       3  [emitted]  domain.dmsDetail
          vendor.bundle.js.map  6.07 kB       4  [emitted]  vendor
   [0] ./src/main/resources/static/js/source/modules/utils.js 1.17 kB {0} {1} {2} [built]
   [1] ./src/main/resources/static/js/source/domain/dmsDetail.js 4.92 kB {3} [built]
   [2] ./src/main/resources/static/js/source/domain/list.js 1.6 kB {2} [built]
   [3] ./src/main/resources/static/js/source/index.js 427 bytes {1} [built]
   [4] ./src/main/resources/static/js/source/sample.js 432 bytes {0} [built]
   [5] multi ./src/main/resources/static/js/source/domain/dmsDetail.js 28 bytes {3} [built]
   [6] multi ./src/main/resources/static/js/source/domain/list.js 28 bytes {2} [built]
   [7] multi ./src/main/resources/static/js/source/index.js ./src/main/resources/static/js/source/modules/utils.js 40 bytes {1} [built]
   [8] multi ./src/main/resources/static/js/source/sample.js ./src/main/resources/static/js/source/modules/utils.js 40 bytes {0} [built]

```

이제 만들어둔 자바스크립트를 번들링해보자

## 개발 일지가 되어간다.

event.stopPropagation(), event.preventDefault () 

- DOM 
  - innerHTML property: way to get the content of an element

- $.ajax 부분에서 missing import 표시될 때
  - https://stackoverflow.com/questions/38207469/webstorm-reports-a-missing-import-on-built-in-objects 