---
title: shiro入门配置
date: 2016-03-30 15:43:06
tags: shiro
---

# 基础配置
使用shiro的步骤
1. 导入JAR包
2. web.xml配置

        <!-- Shiro配置 -->    
        <filter>    
            <filter-name>shiroFilter</filter-name>    
            <filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class>    
        </filter>    
        <filter-mapping>    
            <filter-name>shiroFilter</filter-name>    
            <url-pattern>/*</url-pattern>    
        </filter-mapping>

3. applicationContext.xml配置
```
    <!-- ehcache 的配置 -->
	<cache:annotation-driven cache-manager="cacheManager" />
	<bean id="cacheManagerFactory"
		class="org.springframework.cache.ehcache.EhCacheManagerFactoryBean">
		<property name="configLocation">
			<value>classpath:ehcache.xml</value>
		</property>
	</bean>
	<bean id="cacheManager" class="org.springframework.cache.ehcache.EhCacheCacheManager">
		<property name="cacheManager" ref="cacheManagerFactory" />
	</bean>

	<!-- 自定义Realm实现 -->
	<bean id="myRealm" class="com.lbs.login.myRealm">
		<!-- <property name="cacheManager" ref="shiroEhcacheManager" /> -->
	</bean>
	<!-- 用户授权/认证信息Cache, 采用EhCache 缓存 -->
	<bean id="shiroEhcacheManager" class="org.apache.shiro.cache.ehcache.EhCacheManager">
		<property name="cacheManagerConfigFile" value="classpath:ehcache.xml" />
		<!-- <property name="cacheManager" ref="cacheManager" /> -->
	</bean>
	<!-- securityManager -->
	<bean id="securityManager" class="org.apache.shiro.web.mgt.DefaultWebSecurityManager">
		<property name="realm" ref="myRealm" />
		<property name="cacheManager" ref="shiroEhcacheManager" />
	</bean>
	
	<!-- Shiro生命周期处理器,保证实现了Shiro内部lifecycle函数的bean执行 -->    
    <bean id="lifecycleBeanPostProcessor" class="org.apache.shiro.spring.LifecycleBeanPostProcessor"/>
    
	<!-- Shiro Filter 拦截器相关配置 -->
	<!--shiro过滤器配置，bean的id值须与web中的filter-name的值相同 -->
	<bean id="shiroFilter" class="org.apache.shiro.spring.web.ShiroFilterFactoryBean">
		<!-- securityManager -->
		<property name="securityManager" ref="securityManager" />
		<!-- 登录路径 -->
		<property name="loginUrl" value="/login.jsp" />
		<!-- 登录成功后跳转路径 -->
		<property name="successUrl" value="/manager/index.jsp" />
		<!-- 授权失败跳转路径 -->
		<property name="unauthorizedUrl" value="/login.jsp" />
		<!-- 过滤链定义 -->
		<property name="filterChainDefinitions">
			<value>
				/login.jsp* = anon
				/logout* = anon
				/*.js = anon
				/*.css = anon
				/manager/** = authc,roles["管理员"]<!-- 表示需要认证的链接 -->
				/index.jsp = authc
			</value>
		</property>
	</bean>
```
4. 建立myRealm.java，重写以下两种方法
 > doGetAuthorizationInfo()方法可以理解为是权限验证
 > doGetAuthenticationInfo(AuthenticationToken token)理解为登陆验证。
 
# shiro 授权 
Shiro支持三种方式实现授权过程： 
- 编码实现
- 注解实现
- JSP Taglig实现

## 角色授权
当需要验证用户是否拥有某个角色时，可以调用Subject实例的hasRole*方法验证。

    Subject currentUser = SecurityUtils.getSubject();
    if (currentUser.hasRole("administrator")) {
        //show the admin button
    } else {
        //don't show the button?  Grey it out?
    }
    
