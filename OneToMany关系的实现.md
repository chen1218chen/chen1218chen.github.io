---
title: OneToMany关系的实现
date: 2016-07-08 14:01:44
tags: Hibernate
---
# @OneToMany的实现
应项目需求，对举报信息添加回复功能，举报信息表（Uploadinfo）与回复表（reply）之间是1：n的关系。
>Uploadinfo.java中添加

    @OneToMany(targetEntity = Reply.class, cascade = { CascadeType.ALL })
    @JoinColumn(name="itemid")
    @OrderBy(value = "replyTime desc")
    private Set<Reply> replyid = new HashSet<Reply>();

    public Set<Reply> getReplyid() {
    	return replyid;
    }

    public void setReplyid(Set<Reply> replyid) {
    	this.replyid = replyid;
    }
@OrderBy：排序，对关联的reply表的数据按照replyTime值倒序排列
itemid： 外键，若reply表中没有这个字段，会自动添加一个。

>Reply.java

单向关系时，在reply方不做设置，以下是reply中定义的几个属性。

    private Integer id;
    private Integer itemid;
    private String message;
    private String replyTime;

取值：
![enter description here][1]

  [1]: ./images/Image%201.png "Image 1.png"