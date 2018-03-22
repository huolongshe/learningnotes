## 调试ONAP Portal SDK项目

1. 登录URL:

        http://localhost:8080/epsdk-app-os/login.htm   

2. 在整个项目中全文搜索“login.htm”，找到：

        文件：sdk\ecomp-sdk\epsdk-app-common\src\main\java\org/onap/portalapp/controller/core/SDKLoginController.java:71
        
        @RequestMapping(value = { "/login.htm" }, method = RequestMethod.GET)
	    public ModelAndView login() {
		    Map<String, Object> model = new HashMap<>();
		    return new ModelAndView("login", "model", model);
	    }
	    
3. View名称"login"解析：

        文件：sdk\ecomp-sdk\epsdk-core\src\main\java\org\onap\portalsdk\core\conf\AppConfig.java：

        @Bean
	    @Override
	    public ViewResolver viewResolver() {
		    InternalResourceViewResolver viewResolver =     new InternalResourceViewResolver();
		    viewResolver.setViewClass(JstlView.class);
		    viewResolver.setPrefix("/WEB-INF/jsp/");
		    viewResolver.setSuffix(".jsp");
		    viewResolver.setOrder(2);
		    return viewResolver;
		    
    可知应该去找/WEB-INF/jsp目录下的index.jsp文件。
    
4. 文件sdk\ecomp-sdk\epsdk-app-os\src\main\webapp\WEB-INF\jsp\login.jsp中，输入用户名口令后，向后端发起POST请求，URL为“login_external”：

        <form action="login_external" method="POST"> 
        
5. 找到用户登录程序入口：loginStrategy.doExternalLogin

        文件sdk\ecomp-sdk\epsdk-app-common\src\main\java\org/onap/portalapp/controller/core/SDKLoginController.java:
        
        @RequestMapping(value = { "/login_external" }, method = RequestMethod.POST)
	    public ModelAndView doexternalLogin(HttpServletRequest request, HttpServletResponse response) throws IOException {
		    return loginStrategy.doExternalLogin(request, response);
	    }

6. loginStrategy.doExternalLogin以用户输入的用户名口令为参数去数据库中参照匹配的用户对象，成功后重定向至视图“welcome.htm”：

        文件：org/onap/portalsdk/core/auth/LoginStrategy.java:109 
        
        return new ModelAndView("redirect:welcome.htm");

7. 在浏览器中按“F12”打开调试工具，查看welcome.htm请求的响应体：

         <!DOCTYPE html>
        <!-- Single-page application for EPSDK-App welcome page using DS2 look and feel. X-->
        <html>
        <head>
        	<meta charset="ISO-8859-1">
        	<meta http-equiv="X-UA-Compatible" content="IE=edge, chrome=1" />
        	
        	<title>Welcome</title>
        
        	<!-- B2b Library -->
        	<link rel="stylesheet" type="text/css" href="app/fusion/external/b2b/css/b2b-angular/b2b-angular.css">
        	<link rel="stylesheet" type="text/css" href="app/fusion/external/b2b/css/b2b-angular/font_icons.css">    
        ......
        
8. 用上述响应体内容全文搜索，找到内容匹配的文件：

        sdk/ecomp-sdk/epsdk-app-overlay/src/main/webapp/app/fusion/scripts/DS2-view-models/welcome.html 

9. 用上述文件名全文搜索，找到：

        文件：sdk/ecomp-sdk/epsdk-app-overlay/src/main/webapp/WEB-INF/fusion/defs/definitions.xml中105行有定义：
        
        <definition name="welcome" template="/app/fusion/scripts/DS2-view-models/welcome.html" />
        
10. 查看"/app/fusion/scripts/DS2-view-models/welcome.html"文件内容，它应该就是Portal用户登录之后的主入口界面。其中包含JavaScript写的AngularJS客户端代码。

11. 后续将以重点此为入口来深入分析Portal客户端和后端代码和处理逻辑。 

    主要参考文档：
    
    https://wiki.onap.org/download/attachments/1015849/OpenECOMP%2BPortal%2BSDK%2BDocumentation.docx?api=v2
    https://wiki.onap.org/download/attachments/1015849/OpenECOMP%2BPortal%2BSDK%2BDocumentation.pdf?api=v2
    https://wiki.onap.org/download/attachments/1015849/PortalArchDiagram.pptx?api=v2
    https://wiki.onap.org/download/attachments/1015849/Portal_API.pdf?api=v2
    https://wiki.onap.org/download/attachments/1015849/Portal_API.docx?api=v2

