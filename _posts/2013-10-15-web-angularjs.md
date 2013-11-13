---
layout: post
title: AngularJs 学习笔记
category: web
---


  1. ng-app ：引导引用标记
  
 		手动初始化： 
 		angular.element(document).ready(function(){
 			angular.bootstrap(document); 
 		})
 		
  2. {{}}：使用双大括号标记{{}}的内容是问候语中绑定的表达式
  3. ng-model : 绑定变量
  
 		<input type="text" ng-model="yourname" placeholder="World">         
 		<hr>  Hello {{yourname || 'World'}}!
 		
  4. ng-controller: 指定对应 controller，eg：ng-controller="PhoneListCtrl"
 		
 		var phonecatApp = angular.module('phonecatApp', []); 
 		phonecatApp.controller('PhoneListCtrl', function PhoneListCtrl($scope){     
 			$scope.phones = [ 
 				{'name': 'Nexus S', 'snippet': 'Fast just got faster with Nexus S.'},         
 				{'name': 'Motorola XOOM™ with Wi-Fi', 'snippet': 'The Next, Next Generation tablet.'},         
 				{'name': 'MOTOROLA XOOM™', 'snippet': 'The Next, Next Generation tablet.'}     
 			]; 
 		});
 			
  5. ng-repeat ： ng-repeat="phone in phones"
  6. filter: 过滤
  
 		<input ng-model="query"> ng-repeat="phone in phones | filter:query" 
  
  7. ng-bind-template: 绑定默认不显示原生
 
 		<title ng-bind-template="Google Phone Gallery: {{query}}">Google Phone  Gallery</title>
 		
  8. orderBy: 排序
  
 		<￼￼￼￼￼￼￼￼￼￼￼￼￼￼￼￼￼￼select ng-model="orderProp">     
 			<option value="name">Alphabetical</option>     
 			<option value="age">Newest</option> 
 		</select> 
 		<li ng-repeat="phone in phones | filter:query | orderBy:orderProp"> 
 		    {{phone.name}}<p>{{phone.snippet}}</p> 
 		</li>
 		
  9. ng-src：图片地址
  
 		<img ng-src="{{phone.imageUrl}}">   
 		
  10. ng-view: 为当前路由把对应的视图模板载入到布局模板中
  11. ng-click：绑定点击事件
	
		<img ng-src="{{mainImageUrl}}" class="phone">
		<ul class="phone-thumbs">    
			<li ng-repeat="img in phone.images">
				<img ng-src="{{img}}" ng-click="setImage(img)">    
			</li>
		</ul>

避免压缩为了克服压缩引起的问题,只要在控制器函数里面给$inject属性赋值一个依赖服务标识符的数组,就像被注释掉那段 最后一行那样:PhoneListCtrl.$inject = ['$scope', '$http'];另一种方法也可以用来指定依赖列表并且避免压缩问题——使用Javascript数组方式构造控制器:把要注入的服务放 到一个字符串数组(代表依赖的名字)里,数组最后一个元素是控制器的方法函数: 

	var PhoneListCtrl = ['$scope', '$http', function($scope, $http) { 
		/* constructor body */ 
	}]
		
