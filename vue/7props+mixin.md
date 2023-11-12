## props

> 让组件传入外部的值

``` javascript

	// 简单写法
	props: ["name","sex","age"]

	// 接受限制列席
	props: {
		name: String,
		sex: String,
		age: Number
	}
	
	// 默认值+必要
	props: {
    name: {
      type: String,
      required: true
      default: "张三"
    }
  }

	// props的值不能被修改
	// 如果要修改，可以放到data里面
	data() {
    return {
      myAge: this.age
    }
  }
	
```



## mixin

67课时

