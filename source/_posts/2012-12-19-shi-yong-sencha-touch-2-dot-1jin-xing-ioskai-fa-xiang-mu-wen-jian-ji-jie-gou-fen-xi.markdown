---
layout: post
title: "使用Sencha Touch 2.1进行iOS开发 —— 项目文件结构及代码分析"
date: 2012-12-19 09:20
comments: true
categories: [Sencha Touch] 
---

从第一篇文章我们知道了如何通过sencha命令创建Sencha Touch项目，而创建的项目里面已经包含了不少文件，如要开始写代码，我们有必要先了解所创建项目的文件结构及代码。
<!-- more -->
##文件结构
在终端输入

	$ sencha generate app HelloWorld ~/Desktop/HelloWorld

我们创建了一个名为HelloWorld的项目，进入该文件夹可以看到文件如下：

	app/
		controller/
		model/
    	profile/
    	store/
    	view/
			Main.js
	app.js
	app.json
	build.xml
	index.html
	packager.json
	resources/
	touch/
  
- packager.json 一些sencha package命令要用到的文件，里面是一些编译打包配置信息。
- app.js 包含了应用的初始化逻辑代码。
- app.json 应用部署的配置。
- app/ MVC结构的应用源代码文件。
- resources/ 资源文件夹。
- touch/ Sencha Touch SDK文件。

##代码分析
###应用入口
app.js是应用的初始化文件，也就是意味着应用的入口在这里，打开该文件，我可以看到这一段代码：

	launch: function() {
        // Destroy the #appLoadingIndicator element
        Ext.fly('appLoadingIndicator').destroy();

        // Initialize the main view
        Ext.Viewport.add(Ext.create('HelloWorld.view.Main'));
    },

`Ext.create('HelloWorld.view.Main')`创建了一个HelloWorld.view.Main实例并加入到Viewport中，也就是说我们进入应用看到的主界面的代码源文件是app/view/Main.js文件。

##xtype
打开Main.js文件，上一篇文章已经讲过类的定义了，所以HelloWorld.view.Main类的定义应该比较好理解。需要注意的是里面的`xtype: 'main'`属性，这个是定义当前类的xtype，方便被其他类用进行引用，而config下items的xtype则是对其他已定义xtype的类的引用。

下面是SDK所有的xtype及其对应的类：

	xtype                   Class
	-----------------       ---------------------
	actionsheet             Ext.ActionSheet
	audio                   Ext.Audio
	button                  Ext.Button
	component               Ext.Component
	container               Ext.Container
	image                   Ext.Img
	label                   Ext.Label
	loadmask                Ext.LoadMask
	map                     Ext.Map
	mask                    Ext.Mask
	media                   Ext.Media
	panel                   Ext.Panel
	segmentedbutton         Ext.SegmentedButton
	sheet                   Ext.Sheet
	spacer                  Ext.Spacer
	title                   Ext.Title
	titlebar                Ext.TitleBar
	toolbar                 Ext.Toolbar
	video                   Ext.Video
	carousel                Ext.carousel.Carousel
	carouselindicator       Ext.carousel.Indicator
	navigationview          Ext.navigation.View
	datepicker              Ext.picker.Date
	picker                  Ext.picker.Picker
	pickerslot              Ext.picker.Slot
	slider                  Ext.slider.Slider
	thumb                   Ext.slider.Thumb
	tabbar                  Ext.tab.Bar
	tabpanel                Ext.tab.Panel
	tab                     Ext.tab.Tab
	viewport                Ext.viewport.Default
	
	DataView Components
	---------------------------------------------
	dataview                Ext.dataview.DataView
	list                    Ext.dataview.List
	listitemheader          Ext.dataview.ListItemHeader
	nestedlist              Ext.dataview.NestedList
	dataitem                Ext.dataview.component.DataItem
	
	Form Components
	---------------------------------------------
	checkboxfield           Ext.field.Checkbox
	datepickerfield         Ext.field.DatePicker
	emailfield              Ext.field.Email
	field                   Ext.field.Field
	hiddenfield             Ext.field.Hidden
	input                   Ext.field.Input
	numberfield             Ext.field.Number
	passwordfield           Ext.field.Password
	radiofield              Ext.field.Radio
	searchfield             Ext.field.Search
	selectfield             Ext.field.Select
	sliderfield             Ext.field.Slider
	spinnerfield            Ext.field.Spinner
	textfield               Ext.field.Text
	textareafield           Ext.field.TextArea
	textareainput           Ext.field.TextAreaInput
	togglefield             Ext.field.Toggle
	urlfield                Ext.field.Url
	fieldset                Ext.form.FieldSet
	formpanel               Ext.form.Panel


