---
title: angular实现用户登陆
date: 2016-09-07 16:38:30
tags: angular
---
[toc]

尝试用angular来实现登陆，表单的验证和控制果然很简单方便哎，跟大家分享一下，望不吝赐教。
## 表单验证
### $pristine
布尔值，表示用户是否修改了表单。如果为true，表示没有修改过；false表示修改过。

    formName.inputFieldName.$pristine;
### $dirty
布尔值，表示用户已修改过表单

    formName.inputFieldName.$dirty
### $valid
布尔值，表示表单是否通过验证，为true，则表示通过验证

    formName.inputFieldName.$valid

### $invalid
布尔值，为true表示未通过验证

    formName.inputFieldName.$invalid
### $error
$error对象，包含input每个验证是有效还是无效的

    form.name.$error.minlength
    form.name.$error.maxlength
    form.name.$error.required //非空
    form.email.$error.email  //无效邮箱
### 其他验证

    form.name.$touched  //触发
## ng-pattern
关于正则表达式的验证

    <input type="password" 
            class="form-control" 
            ng-model="user.password" 
            name="password"
            ng-pattern="/^[A-Za-z]{1}[0-9A-Za-z_]{2,29}$/">
            
    <p class="help-block has-error" ng-show="regForm.password.$error.pattern&&regForm.password.$touched">只能是数字、字母和下划线</p>

由上可看出，正则表达式验证是否通过可以通过**regForm.password.$error.pattern**此表达式验证。
## 登陆案例
登陆界面如下：

![登陆界面][1]

表单验证效果：

![表单验证][2]

验证通过效果：

![enter description here][3]
代码如下
> html


    <!DOCTYPE html>
    <html lang="en">
    <head>
    	<meta charset="UTF-8">
    	<link rel="stylesheet" type="text/css" href="../lib/bootstrap-3.3.5/css/bootstrap.min.css">
    	<link rel="stylesheet" type="text/css" href="form.css">
    	<title>登录表单</title>
    </head>
    <body ng-app="myApp">
    	<div class="row">
    		<div class="col-md-offset-3 col-md-6 location ">
    			<div class="panel panel-primary">
    				<div class="panel-heading">
    					<div class="panel-title">登录</div>
    				</div>
    				<div class="panel-body">
    					<div class="row" ng-controller="loginCtrl">
    						<div class="col-md-12">
    							<form class="form-horizontal" name="loginForm"  ng-submit="submitForm()" novalidate>
    								<div class="form-group" >
    									<label class="col-md-2 control-label">用户名：</label>
    									<div class="col-md-10">
    										<input type="text" class="form-control" name="name" ng-model="user.name" ng-class="{error:loginForm.name.$invalid&&!loginForm.name.$pristine}" ng-minlength="4" ng-maxlength="10"placeholder="用户名" required="">
    										<p ng-show="loginForm.name.$error.minlength" class="help-block">name is too short</p>
    										<p class="help-block" ng-show="loginFOrm.name.$error.maxlength">name is too long</p>
    									</div>
    								</div>
    								<div class="form-group" >
    									<label class="col-md-2 control-label">密码：</label>
    									<div class="col-md-10">
    										<input type="text" class="form-control" name="password" ng-model="user.password" placeholder="只能是数字、字母和下划线" ng-minlength="6" ng-maxlength="10"   ng-class="{error:loginForm.password.$invalid&&!loginForm.password.$pristine}" ng-pattern="/^[A-Za-z]{1}[0-9A-Za-z_]{2,29}$/" required="">
    										<p class="help-block" ng-show="loginForm.password.$error.minlength">password is too short</p>
    										<p class="help-block" ng-show="loginForm.password.$error.maxlength">password is too long</p>
    									</div>
    								</div>
    								<div class="form-group">
    									<div class="col-md-offset-2 col-md-10">
    										<div class="checkbox">
    											<label><input type="checkbox" ng-model="user.autoLogin">自动登录</label>
    										</div>
    									</div>
    								</div>
    								<div class="form-group">
    									<div class="col-md-offset-2 col-md-10">
    										<button class="btn btn-primary" ng-disabled="loginForm.$invalid">登录</button>
    									</div>
    								</div>
    							</form>
    						</div>
    					</div>
    				</div>
    			</div>
    		</div>
    	</div>
    	
    	<script src="../lib/angular.min.js"></script>
    	<script type="text/javascript" src="../lib/jquery-2.2.0.min.js"></script>
    	<script type="text/javascript" src="../lib/bootstrap-3.3.5/js/bootstrap.min.js"></script>
    	<script type="text/javascript" src="form.js"></script>
    </body>
    </html>
    
> js


    'use strict';

    var myApp = angular.module('myApp',[]);
    myApp.controller('loginCtrl',['$scope', function($scope){
    	$scope.user={
    	/*	name: "ch",
    		password: 1,
    		autoLogin: true*/
    	};
    	$scope.submitForm = function(){
    		var formData = {
    			name : $("input[name=name]").val(),
    			password : $("input[name=password]").val()
    		};
    		console.dir(formData);
    		alert(formData);
    		$.ajax({
    			type:"post",
    			url:"",
    			dataType:"json",
    			data:formData,
    			success:function(data){
    				alert("submit success!");
    			}
    		});
    /*		event.preventDefault();*/
    	}
    }])
    
> css


    .location{
    	margin-top: 150px; 
    }
    .error{
    	color:red;
    	border-color: red;
        -webkit-box-shadow: 0 0 6px #f8b9b7;
        -moz-box-shadow: 0 0 6px #f8b9b7;
        box-shadow: 0 0 6px #f8b9b7;
    }
    


  [1]: ./images/Image%202.png "Image 2.png"
  [2]: ./images/Image%203.png "Image 3.png"
  [3]: ./images/Image%204.png "Image 4.png"