---
layout: default
title: SpringMVC
parent: Web_Back
nav_order: 7
---

# SpringMVC

---

## MVC Pattern

#### Model

- 동작을 수행하는 코드
- 사용자 View에 어떻게 보일지에 대해서 신경쓰지 않는다.
- 데이터 질의에 대한 정보를 제공하는 기능 및 데이터에 대한 수정을 담당한다.

#### View

- 사용자가 화면에 무엇을 어떻게 볼 것인지를 결정한다.
- 사용자 화면에 보이는 부분이다.
- 모델의 정보를 받아와 사용자에게 보여주는 역할을 수행한다.
- 자체적으로 모델의 정보를 보관하지 않는다.

#### Controller

- 모델과 뷰를 연결하는 역할을 수행한다.
- 사용자에게 데이터를 가져오고 수정하고 제공한다.

## Spring Web MVC

#### Spring Web MVC

- Servlet API를 기반으로 구축된 웹프레임워크다.
- 정식 명칭은 Spring Web MVC이지만, Spring MVC로 알려져있다.
- Spring Framwork이 제공하는 DI, AOP 뿐 아니라, WEB개발을 위한 기능을 제공한다.
- DispatcherServlet(FrontController)를 중심으로 디자인 되었으며, View Resolver, Handler, Mapping, Controller와 같이 객체와 함께 요청을 처리하도록 구성되어 있다.
- 다른 프레임워크와 마찬가지로 front controller pattern으로 구성된다.
- 중심이 되는 DispatcherServlet(front controller)은 요청처리를 위한 기능을 제공한다.

#### 컨테이너 구성

- DispatcherServlet은 Servlet WebApplicationContext와 Root WebApplicationContext로 구성된다.
- Servlet WebApplicationContext: controllers, viewResolver, HandlerMapping 등 웹과 직접관련있는 요소가 담긴다.
- Root WebApplicationContext: Service, Repositories 등 그외의 것이 담긴다.

#### Spring MVC 구성요소

- DispatcherServlet: 클라이언트 요청처리(요청 및 처리 결과 전달)한다.
- HandleMapping: 요청을 어떤 Controller가 처리할지 결정한다.
- Controller: 요청에 따라 수행할 메서드를 선언하고, 요청처리를 위한 로직을 수행한다.(비즈니스 로직 호출)
- ModelAndView: 요청처리를 하기 위해서 필요한 혹은 그 결과를 저장하기 위한 객체다.
- ViewResolver: Controller에 선언된 view이름을 기반으로 결과를 반환할 View를 결정한다.
- View: 응답화면을 새성한다.

#### Spirng MVC: 요청 처리 흐름

- 클라이언트 요청이 들어오면 DispatcherServlet이 받는다.
- HandleMapping이 어떤 Controller가 요청을 처리할지 결정한다.
- DispatcherServlet은 Controller에 요청을 전달한다.
- Controller는 요청을 처리한다.
- 결과(요청처리를 위한 data, 결과를 보여줄 view의 이름)를 ModelAndView에 담아 반환한다.
- ViewResolver에 의해서 실제 결과를 처리할 View를 결정하고 반환한다.
- 결과를 처리할 View에 ModelAndView를 전달한다.
- DispatcherServlet은 View가 만들어낸 결과를 응답한다.

#### Controller

- @RequestMapping
- URL을 클래스 또는 특정 핸들러(메서드)에 매핑한다.
- 일반적으로 클래스에 작성하는 @RequestMapping은 요청경로, 혹은 요청 패턴에 매칭한다.
- 메서드 Annotation은 요청방식(GET, POST) 등으로 범위를 좁힌다.
- HttpServletRequest, HttpServletResponse, HttpSession: Servlet API를 사용할 수 있다.
- Locale: 요청 클라이언트의 Local 정보를 포함한다.
- InputStream, Reader OutputStream, Writer: 요청으로부터 직접 데이터를 읽어오거나, 응답을 직접 생성하기 위해서 사용한다.
- Map, Model, ModelMap: View 데이터를 전달하기 위해서 사용한다.
- RedirectAttributes: 리디렉션(쿼리 문자열에 추가) 시 사용할 속성을 지정한다.
- Errors, BindingResult: 에러와 데이터 바인딩 결과에 접근하기 위해서 사용한다.
- @PathVariable: URI 템플릿 변수에 대한 액세스
- @RequestParam: multipart 파일을 포함하여 요청 파라미터에 액세스
- @RequestHeader: 요청 헤더에 액세스
- @CookieValue: 쿠키에 대한 액세스
- @RequestAttribute, @SessionAttribute: 모든 세션 속성에 대한 액세스, 요청 속성에 액세스
- @ModelAttribute: 모델의 속성에 액세스
- @ResponseBody: HttpMessageConverter 구현을 통해 변환되어 응답한다.
- HttpHeaders: 헤더가 있고 body가 없는 response를 반환한다.
- String: 뷰 이름을 반환한다. (ViewResolver 선언과 함께 사용한다.)
- View: 렌더링 하는데 사용할 View 인스턴스다.
- Map, Model: 명시적으로 모델을 작성하지 않은 경우 사용한다.
- ModelAndView: 사용할 view와 속성을 포함하는 객체다.
- void: method에 ServletResponse, HttpServletResponse인자가 있는 경우, 모든 요청이 처리된 것으로 간주한다. 그렇지 않으면 요청 URI를 view name으로 처리한다.
