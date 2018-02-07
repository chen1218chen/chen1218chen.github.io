---
title: '@Resource与@Autowired的区别'
date: 2018-01-19 13:55:58
tags: springMVC
---

@Resource与@Autowired都是用来注入Bean的，区别在于@Resource默认按照ByName自动注入，由J2EE提供支持；@Autowired默认按照ByType注入，由spring提供支持。
具体用法如下：

    public class BaseServiceImpl implements IBaseService {

        @Autowired
        @Qualifier("IbaseDao")
        private IBaseDao baseDao;
        ...
    }
也可以用@Resource方式注入，写法如下

    public class BaseServiceImpl implements IBaseService {
    	
    	@Resource(name="IbaseDao")
    	private IBaseDao baseDao;
        ...
    }
两种方法效果一样，可以看出@Autowired加上@Qualifier("IbaseDao")标签相当于@Resource按照ByName注入。
推荐使用@Resource