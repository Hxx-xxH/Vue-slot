## Vue-slot新旧语法笔记

#### 前言

`v-slot` 指令自 Vue 2.6.0 起被引入，提供更好的支持 `slot` 和 `slot-scope` attribute 的 API 替代方案,v-slot整合了slot和slot-scope。在接下来所有的 2.x 版本中 `slot` 和 `slot-scope` attribute 仍会被支持，但已经被官方废弃且不会出现在 Vue 3 中。

#### 1-1.具名插槽(named slots) - 旧

###### 父组件中的代码如下:

```html
<base-layout>
  <template slot="header">
    <h1>Here might be a page title</h1>
  </template>
    
  <h3>默认插槽演示</h3>
  <p>A paragraph for the main content.</p> 
  <p>And another one.</p>
    
  <template slot="footer">
    <p>Here's some contact info</p>
  </template>
</base-layout>
```

###### 或

```vue
<base-layout>
  <h1 slot="header">Here might be a page title</h1>
    
  <h3>默认插槽演示</h3>
  <p>A paragraph for the main content.</p> 
  <p>And another one.</p>
    
  <p slot="footer">Here's some contact info</p>
</base-layout>
```

###### 子组件的代码如下:

```javascript
components: {
        baseLayout: {
            template: `<div> <slot name="header"> </slot><slot name="footer"> </slot></div >`,
            }
        }
    }
```

###### render效果如下:

<img src="./Vue-slot comparison.assets/image-20200527201022025.png" style="zoom:25%;" />

#### 1-2.默认插槽(default slots) - 旧

1-1中注释的代码其实是一个未命名插槽，也就是**默认插槽**，捕获所有未被匹配的内容。

单独演示:

###### 父组件代码与1-1相同:

```html
<base-layout>
	<h1 slot="header">Here might be a page title</h1>
			  
	<h3>默认插槽演示</h3>
	<p>A paragraph for the main content.</p> 
    <p>And another one.</p>
			  
	<p slot="footer">Here's some contact info</p>
</base-layout>
```

###### 子组件代码如下:

```javascript
components: {
        baseLayout: {
            template: ` <div> <slot> </slot></div > `,
            }
        }
    }
```

###### render效果如下:

<img src=".\Vue-slot comparison.assets\image-20200527202229484.png" alt="image-20200527202229484" style="zoom: 25%;" />

#### 2-1.具名插槽(named slots) - 新

###### 父组件代码如下:

```html
<base-layout>
  <template #header>
    <h1>Here might be a page title</h1>
  </template>

  <h3>默认插槽演示</h3>
  <p>A paragraph for the main content.</p>
  <p>And another one.</p>

  <template #footer>
    <p>Here's some contact info</p>
  </template>
</base-layout>
```

###### 或(缩写 v-slot: = #)

```html
<base-layout>
  <template #header>
    <h1>Here might be a page title</h1>
  </template>

  <h3>默认插槽演示</h3>
  <p>A paragraph for the main content.</p>
  <p>And another one.</p>

  <template #footer>
    <p>Here's some contact info</p>
  </template>
</base-layout>
```

###### 子组件代码如下:

```javascript
components: {
		"base-layout": {
			template: `<div><slot name="header"></slot><slot name="footer"></slot></div>`,
		}
	}
```

###### render效果如下:

<img src=".\Vue-slot comparison.assets\image-20200527201022025.png" style="zoom: 25%;" />

#### 2-2.默认插槽(default slots)-新

`<template>` 元素中的所有内容都将会被传入相应的插槽。任何没有被包裹在带有 `v-slot` 的 `<template>` 中的内容都会被视为**默认插槽**的内容。

###### 父组件中代码如下:

```vue
<base-layout>
  <template #header>
    <h1>Here might be a page title</h1>
  </template>

  <h3>默认插槽演示</h3>
  <p>A paragraph for the main content.</p>
  <p>And another one.</p>

  <template #footer>
    <p>Here's some contact info</p>
  </template>
</base-layout>
```

