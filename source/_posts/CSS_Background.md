title: CSS Background
date: 2016-04-29 11:33:45
tags: CSS Background
---


### CSS Background

* background
* background-image
* background-size
* background-origin
* background-clip

#### background

顺序

color-->image-->repeat-->attachment-->position

`background: #ffffff url("img_tree.png") no-repeat right top;`

#### background-image

`background-image: url(img_flwr.gif), url(paper.gif);`

也可以使用background代替

`background: url(img_flwr.gif) right bottom no-repeat, url(paper.gif) left top repeat;`

#### background-size

`background-size: 100px 80px;`

background-size: contain； 伸缩图片显示到父容器， 但**不会超出父容器**的范围，所以父容器的有些地反不会被覆盖到。

background-size: cover； 伸缩图片到父容器，会**充满父容器**，所以可能不会把图片的全部内容都显示出来。

#### background-clip

指定背景渲染的范围

* border-box--(default) 图片显示的范围 border+padding+content
* padding-box--图片显示的范围 padding + content
* content-box--图片显示范围 content
 
#### background-clip

指定背景图开始的位置

* border-box--从border的左上角落位置开始
* padding-box--(default) 从padding的左上角落位置开始
* content-box--从content的左上角落位置开始

#### background-color

#### background-repeat

* repeat(default)
* repeat-x
* repeat-y
* no-repeat


#### background-attachment

设置背景图片是fixed还是scrolls

* scroll(default)--The background scrolls along with the element. This is default
* fixed --The background is fixed with regard to the viewport
* local --The background scrolls along with the element's contents

#### background-position

规定了图片开始的位置

如果以left top指定了位置，其默认值为center

```
	left top
	left center
	left bottom
	right top
	right center
	right bottom
	center top
	center center
	center bottom
	
	x% y%
	
	xpos ypos
```


### 问题
 
 * background-position 指定了**图片**开始的位置，Sets the starting position of a background image
 * background-clip --painting area of the background.
 * background-origin --specifies where the background image is positioned.

**这三个的分别怎么用？**




