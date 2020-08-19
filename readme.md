# 面试题总结

## 一、HTML和css相关

### 1. [三栏布局](三栏布局.md)

### 2.

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

## 三、框架相关

### 1.vuex和v-model结合使用

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

### 2.

