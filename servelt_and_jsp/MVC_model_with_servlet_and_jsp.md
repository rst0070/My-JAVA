# JSP & Servelt으로 MVC를 구현하는 방법은??

## 1. 직접 구현하기
<img src="assets/MVC_model_with_servlet_and_jsp_diagram.png"></img>

1. Client가 request보냄.
2. Controller(servlet)가 request를 받고 DB작업을 통해 model을 생성
3. model에 id등을 부여하여 jsp가 해당 모델을 이용할 수 있게한다.
4. `redirect`등을 사용해 jsp가 해당 모델을 갖고 페이지를 생성하도록 한다.

## 2. ReuestDispatcher 사용하기 + Request.setAttribute
`javax.servlet.RequestDispatcher`를 사용하면 controller에서 받은 
`ServletRequest`와 `ServletResponse`를 여러다른 소스(jsp, html , servlet 등)로 넘겨줄수 있다.  
이를 이용하여 ServletRequest.setAttribute를 통해 값을 전달가능.
```java
//Controller
public void do...(HttpServletRequest req, HttpServeltResponse res){
    //setAttribute를 통해 모델 구현 가능
    req.setAttribute("model1", new Object(){
        ...
    });
    //req와 res받을 view지정
    RequestDispatcher rd = req.getRequestDispatcher("view.jsp");
    //전달하기
    rd.forward(req, res);
}
```

## 출처, 참고
* [youtube: telusko](https://youtu.be/MDHj4vgKY6Q)
## 생각해보기
* _model을 전달할때 어떤 방식으로 모델을 식별하고 저장하는것이 효율적일까?_
* _Spring을 사용할때 편리해지는 부분들?_
