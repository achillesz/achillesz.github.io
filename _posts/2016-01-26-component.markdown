--- 
layout: post
title: "Component介绍"
date:   2016-01-26 11:41:11
keywords: js,google closure,component
description: js,google closure,component
categories: [javascript]
tags: [google closure]
---

`google closure` 一个组件 `component` 的用法

```js
	$.ui.Component = function(opt_doc) {
		this.document_ = opt_doc || window.document;
	};
```

`new` 的时候保存一个所在文档的属性
	
## 方法说明：
			
#### `createElement` 
	 
```js
	return $(html, this.document_);
```
当前文档下创建DOM元素,这么看涉及到 `createDom` 的时候还是应该用 `createElement`, 也可以 `createDom` 继承一下，走默认调用，确保在正确的文档里
	
	
#### createDom

```js
	this.element_ = this.createElement("<div>");
```

需要创建element_ 的属性，一般需要重写。注意一定要把创建的东西放在一个容器中。可以继承一下
	
#### setElementInternal

```js
	this.element_ = element;
```

一般说来，没啥用
	
#### getContent

```js
	return this.content_;
```

返回一个内容
	
#### setContent

```js
	$.ui.Component.prototype.setContent = function(content) {
		this.content_ = content;
		this.setContentInternal(content);
	};
```

保存内容，并把内容设置到适当的元素上。（比如比如的element_里面，或者可以指定，指定需要看下面方法，重写实现）
	
#### setContentInternal

```js
	$.ui.Component.prototype.setContentInternal = function(content) {
    	var elem = this.getContentElement();
    	elem && elem.html(content);
    };
```

把内容放入 `this.getContentElement()` 返回的元素里，默认值为 `this.element_` 

#### getContentElement

```js
	$.ui.Component.prototype.getContentElement = function() {
    	return this.element_;
    };
```

表示内容元素 `content` 的容器

#### render

```js

	$.ui.Component.prototype.render = function(opt_parentElement) {
    	this.render_(opt_parentElement);
    };
    
    $.ui.Component.prototype.renderBefore = function(siblingElement) {
    	this.render_(siblingElement.parentNode || siblingElement.parent(), siblingElement);
    };
```

传入一个 `JQ` 元素，或者是 `DOM` 元素，我们一般使用JQ. 都调用了 `render_`

#### render_

```js

	$.ui.Component.prototype.render_ = function(opt_parentElement, opt_beforeElement) {
    	if (this.inDocument_) {
    		throw Error('ALREADY_RENDERED');
    	}
    	
    	if (!this.element_) {
    		this.createDom();
    	}
    	
    	if(this.element_.jquery) {
    		if (opt_beforeElement) {
    			this.element_.insertBefore(opt_beforeElement);
    		} else if (opt_parentElement) {
    			this.element_.appendTo(opt_parentElement);
    		} else {
    			this.element_.appendTo('body');
    		}
    	} else {
    		if (opt_parentElement) {
    			opt_parentElement.insertBefore(this.element_, opt_beforeElement || null);
    		} else {
    			document.body.appendChild(this.element_);
    		}
    	}
    	
    	
    	if(!this.parent_ || this.parent_.isInDocument()) {
    		this.enterDocument();
    	}
    };
    
```

调用的时候如果已经进入文档 `throw error`

自动调用 `createDom`

还有另一个自动调用 条件是 `!this.parent_ || this.parent_.isInDocument()` 包含2个意思

1. 不存在父元素
2. 父元素存在已经进入文档
反过来看是：存在父元素，父元素没进入文档，那么子元素就不进入文档，这条很重要，可以想象，那父元素进入文档的时候，也一定会触发子元素进入文档。看看是不是？

#### enterDocument

```js

	$.ui.Component.prototype.enterDocument = function() {
		this.inDocument_ = true;
		
		this.forEachChild(function(i, child) {
			if (!child.isInDocument() && child.getElement()) {
				child.enterDocument();
			}
		});
	};
	
```

1. 进入文档，
2. 遍历子元素。
	* 子元素没有进入文档且存在 `element_`
	* 反过来：是不是同意子元素可以先进入文档 但从进入文档的自动调用机制来看，是没有这种可能的。
	* 也就是子元素可以先 `render` 但不会进入文档，而在父元素 `render` 的时候 再去进入子文档
	* 情况比较复杂： 需要先看一下子元素是如何插入的
	
####

```js

	$.ui.Component.prototype.addChild = function(child, opt_render) {
		this.addChildAt(child, this.getChildCount(), opt_render);
	};
		
```

```js

	$.ui.Component.prototype.addChildAt = function(child, index, opt_render) {
    		if (child.inDocument_ && (opt_render || !this.inDocument_)) {
    			throw Error('ALREADY_RENDERED');
    		}
    		if (index < 0 || index > this.getChildCount()) {
    			throw Error('CHILD_INDEX_OUT_OF_BOUNDS');
    		}
    		
    		// Create the index and the child array on first use.
    		if (!this.childIndex_ || !this.children_) {
    			this.childIndex_ = {};
    			this.children_ = [];
    		}
    		
    		// Moving child within component, remove old reference.
    		if (child.getParent() == this) {
    			$.object.set(this.childIndex_, child.getId(), child);
    			$.array.remove(this.children_, child);
    		} else {
    			$.object.add(this.childIndex_, child.getId(), child);
    		}
    		
    		child.setParent(this);
    		$.array.insertAt(this.children_, child, index);
    		
    		if (child.inDocument_ && this.inDocument_) {
    			var contentElement = this.getContentElement();
    			contentElement.insertBefore(child.getElement(),
    				(this.getChildAt(index).getElement() || null));
    			/*var sibling = this.getChildAt(index + 1);
    			
    			if (sibling) {
    				sibling.getElement().before(child.getElement());
    			} else {
    				this.getContentElement().append(child.getElement());
    			}*/
    		} else if (opt_render) {
    			if (!this.element_) {
    				this.createDom();
    			}
    			this.renderChildAt(child, index);
    		} else {
    			if (this.inDocument_ && !child.inDocument_ && child.getElement()) {
    				child.enterDocument();
    			}
    		}
    	};	
```

