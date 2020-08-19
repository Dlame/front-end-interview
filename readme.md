# 面试题总结

## 一、HTML和css相关

### 1. [三栏布局](三栏布局.md)

### 2. flex布局

* flex:1；

  `flex`属性是`flex-grow`, `flex-shrink` 和 `flex-basis`的简写，默认值为`0 1 auto`。后两个属性可选。

  flex:1; 是flex:1 1 auto;

* 

## 二、js相关

### 1. 多维数组扁平化

* 方法一 递归

```js
function flat(arr) {
    let newArr = [];
    if (!Array.isArray(arr)) return [];
    function handleArr(arr) {
        // 遍历数组
        for (let i = 0; i < arr.length; i++) {
            let item = arr[i];
            // 判断元素是否为数组
            if (Array.isArray(item)) {
                // 是 递归
                handleArr(arr[i]);
            } else {
                // 否 添加元素至一个新的数组
                newArr.push(arr[i]);
            }
        }
    }
    handleArr(arr);
    return newArr;
}
```

* 方法二 Array.prototype.flat()

```js
let arr = [1, 2, [3, 4, [5, 6, [7, 8]]]];
console.log(arr.flat(Infinity));// [1,2,3,4,5,6,7,8]
```

### 2. 防抖和节流

* #### 防抖（debounce）

所谓防抖，就是指触发事件后 n 秒后才执行函数，如果在 n 秒内又触发了事件，则会重新计算函数执行时间。


* #### 节流（throttle）

所谓节流，就是指连续触发事件但是在 n 秒中只执行一次函数。 节流会稀释函数的执行频率。

### 3. js类的继承

* **es6** 直接使用 extends 关键字

* **es5** 最佳解决方案：

  寄生组合继承：先对超类的原型进行一次浅复制。然后将中间对象的构造函数替换为普通类。为什么要进行这一步？因为对超类的原型进行浅复制以后，中间对象的构造函数变成了Object，需要对该对象进行增强处理。最后将普通类的原型指向中间变量，这样就只需要调用一次超类就可以完成继承。

  ```js
  function OldCar(brand){
      this.brand = brand;
      this.passengers = ['a','b','c']
  }
  OldCar.prototype.getBrand = function(){
      return this.brand;
  }
  function NewCar(name,color){
      OldCar.call(this,name)
      this.color = color;
  }
  
  //继承开始
  var middleObj = Objcet.create(OldCar.prototype);
  middleObj.constructor = NewCar;
  NewCar.prototype = middleObj
  //继承结束
  
  NewCar.prototype.getColor = function(){
      return this.color;
  }
  ```

### 4. this、apply、call、bind

**this 永远指向最后调用它的那个对象**

改变this的指向

- 使用 ES6 的箭头函数：

  **箭头函数的 this 始终指向函数定义时的 this，而非执行时。**

- 在函数内部使用 `_this = this`

- 使用 `apply`、`call`、`bind`

  * apply 和 call 的区别

    两者用法类似，区别在于传入参数不同

    apply

    ```js
    var a ={
        name : "Cherry",
        fn : function (a,b) {
            console.log( a + b)
        }
    }
    
    var b = a.fn;
    b.apply(a,[1,2])     // 3
    ```

    call

    ```js
    var a ={
        name : "Cherry",
        fn : function (a,b) {
            console.log( a + b)
        }
    }
    
    var b = a.fn;
    b.call(a,1,2)       // 3
    ```

    

  * bind 和 apply、call 区别

    bind 是创建一个新的函数，我们必须要手动去调用

    ```js
    var a ={
        name : "Cherry",
        fn : function (a,b) {
            console.log( a + b)
        }
    }
    
    var b = a.fn;
    b.bind(a,1,2)() 
    ```

- new 实例化一个对象

### 5. 

## 三、框架相关

### 1. vuex和v-model结合使用

利用computed的setter和getter，在get()中将vuex中的值返回，在set中获取到值修改到vuex

```js

 <input v-model="getVal" />
 
computed: {
    getVal: {
        get() {
            return this.$store.state.Root.value
        },
         set(newVal) {
             this.$store.commit('handleVal', newVal)
         }
    }
}
```

### 2. history模式打包后刷新出现404

当我们设置了mode为history时，当前端发送路径给服务端时，服务端根本就不认识省去#的url，所以返回给我们404



## 四、网络相关

### 1. get请求和post请求的区别

* `GET` 请求的请求参数是添加到 `head` 中，可以在 `url` 中可以看到；`POST` 请求的请求参数是添加到`BODY`中,在`url` 中不可见。
* 因为请求的`url`有长度限制，这个限制由浏览器和 `web` 服务器决定和设置的。例如IE浏览器对 `URL`的最大限制为2083个字符，如果超过这个数字，提交按钮没有任何反应。因为`GET`请求的参数是添加到`URL`中，所以`GET`请求的`URL`的长度限制需要将请求参数长度也考虑进去。而`POST`请求不用考虑请求参数的长度。
* `GET`请求产生一个数据包;  `POST`请求产生2个数据包,在火狐浏览器中，产生一个数据包。这个区别点在于浏览器的请求机制，先发送请求头，再发送请求体。因为`GET`没有请求体，所以就发送一个数据包，而`POST`包含请求体，所以发送两次数据包，但是由于火狐机制不同，所以发送一个数据包。
* 由于`GET`请求的参数是在`url`中，所以可以直接在浏览器中打开
* `GET` 请求会被浏览器主动缓存下来，留下历史记录，而 `POST` 默认不会。

### 2. Http 缓存策略

**1 )浏览器缓存策略**

浏览器每次发起请求时，先在本地缓存中查找结果以及缓存标识，根据缓存标识来判断是否使用本地缓存。如果缓存有效，则使
用本地缓存；否则，则向服务器发起请求并携带缓存标识。根据是否需向服务器发起HTTP请求，将缓存过程划分为两个部分：
强制缓存和协商缓存，强缓优先于协商缓存。




