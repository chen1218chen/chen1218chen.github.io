---
title: SpringMVC之转发请求和重定向
date: 2017-01-03 10:27:55
tags: springMVC
---

spring MVC框架controller间跳转和重定向
## ModelAndView

    return new ModelAndView("redirect:/user/toList");//redirect模式
    return new ModelAndView("/user/toList");//默认为forward模式

若带参数请使用RedirectAttributes。
## RedirectAttributes
RedirectAttributes是Spring mvc 3.1版本之后出来的一个功能，专门用于重定向之后还能带参数跳转的
他有两种带参的方式：
### addAttribute

    RedirectAttributes attr = null;
    attr.addAttribute("name","123");
    attr.addAttribute("message", "error");
    return "redirect:/index";
相当于
> return "redirect:/index?name=123&message=error"

### addFlashAttribute
这种方式也能达到重新向带参，而且能隐藏参数，其原理就是放到session中，session在跳到页面后马上移除对象。所以你刷新一下后这个值就会丢掉

    attr.addFlashAttribute("message", "success");