---
title: shiro角色配置
date: 2016-04-06 14:50:46
tags: shiro
---
[TOC]

# 角色验证
## 数据库表
- tuser:用户表
    uid + uname + upassword 
- roles:角色表
    rid + rname
- user_to_role：用户角色表，hibernate会自动建立
    user_id  +  role_id

### roles


    @Entity
    @Table(name = "roles", schema = "public")
    public class Roles implements java.io.Serializable {
    	private Integer rid;
    	private String rname;
    //	private Set<User> user;

    	public Roles() {
    	}
    	/** full constructor */
    	public Roles(String rname) {
    		this.rname = rname;
    	}

    	@Id
    	@GeneratedValue
    	@Column(name = "rid", unique = true, nullable = false)
    	public Integer getRid() {
    		return rid;
    	}

    	public void setRid(Integer rid) {
    		this.rid = rid;
    	}

        @Column(name = "rname", length = 20)
        public String getRname() {
        	return rname;
        }
        
        public void setRname(String rname) {
        	this.rname = rname;
        }
        
        /*	@ManyToMany(cascade = CascadeType.MERGE, fetch = FetchType.EAGER)
        @JoinTable(name = "user_to_role",   
        	joinColumns ={@JoinColumn(name = "role_id") },   
        	inverseJoinColumns = { @JoinColumn(name = "user_id")
        })
        	public Set<User> getUser() {
        	return user;
        }
        
        public void setUser(Set<User> user) {
        	this.user = user;
        }*/
        
        }
为防止转化成json时出现循环，ManyToMany配置是只在依赖方配置，即只在user端进行配置。
### user

        @Entity
        @Table(name = "tuser")
        public class User implements java.io.Serializable {

        	private Integer uid;
        	private String uname;
        	private String upassword;
        	//一个用户具有多个角色 
        	private Set<Roles> roles = new HashSet<Roles>();
        
        	public User() {
        	}
        
        	// Property accessors
        	@Id
        	@GeneratedValue
        	@Column(name = "uid", unique = true, nullable = false)
        	public Integer getUid() {
        		return uid;
        	}
        
        	public void setUid(Integer uid) {
        		this.uid = uid;
        	}
        
        	@Column(name = "uname", length = 20)
        	public String getUname() {
        		return this.uname;
        	}
        	public void setUname(String uname) {
        		this.uname = uname;
        	}
        
        	@Column(name = "upassword", length = 20)
        	public String getUpassword() {
        		return this.upassword;
        	}
        
        	public void setUpassword(String upassword) {
        		this.upassword = upassword;
        	}
        	
        	@ManyToMany(cascade={CascadeType.MERGE}, fetch=FetchType.EAGER)
        	@JoinTable(name = "user_to_role",   
        		joinColumns= { @JoinColumn(name = "user_id")},
        		inverseJoinColumns ={@JoinColumn(name = "role_id")  
        	})
        	public Set<Roles> getRoles() {
        		return roles;
        	}
        
        	public void setRoles(Set<Roles> roles) {
        		this.roles = roles;
        	}
        }
此例子比较简单，没有涉及到权限permission
如果涉及到权限则需要建立权限表及对应的POJO
- permission：权限表
    id  +  permissionname  +  role_id
在role中添加：

```
    private List<Permission> permissionList;//一个角色对应多个权限
    ...
    @OneToMany(mappedBy="role")  
    public List<Permission> getPermissionList() {  
        return permissionList;  
    }  
    public void setPermissionList(List<Permission> permissionList) {  
        this.permissionList = permissionList;  
    }
```
Permission pojo

    @Entity  
    @Table(name="t_permission")  
    public class Permission {  
      
        private Integer id;  
        private String permissionname;  
        private Role role;//一个权限对应一个角色  
          
        @Id  
        @GeneratedValue(strategy=GenerationType.IDENTITY)  
        public Integer getId() {  
            return id;  
        }  
        public void setId(Integer id) {  
            this.id = id;  
        }  
        public String getPermissionname() {  
            return permissionname;  
        }  
        public void setPermissionname(String permissionname) {  
            this.permissionname = permissionname;  
        }  
        @ManyToOne  
        @JoinColumn(name="role_id")  
        public Role getRole() {  
            return role;  
        }  
        public void setRole(Role role) {  
            this.role = role;  
        }  
          
    } 
    