最复杂的一个函数:
1. 实现父子关系的管理
2. 根据当前状态
	* 都进入了文档。再去 `addChild` 几乎没遇到过，写法应该有点问题。
	* 子元素先进入文档的例子，没有碰到过。 父元素先进入文档，子元素没有进入，有参数true 创建元素，`renderChildAt`
	* 无参数 父元素进入，子元素没进入，子元素存在元素（即 this.element_） 这种情况下添加的子元素不存在 `dom` 上的关联，可以先创建子元素，那么就会顺带调用子元素进入文档，或者子元素自己单独实现进入文档。
	
```js
	$.ui.Component.prototype.renderChildAt = function(child, index) {
    	var sibling = this.getChildAt(index + 1);
    	child.render_(this.getContentElement(), sibling ? sibling.getElement() : null);
    };
```

一般都在最后的原因，因为 `index+1` 是不存在的子元素，一共3个地方用到 `render_` 现在全了。子元素的插入规则就是如此，注意 `render_` 的位置 `this.getContentElement` 经常需要重成你希望的位置

#### removeChild

```js

	$.ui.Component.prototype.removeChild = function(child) {
    	if(child) {
    		$.object.remove(this.childIndex_, child.getId());
    		$.array.remove(this.children_, child);
    		
    		child.exitDocument();
    		child.setParent(null);
    		var elem = child.getElement();
    		if(elem) {
    			if(elem.remove)
    				elem.remove();
    			else
    				elem.parentNode && elem.parentNode.removeChild(elem);
    		}
    	}
    };
    
```

1. 删除某一个子元素
2. 先删除内部数据引用
3. 删除DOM `elem.remove` 表示是 `jquery` 对象

#### getChildAt

```js
	$.ui.Component.prototype.getChildAt = function(index) {
    	return this.children_ ? this.children_[index] || null : null;
    };
```

支持索引获取子元素，很少使用索引的时候。。。

#### getChildren

```js
	
	$.ui.Component.prototype.getChildren = function() {
		return this.children_ ? this.children_ : [];
	};

```

获取子元素


#### getChild

```js
	$.ui.Component.prototype.getChild = function(id) {
      return (this.childIndex_ && id) ? this.childIndex_[id] || null : null;
    };
```

根据 `id` 获取元素

#### getId

```js

	$.ui.Component.prototype.setId = function(id) {
    	if (this.parent_ && this.parent_.childIndex_) {
    		$.object.remove(this.parent_.childIndex_, this.id_);
    		$.object.add(this.parent_.childIndex_, id, this);
    	}
    	this.id_ = id;
    };
    
    
    $.ui.Component.prototype.getId = function() {
    	return this.id_ || (this.id_ = ':' + ($.guid++));
    };
    
```

`getId` 自带设值功能，没有用到重写设置 `setId` 的情况

#### forEachChild

```js
	$.ui.Component.prototype.forEachChild = function(fn, opt_this) {
		var this_ = opt_this || this;
		if (this.children_) {
			for(var i = 0, l = this.children_.length; i < l; i++) {
				fn.call(this_, i, this.children_[i]);
			}
		}
	};
```

遍历子元素数组，执行某个函数，指定上下文，参数为索引，和某一个 `child`


#### decorate

```js
	$.ui.Component.prototype.decorate = function(element) {
		this.element_ = element;
		this.enterDocument();
	};
```

另一种方式，比如 `ajaxform` 不需要 `DOM` 相关处理，以及父子元素处理，值使用了 `event` 相关东西

#### dispose

```js

	$.Disposable.prototype.dispose = function() {
      if (!this.disposed_) {
        // Set disposed_ to true first, in case during the chain of disposal this
        // gets disposed recursively.
        this.disposed_ = true;
        this.disposeInternal();
      }
    };
    
    
    $.Disposable.prototype.disposeInternal = function() {
      // No-op in the base class.
    };
    
    
    $.ui.Component.prototype.disposeInternal = function() {
    	$.ui.Component.superClass_.disposeInternal.call(this);
    	
    	if (this.inDocument_) {
    		this.exitDocument();
    	}
    	
    	// Disposes of the component's children, if any.
    	this.forEachChild(function(i, child) {
    		child.dispose();
    	});
    	
    	// Detach the component's element from the DOM, unless it was decorated.
    	if (this.element_) {
    		(this.element_.remove && this.element_.remove()) ||
    		(this.element_.parentNode && this.element_.parentNode.removeChild(this.element_));
    	}
    	
    	this.children_ = null;
    	this.childIndex_ = null;
    	this.element_ = null;
    	this.parent_ = null;
    	// TODO(user): delete this.dom_ breaks many unit tests.
    };
```

一般时候渲染完成就OK了，但特殊情况还需要清掉，比如切换一个类型之后，要清掉当前的体系，清掉的过程，需要退出文档，子元素先清掉，然后父元素DOM 移除，去掉引用相关

####

```js

	$.ui.Component.prototype.removeChildren = function(opt_unrender) {
		while (this.hasChildren()) {
			this.removeChildAt(0);
		}
	};
	
```

只能把子元素移除掉，但不能改变父元素（所以彻底的删除的话要用 `dispose`）














