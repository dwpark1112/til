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

## Webpack

모듈 빌드 도구 였던가? 7일전에 공부했는데 또 까먹었네

