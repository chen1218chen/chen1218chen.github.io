---
title: shiro之roles实现or关系的角色过滤
date: 2017-03-07 09:58:07
tags: shiro
---

roles参数可以写多个，多个时必须加上引号，并且参数之间用逗号分割，当有多个参数时，每个参数通过才算通过，相当于`hasAllRoles()`方法。**shiro的角色过滤是and的关系**。

而最近根据项目需求，要求订阅号管理模块，多个角色都可以进行管理，角色的过滤非and关系而是or的关系，`applicationContext.xml`中roles["管理员"，"订阅号"]不能实现或者关系的角色过滤，需要自定义继承`AuthorizationFilter`的Filter。
## CustomRolesAuthorizationFilter

    import javax.servlet.ServletRequest;
    import javax.servlet.ServletResponse;
    import org.apache.shiro.subject.Subject;
    import org.apache.shiro.web.filter.authz.AuthorizationFilter;

    public class CustomRolesAuthorizationFilter extends AuthorizationFilter
    {

     @Override
     protected boolean isAccessAllowed(ServletRequest request,
       ServletResponse response, Object mappedValue) throws Exception
     {
      Subject subject = getSubject(request, response);  
            String[] rolesArray = (String[]) mappedValue;  
      
            if (rolesArray == null || rolesArray.length == 0) 
            {  
                return true;  
            }  
            for(int i=0;i<rolesArray.length;i++)
            {
                if(subject.hasRole(rolesArray[i]))
                {  
                    return true;  
                }  
            }  
            return false;  
     }
    }
## applicationContext.xml

    <!--自定义的filter-->
    <bean id="roleOrFilter" class="com.city.login.CustomRolesAuthorizationFilter"> 
     </bean>

    <bean id="shiroFilter" class="org.apache.shiro.spring.web.ShiroFilterFactoryBean" depends-on="roleOrFilter">
    	<!-- securityManager -->
    	<property name="securityManager" ref="securityManager" />
    	<!-- 登录路径 -->
    	<property name="loginUrl" value="/login.jsp" />
    	<!-- 登录成功后跳转路径 -->
    	<property name="successUrl" value="/index.jsp" />
    	<!-- 授权失败跳转路径 -->
    	<property name="unauthorizedUrl" value="/login.jsp" />
    	<property name="filters">
    		<util:map>
    			<entry key="authc" value-ref="formAuthenticationFilter" />
    			<entry key="sysUser" value-ref="sysUserFilter" />
    			<entry key="kickout" value-ref="kickoutSessionControlFilter" />
    			<entry key="roleOrFilter" value-ref="roleOrFilter"/>
    		</util:map>
    	</property>

    	<!-- 过滤链定义 -->
    	<property name="filterChainDefinitions">
    		<value>
    			/login.jsp = anon
    			/logout = logout 
    			<!-- authc表示需要认证的服务 -->
    			/rest/credit/** = authc 
    			/rest/Uploadinfo/** = authc
    			/userManager.jsp = kickout,roles["管理员"]
    			/subscribe.jsp = kickout,roleOrFilter["订阅号","管理员"]
    			/lost.jsp = kickout,authc,roles["失物招领"]
    			/*.jsp = kickout,authc
    		</value>
    	</property>
    </bean>
这样，`roleOrFilter`就可以实现or的关系，即订阅号角色和管理员角色都可以访问`subscribe.jsp`页面。