## index.jsp

    <%@ taglib prefix="shiro" uri="http://shiro.apache.org/tags"%>
    ...
    <ul class="nav navbar-nav navbar-right">
    <li><a>
    	<shiro:user>
    		欢迎你:<shiro:principal />
    	</shiro:user>
    	<!-- 没有登录，就认为是Guest，显示注册链接 --> 
    	<shiro:guest>
    		<a href="register.jsp">注册</a>
    	</shiro:guest></a>
    </li>
    <li>
    	<shiro:hasRole name="管理员">  
    	    <a href="manager/index.jsp">后台管理</a>  
    	</shiro:hasRole>
    	
    	//示例，具有两个中任一个权限时有操作权限
    	 <shiro:hasAnyRoles name="管理员,普通用户">
    	 	操作
    	 </shiro:hasAnyRoles>
    </li>
    <li><a href="javascript:void(0)" id="exitBtn" type="button">退出</a></li>
    </ul>
## 配置文件

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
    			<!-- 表示需要认证的链接 -->
    			/manager/** = authc,roles["管理员"]
    			/index.jsp = authc
    		</value>
    	</property>
    </bean>
   
## myRealm
权限验证和登陆验证

    public class myRealm extends AuthorizingRealm {
    	@Resource
    	private IUserService userManagerImpl;
    	/**
    	 * 权限验证
    	 */
    	@Override
    	protected AuthorizationInfo doGetAuthorizationInfo(
    			PrincipalCollection principals) {
    		// TODO Auto-generated method stub
    		String userName = (String) principals.fromRealm(getName()).iterator().next();
    		//根据用户名查找拥有的角色
    		User user = userManagerImpl.queryByName(userName).get(0);
    		Set<Roles> roles = user.getRoles();
    		if (roles != null) {
    			SimpleAuthorizationInfo info = new SimpleAuthorizationInfo();

    			for (Roles role : roles) {
    				info.addRole(role.getRname());
    			}

    			return info;
    		} else {
    			return null;
    		}
    		
    	}

    	/**
    	 * 登录验证
    	 */
    	@Override
    	protected AuthenticationInfo doGetAuthenticationInfo(
    			AuthenticationToken authcToken) throws AuthenticationException {
    			
    		// 令牌——基于用户名和密码的令牌
    		UsernamePasswordToken token = (UsernamePasswordToken) authcToken;
    		// 令牌中可以取出用户名密码
    		String userName = token.getUsername();
    		String passWord = new String(token.getPassword());
    		User user = userManagerImpl.queryByName(userName).get(0);
    		String passwd = user.getUpassword();
    		String approval = user.getIsapproval();
    	    if(user.getUid() == null){
    	    	System.out.println("没有找到用户 ");
    			throw new UnknownAccountException("没有找到用户 [" + userName + "]");
    	    }
    	    if(!passwd.equals(passWord)){
    	    	System.out.println("密码错误！！");
    	    	throw new IncorrectCredentialsException("密码错误！！");
    	    }
    	    if(approval.equals("0")){
    	    	System.out.println("审批未通过");
    	    	throw new IncorrectCredentialsException("审批未通过！！");
    	    }
    		//将正确的用户信息，请求登录用户的用户名和正确的密码，创建AuthenticationInfo对象并返回  
    		AuthenticationInfo authinfo = new SimpleAuthenticationInfo(userName,
    				passWord, getName());
    		return authinfo;
    	}
    }


