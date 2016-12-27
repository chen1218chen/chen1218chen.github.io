---
title: XML文档解读
date: 2016-12-27 10:34:19
tags: XML
---
[TOC]

关于XML文档的xmlns、xmlns:xsi和xsi:schemaLocation
**参见Spring的配置文件**
## xmlns
xmlns其实是XML Namespace的缩写, XML命名空间。
> 为什么需要xmlns

考虑这样两个XML文档：表示HTML表格元素的table

    <table>
      <tr>
        <td>Apples</td>
        <td>Bananas</td>
      </tr>
    </table>
和描述一张桌子的table：


    <table>
      <name>African Coffee Table</name>
      <width>80</width>
      <length>120</length>
    </table>
    
假如这两个 XML 文档被一起使用，由于两个文档都包含带有不同内容和定义的 <table> 元素，就会发生命名冲突。XML 解析器是无法确定如何处理这类冲突。为了解决上述问题，xmlns就产生了。

> 如何使用xmlns

语法：xmlns:namespace-prefix="namespaceURI"
例如：

    xmlns:context="http://www.springframework.org/schema/context"

    <context:component-scan base-package="xxx.xxx.controller" />
则上面的两个table可写成

    <!-- 这里xmlns:h="url1"表示这个table是用h作为标记，table的写法在url1中定义 -->

    <h:table xmlns:h="url1">
      <h:tr>
        <h:td>Apples</h:td>
        <h:td>Bananas</h:td>
      </h:tr>
    </h:table>
    
    
    <!-- 这里xmlns:f="url2"表示这个table是用f作为标记，table的写法在url2中定义 -->

    <f:table xmlns:f="url2">
      <f:name>African Coffee Table</f:name>
      <f:width>80</f:width>
      <f:length>120</f:length>
    </f:table>
## xmlns和xmlns:xsi有什么不同？

xmlns表示默认的Namespace。例如：

    xmlns="http://www.springframework.org/schema/beans"
这一句表示该文档默认的XML Namespace为http://www.springframwork.org/schema/beans。对于默认的Namespace中的元素，可以不使用前缀。例如Spring XML文档中的

    <bean id="xxx" class="xxx.xxx.xxx.Xxx">
      <property name="xxx" value="xxxx"/>
    </bean>

xmlns:xsi表示使用xsi作为前缀的Namespace，当然前缀xsi需要在文档中声明。
## xsi:schemaLocation
xsi:schemaLocation属性其实是Namespace为http://www.w3.org/2001/XMLSchema-instance里的schemaLocation属性，正是因为我们一开始声明了

    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
它定义了XML Namespace和对应的XSD（Xml Schema Definition）文档的位置的关系。它的值由一个或多个URI引用对组成，两个URI之间以空白符分隔（空格和换行均可）。第一个URI是定义的XML Namespace的值，第二个URI给出Schema文档的位置，Schema处理器将从这个位置读取Schema文档，该文档的targetNamespace必须与第一个URI相匹配。例如：

    xsi:schemaLocation="http://www.springframework.org/schema/context 
                        http://www.springframework.org/schema/context/spring-context.xsd"
这里表示Namespace为http://www.springframework.org/schema/context的Schema的位置为http://www.springframework.org/schema/context/spring-context.xsd。

原文请参考：
[关于XML文档的xmlns、xmlns:xsi和xsi:schemaLocation][1]


  [1]: https://yq.aliyun.com/articles/40353