

## method

由于在 RequestMapping 注解类中 method() 方法返回的是 RequestMethod 数组，所以可以给 method 同时指定多个请求方式

    @Controller
    @RequestMapping(path = "/user")
    public class UserController {
            // 该方法将同时接收通过GET和POST方式发来的请求
    	@RequestMapping(path = "/login", method={RequestMethod.POST,RequestMethod.GET})
    	public String login() {
    		return "success";
    	}
    }
    
## params
@RequestMapping 中可以使用 params 来限制请求参数，来实现进一步的过滤请求

    @Controller
    @RequestMapping(path = "/user")
    public class UserController {
            
            // 该方法将接收 /user/login 发来的请求，且请求参数必须为 username=kolbe&password=123456
    	@RequestMapping(path = "/login", params={"username=kolbe","password=123456"})
    	public String login() {
    		return "success";
    	}
    }
    
>如：http://localhost/SpringMVC/user/login?username=kolbe&password=123456

## headers
 @RequestMapping 的 headers 属性,该属性表示请求头

    @Controller
    @RequestMapping(path = "/user")
    public class UserController {
            // 表示只接收本机发来的请求
    	@RequestMapping(path = "/login", headers="Host=localhost:8080")
    	public String login() {
    		return "success";
    	}
    }