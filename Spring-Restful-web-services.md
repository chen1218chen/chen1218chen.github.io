---
title: Spring Restful web services
date: 2016-09-14 10:42:30
tags: [Spring, springMVC]
---
Spring 是流行的 Java EE 应用开发框架，现在它的 MVC 层也支持 REST 了,
在Spring框架支持REST之前，人们会选用其他几种技术来实现java的RESTful Web Services,如REStlet,Jersey和RESTEasy。
**Spring3.0**之后增加了对RESTful Web Services的支持。REST 支持被无缝整合到 Spring 的 MVC 层，它可以很容易应用到使用 Spring 构建的应用中。
Spring REST 支持的主要特性包括：
- 注释，如 @RequestMapping 和 @PathVariable，支持资源标识和 URL 映射
- ContentNegotiatingViewResolver 支持为不同的 MIME/内容类型使用不同的表示方式
- 使用相似的编程模型无缝地整合到原始的 MVC 层
## 实现
web.xml中激活Spring,配置：


    listener-ContextLoadListener
    servlet-DispatcherServlet
Spring的配置文件

    <!--指明 controller 所在包，并扫描其中的注解-->
    <context:component-scan base-package="com.aerors.th.gis"/>
    <!-- 开启注解 -->
    <mvc:annotation-driven/>
    <!-- 用注解进行依赖注入 -->
    <context:annotation-config></context:annotation-config>

**component-scan:** 启用对带有 Spring 注释的类进行自动扫描
在实践中，它将检查控制器类中所定义的 @Controller 注释

## Controller控制器实现    

    @Controller
    @RequestMapping("/user")
    public class UserRest {
    	@Resource
    	private IUserService userManagerImpl;
    	
    	@RequestMapping(value = "/queryAllUser", method = RequestMethod.GET)
    	@ResponseBody
    	public JSONArray queryAllUser() {
    		
    		List<User> dataList = userManagerImpl.queryAll();
    		JSONArray userJSONList2 = JSONArray.fromObject(dataList);
    		return userJSONList2;
    	}
    	
> @PathVariable与@RequestParam


    @Controller
    public class EmployeeController {
        @RequestMapping(method=RequestMethod.GET, value="/employee/{id}")
        public ModelAndView getEmployee(@PathVariable String id) {
        	Employee e = employeeDS.get(Long.parseLong(id));
        	returnnew ModelAndView(XML_VIEW_NAME, "object", e);
        }
        ...
    }


    @Controller
    @RequestMapping("employees")
    public class EmployeeController {
     
        Employee employee = new Employee();
     
        @RequestMapping(value = "/{name}", method = RequestMethod.GET, produces = "application/json")
        public @ResponseBody Employee getEmployeeInJSON(@PathVariable String name) {
     
       	 employee.setName(name);
       	 employee.setEmail("employee1@genuitec.com");
     
       	 return employee;
     
        }
        
        @RequestMapping(value = "/queryByName", method = RequestMethod.POST)
        @ResponseBody
        public User queryByName(@RequestParam("uname") String uname) {

        	User user;
        	try {
        		user = userManagerImpl.queryByName(uname).get(0);
        	} catch (Exception e) {
        		// TODO 自动生成的 catch 块
        		e.printStackTrace();
        		return null;
        	}

        	return user;
        }
    }
> @RestController

Spring 4.0中引入 @RestController简化了@Controller，@ResponseBody注释
@RestController注解相当于@ResponseBody ＋ @Controller合在一起的作用。

    @RestController
    @RequestMapping("employees")
    public class EmployeeController {
     
        Employee employee = new Employee();
     
        @RequestMapping(value = "/{name}", method = RequestMethod.GET, produces = "application/json")
        public Employee getEmployeeInJSON(@PathVariable String name) {
     
       	 employee.setName(name);
       	 employee.setEmail("employee1@genuitec.com");
     
       	 return employee;
     
        }
    }