---
layout: post
title: 用CSS隐藏元素的五种方法
---

![](/images/Bing_710.JPG)

## 1. opacity: 0


    .hide {
      opacity: 0;
    }      

相当于设置元素为透明
屏幕阅读器可读, 可以继续交互

## 2. visibility: hidden

    .hide {
      visibility: hidden;
    }      

屏幕阅读器不可读.

重新成visible后就可以重新进行交互了
后续的子节点可以设置为visible而变得可见

## 3. display: none

	.hide {
      display: none;
	}

该元素不会被渲染, 所有后续的子节点也都是不会显示的
只可以通过dom api进行操作
屏幕阅读器不可读.

## 4. move out the viewport

	.hide {
	   position: absolute;
	   top: -9999px;
	   left: -9999px;
	}

该方法的目的是想保留交互的功能,同时不要影响布局.
屏幕阅读器可读.

## 5. clip-path

	.hide {
	  clip-path: polygon(0px 0px,0px 0px,0px 0px,0px 0px);
	}

通过裁剪的方法来隐藏.之前大家会用clip属性,现在clip属性已经废弃了,可以使用新的clip-path属性
想深入了解的可以看: [介绍clip-path属性](https://www.sitepoint.com/introducing-css-clip-path-property/)

不过现在IE对clip-path还不支持

## 总结

用一张表来总结各种方法的区别:

| 方法        | Occupy its position | User interaction  | Screen Reader | Transition Animation| IE Support |
| ------------- |:-------------:| -----:|-----:|-----:|-----:|
| opacity: 0      | yes | yes | yes | yes | yes |
| visibility: hidden | yes | no | no | no | yes |
| display: none | no | no | no | no | yes |
| move out the viewport | no | yes | yes | yes | yes |
| clip-path | yes | no |yes| yes | no |