**然而**，如果你希望更明确一些，仍然可以在一个 `<template>` 中包裹默认插槽(#default)的内容：

```html
<base-layout>
  <template v-slot:header>
    <h1>Here might be a page title</h1>
  </template>

  <template v-slot:default>
  	<h3>默认插槽演示</h3>
    <p>A paragraph for the main content.</p>
    <p>And another one.</p>
  </template>

  <template v-slot:footer>
    <p>Here's some contact info</p>
  </template>
</base-layout>
```

###### 子组件中代码如下:

```javascript
components: {
        baseLayout: {
            template: ` <div> <slot> </slot></div > `,
            }
        }
    }
```

<img src=".\Vue-slot comparison.assets\image-20200527202229484.png" style="zoom: 25%;" />

#### 3.后备内容(Fallback Content)

有时为一个插槽设置具体的后备 (也就是默认的) 内容是很有用的，它只会在没有提供内容的时候被渲染。例如:

###### 父组件代码如下:

```html
<current-user>
</current-user>
```

###### 子组件代码如下:

```javascript
components: {
	currentUser: {
		template: `<div><h3>后备内容演示</h3><slot>Tom</slot></div>`,
	}
}
```

###### render效果如下:

<img src=".\Vue-slot comparison.assets\image-20200528135514962.png" alt="image-20200528135514962" style="zoom: 25%;" />

###### 当组件内添加内容替换后备内容:

```html
<current-user>
    Jerry
</current-user>
```

###### render效果如下:

<img src=".\Vue-slot comparison.assets\image-20200528135655090.png" alt="image-20200528135655090" style="zoom: 25%;" />

#### 4.编译作用域(compilation scope)

当用data作为后备内容时

###### 父组件代码如下:

```html
<current-user>
</current-user>
```

###### 子组件代码如下:

```javascript
components: {
	currentUser: {
		template: `<div><h3>后备内容演示</h3><slot>{{user.firstName}}</slot></div>`,
		data(){
			return {
				user:{
					firstName:"Li",
					lastName:"Lei"
				}
			}
		}
	}
}
```

###### render效果如下:

<img src=".\Vue-slot comparison.assets\image-20200528135802139.png" alt="image-20200528135802139" style="zoom:25%;" />

###### 当组件内添加内容用{{user.lastName}}替换后备内容:

```html
<current-user>
	{{user.lastName}}
</current-user>
```

###### render效果如下:

<img src=".\Vue-slot comparison.assets\image-20200527231839192.png" alt="image-20200527231839192" style="zoom: 25%;" />

此时{{user.lastName}}无法被编译,因为lastName是子组件模板中的内容,在父组件模板中是访问不到的.

作为一条规则，请记住：

**父级模板里的所有内容都是在父级作用域中编译的；子模板里的所有内容都是在子作用域中编译的。**

#### 5-1.作用域插槽(scoped slots) - 旧

为了让父组件模板中能访问到user,添加如下代码:

```html
<current-user>
	<template slot="default" slot-scope="slotProps">
        {{ slotProps.user.lastName }}
	</template>
</current-user>
```

这里的 `slot="default"` 可以被忽略为隐性写法：

```html
<current-user>
	<template slot-scope="slotProps">
        {{ slotProps.user.lastName }}
	</template>
</current-user>
```

这里的 `slot-scope` 声明了被接收的 prop 对象会作为 `slotProps` 变量存在于 `<template>` 作用域中。你可以像命名 JavaScript 函数参数一样随意命名 `slotProps`。

即:

```javascript
slotProps = {
	user:{
		firstName:"Li",
		lastName:"Lei"
	},
    ...
}
```

为了让 `user` 在父级的插槽内容中可用，我们可以将 `user` 作为 `<slot>` 元素的一个 attribute 绑定上去：

```javascript
components: {
	currentUser: {
		template: `<div><h3>scope slot演示</h3><slot :user="user">{{user.firstName}}</slot></div>`,
		data() {
			return {
				user: {
					firstName: "Li",
					lastName: "Lei"
				}
			}
		}
	}
}
```

绑定在 `<slot>` 元素上的 attribute 被称为**插槽 prop**。我们选择将包含所有插槽 prop 的对象命名`slotProps`，但你也可以使用任意你喜欢的名字。

###### render效果如下:

<img src=".\Vue-slot comparison.assets\image-20200528140507918.png" alt="image-20200528140507918" style="zoom: 25%;" />

**解构插槽prop**

`slot-scope` 的值可以接收任何有效的可以出现在函数定义的参数位置上的 JavaScript 表达式。这意味着在支持的环境下 ([单文件组件](https://cn.vuejs.org/v2/guide/single-file-components.html)或[现代浏览器](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment#浏览器兼容))，你也可以在表达式中使用 [ES2015 解构](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment#解构对象)，如下:

```html
<current-user>
	<template slot-scope="{user}">
        {{ user.lastName }}
	</template>
</current-user>
```

即:

```javascript
let {user}={
			user:{
				firstName:"Li",
				lastName:"Lei"
			}
		}
user//{
		firstName:"Li",
		lastName:"Lei"
	}
```

render效果与上面的代码相同

也可以利用解构默认值会插槽添加后备内容

```html
<current-user>
	<template slot-scope="{user={lastName:'Lei'}}">
        {{ user.lastName }}
	</template>
</current-user>
```

当插槽Prop为`undefined`:

```javascript
components: {
	currentUser: {
		template: `<div><h3>scope slot演示</h3><slot></slot></div>`,
	}
}
```

render效果如下:

<img src=".\Vue-slot comparison.assets\image-20200528140507918.png" style="zoom: 25%;" />

#### 5-2.作用域插槽(scoped slots) -新

###### 父组件中代码如下:

```html
<current-user>
	<template v-slot:default="slotProps">
        {{ slotProps.user.lastName }}
	</template>
</current-user>
```

###### 或

```html
<current-user>
	<template #default="slotProps">
        {{ slotProps.user.lastName }}
	</template>
</current-user>
```

##### **独占默认插槽的缩写写法**

在上述情况下，当被提供的内容*只有默认*插槽时，组件的标签才可以被当作插槽的模板来使用.这样我们就可以把 `v-slot` 直接用在组件上：

```html
<current-user v-slot:default="slotProps">
  {{ slotProps.user.lastName }}
</current-user>
```

这种写法还可以更简单。就像假定未指明的内容对应默认插槽一样，不带参数的 `v-slot` 被假定对应默认插槽：

```html
<current-user v-slot="slotProps">
  {{ slotProps.user.lastName }}
</current-user>
```

注意默认插槽的缩写语法**不能**和具名插槽混用，因为它会导致作用域不明确：

```html
<!-- 无效，会导致警告 -->
<current-user v-slot="slotProps">
  {{ slotProps.user.lastName }}
  <template v-slot:other="otherSlotProps">
    slotProps is NOT available here
  </template>
</current-user>
```

###### 报错:

<img src=".\Vue-slot comparison.assets\image-20200528002851416.png" alt="image-20200528002851416" style="zoom: 25%;" />

To avoid scope ambiguity, the default slot should also use <template> syntax when there are other named slots.为了避免作用域不明确,当存在其他具名插槽混用时,默认插槽应该用<template> 标签语法.

###### 即:

```html
<current-user>
  <template v-slot="slotProps">
    {{ slotProps.user.lastName }}
  </template>

  <template #other="otherSlotProps">
      {{otherSlotProps.otherUser}}
  </template>
</current-user>
```

###### 子组件模板中的代码如下:

```javascript
components: {
	currentUser: {
		template: `<div><h3>scope slot演示</h3><slot :user="user">{{user.firstName}}</slot><br><slot name="other" :otherUser="otherUser"></slot></div>`,
		data() {
			return {
				user: {
					firstName: "Li",
					lastName: "Lei"
				},
				otherUser:"Tom"
			}
		}
	}
}
```

###### render效果如下:

<img src=".\Vue-slot comparison.assets\image-20200528140832014.png" alt="image-20200528140832014" style="zoom: 25%;" />