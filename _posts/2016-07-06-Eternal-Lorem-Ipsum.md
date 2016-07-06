---
layout: post
title: 文字超出div显示为…或者隐藏（css）
author: juily
---
## 文字超出div显示为…或者隐藏
-----


一般的文字截断（适用于内联与块）：

	.text-overflow {
		display:block;/*内联对象需加*/
		width:31em;
		word-break:keep-all;/* 不换行 */
		white-space:nowrap;/* 不换行 */
		overflow:hidden;/* 内容超出宽度时隐藏超出部分的内容 */
		text-overflow:ellipsis;/* 当对象内文本溢出时显示省略标记(...)；需与overflow:hidden;一起使用。*/
	}

对于表格文字溢出的定义：

	table{
		width:30em;
		table-layout:fixed;/* 只有定义了表格的布局算法为fixed，下面td的定义才能起作用。 */
	}
	td{
		width:100%;
		word-break:keep-all;/* 不换行 */
		white-space:nowrap;/* 不换行 */
		overflow:hidden;/* 内容超出宽度时隐藏超出部分的内容 */
		text-overflow:ellipsis;/* 当对象内文本溢出时显示省略标记(...) ；需与overflow:hidden;一起使用。*/
	}


