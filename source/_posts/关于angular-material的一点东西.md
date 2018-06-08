---
title: 关于angular material的一点东西
date: 2017-08-16 17:44:46
categories: 
  - 技术
tags: 
  - angular-material
---

### md-select
当使用ng-repeat时，如果为一般数组时，可以使用一下代码：
````
<div ng-controller="MyCtrl">
  <md-select ng-model="selectedUser">
    <md-option ng-value="user" ng-repeat="user in users">{{ user.name }}</md-option>
  </md-select>
</div>
````
如果数组里为对象时，就要添加ng-model-options属性。
```
<div ng-controller="MyCtrl">
    <md-select ng-model="selectedUser" ng-model-options="{trackBy: '$value.id'}">
        <md-option ng-value="user" ng-repeat="user in users">{{ user.name }}</md-option>
    </md-select>
</div>
```   
