# Servlet tutorial
  
1. [directory 구조](#link1)
2. Servlet이 무엇인가?
3. Life cycle of Servlet
4. web.xml에 servlet mapping
5. request에 대해
6. response에 대해
7. Filter
8. Exception처리
9. Cookies
10. Session

## 1. Directory구조: maven webapp<a name="link1"></a>
```
<project>
|--pom.xml
|--target
|--src
    |--test
    |   |--java
    |   |--resources
    |--main
        |--java
        |--resources
        |--webapp
             |--static
             |--WEB-INF
```
* `pom.xml` - maven 설정파일
* `src/main/java` - java 소스파일들
* `src/test/java` - 테스트용 java 소스파일 보통 `src/main/java`의 소스들과 같은 패키지로 사용
* `resources` - properties나 기타 xml설정 파일등 저장용도
* `webapp` - web application 설정파일
    * `/static` - css, img, js 파일등
    * `/WEB-INF` - 웹 앱 정보 (context.xml, web.xml 등등..) 및 jsp파일
* `target` - 빌드결과
* `WEB-INF/web.xml` - deploy descriptor : was가 실행될때 webapp의 설정 알려줌.

## 2. Servlet의 이해
Servlet은 하나의 객체인데 