---
title: angular用户注册及密码一致性验证
date: 2016-09-12 15:52:30
tags: angular
---
# 注册表单
初学习angular，用一个简单的用户注册页面来练练手，mark一下，以后需要就直接Ctrl+c了。

    <form action="" class="form-horizontal" name="regForm" 	ng-controller="regController" 
     ng-submit="submitForm()" novalidate>
        <div class="form-group" ng-class="{'has-success':regForm.username.$valid&&regForm.username.$touched,'has-error':regForm.username.$invalid&&regForm.username.$touched }">
        	<label for="" class="col-md-2 control-label">
        		用户名
        	</label>
        	<div class="col-md-9">
        		<input type="text" 
        		class="form-control" 
        		ng-model="user.username" 
        		name="username" 
        		ng-minlength="4" ng-maxlength="10" 
        		placeholder="用户名" 
        		required>
        		<p class="help-block has-error" ng-show="regForm.username.$error.required&&regForm.username.$touched">用户名不能为空</p>
        		<p class="help-block has-error" ng-show="regForm.username.$error.minlength||regForm.username.$error.maxlength">长度为4-10位</p>
        	</div>
        </div>
        <div class="form-group" ng-class="{'has-success':regForm.password.$valid&&regForm.password.$touched,'has-error':regForm.password.$invalid&&regForm.password.$touched}">
        	<label for="" class="col-md-2 control-label">
        		密码
        	</label>
        	<div class="col-md-9">
        		<input type="password" 
        		class="form-control" 
        		ng-model="user.password" 
        		name="password" 
        		ng-pattern="/^[A-Za-z]{1}[0-9A-Za-z_]{2,29}$/"
        		ng-minlength="4" ng-maxlength="10" 
        		placeholder="只能是数字、字母和下划线 " 
        		required>
        		<p class="help-block has-error" ng-show="regForm.password.$error.required
        		&&regForm.password.$touched">密码不能为空</p>
        		<!--正则验证-->
        		<p class="help-block has-error" ng-show="regForm.password.$error.pattern&&regForm.password.$touched">
        		只能是数字、字母和下划线</p>
        		<p class="help-block has-error" 
        		ng-show="regForm.password.$error.minlength||regForm.password.$error.maxlength">密码为4-10位</p>
        	</div>
        </div>
        <div class="form-group" ng-class="{'has-success':regForm.confirmPassword.$valid&&regForm.confirmPassword.$touched,'has-error':(regForm.confirmPassword.$invalid&&regForm.confirmPassword.$touched)||(user.password!=user.confirmPassword&&regForm.confirmPassword.$touched)}">
        	<label for="" class="col-md-2 control-label">
        		确认密码
        	</label>
        	<div class="col-md-9">
        		<input type="password" 
        		class="form-control" 
        		ng-model="user.confirmPassword" 
        		name="confirmPassword" 
        		ng-minlength="4" ng-maxlength="10" 
        		ng-pattern="/^[A-Za-z]{1}[0-9A-Za-z_]{2,29}$/"
        		placeholder="只能是数字、字母和下划线" 
        		required>
        		<p class="help-block has-error" ng-show="regForm.confirmPassword.$error.required&&regForm.confirmPassword.$touched">确认密码不能为空</p>
        		<!--正则验证-->
        		<p class="help-block has-error" ng-show="regForm.confirmPassword.$error.pattern&&regForm.confirmPassword.$touched">只能是数字、字母和下划线</p>
        		<p class="help-block has-error" ng-show="regForm.confirmPassword.$error.minlength||regForm.confirmPassword.$error.maxlength">确认密码为4-10位</p>
        		<p class="help-block has-error" 
        		ng-show="user.password!=user.confirmPassword&&regForm.confirmPassword.$touched">两次密码不一致</p>
        	</div>
        </div>
        <div class="form-group">
        	<div class="col-md-offset-2 col-md-9">
        		<button class="btn btn-primary" ng-disabled="regForm.$invalid">登陆</button>
        	</div>
        </div>
    </form>
