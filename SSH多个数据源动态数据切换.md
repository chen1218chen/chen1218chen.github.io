---
title: SSH多个数据源动态数据切换
date: 2016-03-04 16:16:29
tags: ssh, aop,hibernate
---

[TOC]

## SSH多个数据源动态数据切换 
一般情况下我们在spring配置中只配置一个dataSource来连接数据库，然后在SessionFactory中绑定dataSource。如果有需要连接多个数据库时的正确做法是：
![enter description here][1]

SSH框架的项目中我需要连接两个PostgreSQL数据库既可以手动切换，也可以使用aop来动态切换。
### 1. 配置文件
    
    <!-- 自动为spring容器中那些配置@aspectJ切面的bean创建代理，织入切面 -->
    <aop:aspectj-autoproxy proxy-target-class="true" />
    <!--第一个数据源-->
    <bean id="dataSourceJDJ" class="com.mchange.v2.c3p0.ComboPooledDataSource"
		destroy-method="close">
		<!-- oracle连接 <property name="driverClass" value="oracle.jdbc.driver.OracleDriver"/> 
			<property name="jdbcUrl" value="jdbc:oracle:thin:@127.0.0.1:1521:orcl"/> -->
		<property name="driverClass" value="org.postgresql.Driver" />
		<property name="jdbcUrl" value="jdbc:postgresql://127.0.0.1:5432/jdj" />
		<property name="user" value="postgres" />
		<property name="password" value="123456" />
		<property name="minPoolSize" value="5" />
		<property name="maxPoolSize" value="20" />
		<property name="initialPoolSize" value="10" />
		<property name="maxIdleTime" value="60" />
		<property name="acquireIncrement" value="5" />
		<property name="maxStatements" value="20" />
		<property name="idleConnectionTestPeriod" value="30" />
		<property name="acquireRetryAttempts" value="10" />
		<property name="breakAfterAcquireFailure" value="true" />
		<property name="testConnectionOnCheckout" value="true" />
	</bean>
    
    <!--第二个数据源-->
    	<bean id="dataSourceSDE" class="com.mchange.v2.c3p0.ComboPooledDataSource"  
        destroy-method="close">  
     <property name="driverClass" value="org.postgresql.Driver" />
		<property name="jdbcUrl" value="jdbc:postgresql://192.168.1.139:5432/sde" />
		<property name="user" value="postgres" />
		<property name="password" value="123456" />
		<property name="minPoolSize" value="5" />
		<property name="maxPoolSize" value="20" />
		<property name="initialPoolSize" value="10" />
		<property name="maxIdleTime" value="60" />
		<property name="acquireIncrement" value="5" />
		<property name="maxStatements" value="20" />
		<property name="idleConnectionTestPeriod" value="30" />
		<property name="acquireRetryAttempts" value="10" />
		<property name="breakAfterAcquireFailure" value="true" /> 
		<property name="testConnectionOnCheckout" value="true" />
    </bean> 
    
	<!--动态切换配置-->
	<bean id="dynamicDataSource" class="com.lbs.core.DynamicDataSource">  
        <property name="targetDataSources">  
            <map key-type="java.lang.String">  
                <entry value-ref="dataSourceJDJ" key="dataSourceJDJ"></entry>  
                <entry value-ref="dataSourceSDE" key="dataSourceSDE"></entry>  
            </map>  
        </property>  
        <property name="defaultTargetDataSource" ref="dataSourceJDJ"></property>  
    </bean> 
	
	<bean id="sessionFactory"
		class="org.springframework.orm.hibernate4.LocalSessionFactoryBean">
		<property name="dataSource">
			<ref bean="dynamicDataSource" />
		</property>
		<property name="packagesToScan">
			<list>
				<value>com.lbs.model</value>
			</list>
		</property>
		<property name="hibernateProperties">
			<props>
				<prop key="hibernate.show_sql">true</prop>
				<prop key="hibernate.dialect">org.hibernate.dialect.PostgreSQLDialect</prop>
				<prop key="hibernate.hbm2ddl.auto">update</prop>
			</props>
		</property>
	</bean>
	
	<!-- 注解的事务 -->
	<tx:annotation-driven transaction-manager="transactionManager" />
	
	<bean id="transactionManager"
		class="org.springframework.orm.hibernate4.HibernateTransactionManager">
		<property name="sessionFactory" ref="sessionFactory" />
	</bean>
    <aop:config>  
        <aop:pointcut id="transactionPointCut" expression="execution(* com.lbs.service..*.*(..))" />  
        <aop:advisor advice-ref="txAdvice" pointcut-ref="transactionPointCut" />  
    </aop:config>  
  
    <tx:advice id="txAdvice" transaction-manager="transactionManager">  
        <tx:attributes>  
            <tx:method name="add*" propagation="REQUIRED" />  
            <tx:method name="save*" propagation="REQUIRED" />  
            <tx:method name="update*" propagation="REQUIRED" />  
            <tx:method name="delete*" propagation="REQUIRED" />  
            <tx:method name="*" read-only="true" />  
        </tx:attributes>  
    </tx:advice>  
  
    <aop:config>  
        <aop:aspect id="dataSourceAspect" ref="dataSourceInterceptor">  
            <aop:pointcut id="daoOne" expression="execution(* com.dao.one.*.*(..))" />  
            <aop:pointcut id="daoTwo" expression="execution(* com.dao.two.*.*(..))" />  
            <aop:before pointcut-ref="daoOne" method="setdataSourceOne" />  
            <aop:before pointcut-ref="daoTwo" method="setdataSourceTwo" />  
        </aop:aspect>  
    </aop:config>  
