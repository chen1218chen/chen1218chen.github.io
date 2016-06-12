---
title: Struts2之ModelDriven接口实现登陆及退出
date: 2016-06-12 14:05:01
tags: struts2
---
当用POST方式提交表单，提交的数据量大时，action中定义各种数据，并实现get/set请求就显得过于臃肿，改用Struts2的ModelDriven接口来获取用户提交的HTTP请求，只需要定义相应的Model，Struts2框架会自动将用户提交的HTTP信息赋给相应的Model。
对项目中的LoginAction进行修改，采用ModelDriven方式。
## LoginModel
首先，系统采用电话+密码的登陆方式，建立对应的LoginModel.java。

    public class LoginModel {
    	private String telephone;
    	private String password;

    	public String getTelephone() {
    		return telephone;
    	}

    	public void setTelephone(String telephone) {
    		this.telephone = telephone;
    	}

    	public String getPassword() {
    		return password;
    	}

    	public void setPassword(String password) {
    		this.password = password;
    	}
    }
## LoginAction
表单提交的action继承ModelDriven<LoginModel>接口，实现getter方法，重载execute方法。

    @ParentPackage(value = "json-default")
    @Controller
    @Action(value = "/loginAction2", results = { @Result(name = "success", type = "redirect", location = "/index.jsp"),
    		@Result(name = "input", type = "redirect", location = "/login.html"),
    		@Result(name = "error", type = "redirect", location = "/login.html") }, exceptionMappings = {
    				@ExceptionMapping(exception = "java.lang.Exception", result = "error") })
    public class LoginAction2 extends BaseAction implements ModelDriven<LoginModel> {

    	private static final long serialVersionUID = 1L;

    	@Resource
    	private IUserService userManagerImpl;
    	private LoginModel login = new LoginModel();

    	@Override
    	public LoginModel getModel() {
    		// TODO Auto-generated method stub
    		return login;
    	}

    	// 重载
    	public String execute() throws Exception {
    		String password = login.getPassword();
    		String telephone = login.getTelephone();
    		if (password != null && !"".equals(password)) {
    			// password = MD5.MD5Encode(password);
    		}
    		if (telephone == null || telephone == "" || password == null || password == "")
    			return INPUT;
    		Subject subject = SecurityUtils.getSubject();
    		UsernamePasswordToken token = new UsernamePasswordToken(telephone, password);
    		token.setRememberMe(true);// 记住我

    		try {
    			// 登录，即进行身份验证操作
    			subject.login(token);// 调用 myRealm的 doGetAuthenticationInfo方法
    			String telephone2 = (String) subject.getPrincipal();// 登陆成功就可以这样子拿到用户名了
    			User user = userManagerImpl.queryByTel(telephone2);
    			subject.getSession().setAttribute("uname", user.getName());
    		} catch (UnknownSessionException uae) {
    			subject.getSession().setAttribute("myerror", "登录异常!");
    			return ERROR;
    		} catch (UnknownAccountException ex) {
    			subject.getSession().setAttribute("myerror", "用户名不存在!");
    			return INPUT;
    		} catch (IncorrectCredentialsException ice) {
    			subject.getSession().setAttribute("myerror", "密码错误!");
    			return INPUT;
    		} catch (LockedAccountException lae) {
    			subject.getSession().setAttribute("myerror", "登录异常!");
    			return INPUT;
    		} catch (AuthenticationException e) {
    			subject.getSession().setAttribute("myerror", "用户登录失败!");
    			return INPUT;
    		}
    		return SUCCESS;
    	}
    	
    	public String logout() throws Exception {
    		Subject subject = SecurityUtils.getSubject();
    		if (subject.isAuthenticated()) {
    			subject.logout(); // session 会销毁，在SessionListener监听session销毁，清理权限缓存
    		}
    		return INPUT;
    	}
    }
注意`subject.getSession().setAttribute(arg0, arg1);`shiro的seeion，可以在login.jsp页面获取到错误类型。

    <%
    	Object error = session.getAttribute("myerror"); 
    	if(error==null)
    		error=" ";
     %>
     <form action="login.action">
        ...
        <label style="color:red"><%=error%></label>
        <div class="form-group">
        	<input type="text" name="telephone" class="email"
        		placeholder="请输入电话" autocomplete="off" required="required" />
        </div>
        <div class="form-group ">
        	<input type="password" name="password" class="password"
        		placeholder="请输入密码" autocomplete="off" required="required" />
        </div>
        ...
    </form>
## LogoutAction
注销账号退出系统，可以在LoginAction中添加一个logout方法，直接调用`loginAction!logout.action`，或者重新写一个Action

    @ParentPackage(value = "json-default")
    @Controller
    public class LogoutAction extends BaseAction {

    	private static final long serialVersionUID = 1L;
    	
    	@Action(value = "/logoutAction", results = { 
    			@Result(name = "success", type = "redirect", location = "/login.jsp"),
    			@Result(name = "error", type = "redirect", location = "/index.jsp") }, exceptionMappings = {
    					@ExceptionMapping(exception = "java.lang.Exception", result = "error") })
    	public String logout() throws Exception {
    		Subject subject = SecurityUtils.getSubject();
    		if (subject.isAuthenticated()) {
    			subject.logout(); // session 会销毁，在SessionListener监听session销毁，清理权限缓存
    		}
    		return SUCCESS;
    	}
    }
调用`logoutAction.action`即可。
