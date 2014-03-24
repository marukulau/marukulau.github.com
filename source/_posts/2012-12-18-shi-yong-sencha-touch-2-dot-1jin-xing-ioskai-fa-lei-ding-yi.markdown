---
layout: post
title: "使用Sencha Touch 2.1进行iOS开发 —— 类的定义与使用"
date: 2012-12-18 13:57
comments: true
categories: [Sencha Touch]
---

##类的定义
Sencha Touch有自己的类定义方式，我们先来看个例子： 
``` js
	Ext.define('Animal', {
        config: {
            name: null
        },
     
        constructor: function(config) {
            this.initConfig(config);
        },
     
        speak: function() {
            alert('grunt');
        }
    });
```
上面我们简单地定义了一个Animal类，只有一个name属性和一个方法speak();  
<!-- more -->
##类的继承
``` js
	Ext.define('Human', {
	    extend: 'Animal',
	 
	    speak: function() {
	        alert(this.getName());
	    }
	});
``` 
我们定义了一个Human类继承自Animal，并重写speak()方法。

##类的实例化
``` js
	var bob = Ext.create('Human', {
		name: 'Bob'
    });
 
	bob.speak(); //alerts 'Bob'
``` 
通过Ext.create()静态方法创建类实例。

##getter和setter方法
对应的属性会自动生成getter和setter方法，如上面继承的例子中的this.getName()，setter方法为：this.setName('')。  
``` js
	Ext.define('Human', {
	    extend: 'Animal',
	 
	    applyName: function(newName, oldName) {
	        return confirm('Are you sure you want to change name to ' + newName + '?')? newName : oldName;
	    }
	});
``` 
applyName()方法是在调用setter方法后自动回调的方法，上面例子将在调用setter方法后弹出确认窗口询问是否修改name的值，点击no的话则不进行修改。  
``` js
	Ext.define('Human', {
	    extend: 'Animal',
	 
	    updateName: function(newName, oldName) {
	        alert('Name changed. New name is: ' + newName);
	    }
	});
``` 
updateName()方法则是在调用setter方法后并且改属性值已经修改了的情况下自动回调的。

##依赖和动态加载
有时候我们需要在类里面使用某个类的，这时候我们需要加入这个类的引用声明：requires: 'Ext.MessageBox'。
``` js
	Ext.define('Human', {
	    extend: 'Animal',
	 
	    requires: 'Ext.MessageBox',
	 
	    speak: function() {
	        Ext.Msg.alert(this.getName(), "Speaks...");
	    }
	});
``` 
加入引用声明后，Sencha Touch会自动判断是否Ext.MessageBox已经加载，如果未加载的话则会自动通过AJAX加载对应的类文件。

##命名空间
以上一篇文章的Main.js文件为例
``` js
	Ext.define('HelloSenCha.view.Main', {
	    extend: 'Ext.navigation.View',
	    xtype: 'mainview',
	
	    requires: [
	        'HelloSenCha.view.Contacts',
	        'HelloSenCha.view.contact.Show',
	        'HelloSenCha.view.contact.Edit'
	    ],
	...
``` 
类名规则为：项目名称.目录1.目录2...类名，也就是说，Main.js和Contacts.js文件在view文件夹下，Show.js和Edit.js在view/contact文件夹下。