1.  注意事务拦截器的配置
Spring的事务管理是与数据源绑定的，一旦程序执行到事务管理的那一层（如service）的话，由于在进入该层之前事务已经通过拦截器开启，因此在该层切换数据源是不行的，明白事务的原理是尤为重要的，我之前的文章中，将切换数据源的拦截器配置在了Dao层是有问题的。改为在service层进行配置拦截
2. 注意数据库表的创建
一些人喜欢用Hibernate的自动创建表的功能，但需要注意，在多数据源中，尤其是不同数据库的多数据源，想都自动建表是不行的。因为Hibernate自动建表是在项目启动时触发的，因此只会建立项目配置的默认数据源的表，而其他数据源的表则不会自动创建。大家要注意着点。
3. Hibernate的数据库方言（dialect）可以忽略
在多数据源时，方言的设置可以忽略，Hibernate在使用时会自动识别不同的数据库，因此不必纠结这个配置，甚至不配置也可以。

    
### 2. java文件
需要写以下三个.java文件来实现切换
![enter description here][2]

> DatabaseContextHolder， 用来保存当前应该使用的数据源名称
   
    package com.lbs.core;

    public class DatabaseContextHolder {
    	private static final ThreadLocal<String> contextHolder = new ThreadLocal<String>();  
    	public static void setCustomerType(String customerType) {  
            contextHolder.set(customerType);  
        }  
        public static String getCustomerType() {  
            return contextHolder.get();  
        }  
        public static void clearCustomerType() {  
            contextHolder.remove();  
        }  
    }
> AbstractRoutingDataSource实现类，实现数据源路由选择

    package com.lbs.core;

    import org.springframework.jdbc.datasource.lookup.AbstractRoutingDataSource;
    
    public class DynamicDataSource extends AbstractRoutingDataSource{
    
    	@Override
    	protected Object determineCurrentLookupKey() {
    		// TODO 自动生成的方法存根
    		return DatabaseContextHolder.getCustomerType(); 
    	}
    }
> DataSourceInterceptor来进行切换
    
    package com.lbs.core;

    import org.aspectj.lang.JoinPoint;
    import org.aspectj.lang.annotation.Aspect;
    import org.springframework.stereotype.Component;
    
    @Aspect
    @Component
    //@Order(value=1)
    //使用aop来自动切换数据源
    public class DataSourceInterceptor {
    	
    	 @Before("execution(* com.lbs.JDJDao.impl.*.*(..))")
    	public void setdataSourceJDJ(JoinPoint jp) {
    		DatabaseContextHolder.setCustomerType("dataSourceJDJ");
    	}
         @Before("execution(* com.lbs.SDEDao.impl.*.*(..))")
    	public void setdataSourceSDE(JoinPoint jp) {
    		DatabaseContextHolder.setCustomerType("dataSourceSDE");
    	}
    }
    
或者不用aop @before来拦截，手动调用以下语句
    
    DatabaseContextHolder.setCustomerType("dataSourceSDE");
    


 


  [1]: ./images/image1.jpg "image1.jpg"
  [2]: ./images/image2.jpg "image2.jpg"