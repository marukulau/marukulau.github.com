---
layout: post
title: "Shecha Touch实例学习"
date: 2012-12-19 15:38
comments: true
categories: [Sencha Touch] 
---

通过前几篇文章的介绍，大家应该对Sencha Touch总体有了了解，现在已经可以开始做一些简单的应用。这一次我们要做的是一个简单通讯录应用。
<!-- more -->
##原型图
开始工作之前我们先把思路整理一下，最好能先把原型图画一下。


##开始编码
- 创建名为AddressBook的Sencha Touch项目。
- 把准备好联系人数据文件contacts.json拷贝到项目根目录下。 
- 修改app.json配置文件。

缓存：
``` js
	"appCache": {
        /**
         * List of items in the CACHE MANIFEST section
         */
        "cache": [
            "index.html",
			"contacts.json" //加入新增的数据文件
        ],
```
资源：
```
	 /**
     * Extra resources to be copied along when build
     */
    "resources": [
		"contacts.json", //加入新增的数据文件
        "resources/images",
        "resources/icons",
        "resources/startup"
    ],
```
### 修改app/view/Main.js文件
```
	Ext.define('AddressBook.view.Main', {
	    extend: 'Ext.navigation.View',
		xtype: 'mainview', //自定义xtype
	    config: {
	        scrollable: 'vertical',
	        items: [
	            {
	                xtype: 'list',
	                title: 'Contacts', //设置navigationView的title
					id: 'contact-list',
	                itemTpl: [
	                    '<div><img src=\'\' /> {firstName}&nbsp;{lastName}</div>' //自定义list cell的模版
	                ],
	                store: 'contacts', //指定数据仓库
	                grouped: true,
	                indexBar: true
	            }
	        ]
	    }
	
	});
```
由于我们需要点击该列表进入对应的联系人详情页，所以应该选择navigation控件，把Main类的父类设为navigation.View:`extend: 'Ext.navigation.View'`。

###新建app/store/contacts.js和app/model/ContactModel.js文件
*contacts.js：*
```
	Ext.define('AddressBook.store.contacts', {
	    extend: 'Ext.data.Store',
	
	    requires: [
	        'AddressBook.model.ContactModel'
	    ],
	
	    config: {
	        autoLoad: true,
	        model: 'AddressBook.model.ContactModel',
	        remoteSort: false,
	        storeId: 'contacts',
	        proxy: {
	            type: 'ajax',
	            url: 'contacts.json'
	        },
	        sorters: 'firstName',
			grouper: {
	            groupFn: function(record) {
	                return record.get('lastName')[0];
	            }
	        }
	
	    }
	});
```
*ContactModel.js：*
```
	Ext.define('AddressBook.model.ContactModel', {
	    extend: 'Ext.data.Model',
	
	    config: {
	        fields: [
	            'firstName',
				'lastName',
				'title'
	        ]
	    }
	});
```

