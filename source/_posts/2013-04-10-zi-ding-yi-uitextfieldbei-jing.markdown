---
layout: post
title: "自定义UITextField背景"
date: 2013-04-10 10:59
comments: true
categories: [Object C, iOS] 
---

##设置背景图片

	UIImage *textFieldBgImage = [[UIImage imageNamed:@"textfield_bg.png"] stretchableImageWithLeftCapWidth:5 topCapHeight:5];
	[self.textField setBackground:textFieldBgImage];

##修改文字边距

设置好图片后输入文字会发现左边的文字和背景图片的边框重叠了，需要设置一下文本框的边距，可是UITextField没有相应的属性可以设置，所以只有重写UITextField的相关方法。


    @implementation UITextField(UITextFieldCategory)
    
    - (CGRect)textRectForBounds:(CGRect)bounds {
        CGRect inset = CGRectInset(bounds, 5, 5);
        return inset;
    }
    
    - (CGRect)editingRectForBounds:(CGRect)bounds {
        CGRect inset = CGRectInset(bounds, 5, 5);
        return inset;
    }
    
    @end

上面偷了个懒，直接用category的方式重写了这个两个方法。
