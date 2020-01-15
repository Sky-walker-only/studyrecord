# ES6



## 字符串

##### includes

判断是否包含某个字符串，并返回boolean

```javascript
const str = 'hellow Word!'
console.log(str.includes('h'))		//true
```



#####  repeat

获取字符串，并重复n次

```javascript
const str = 'he'
console.log(str.repeat(2))		//'hehe'
```



##### startWith/endWith

判断字符串是否是以‘特定字符’开始/结束，并返回boolean

```javascript
const str = 'hellow Word!'
console.log(str.startWith('h'))		//true
console.log(str.endWith('h'))		//true
```





## 对象



##### Object.assign()--浅复制

将任意多个源对象的可枚举属性，拷贝到目标对象，方法的第一个参数将作为目标对象，并返回。

``` javascript
 const objA = { name: 'cc', age: 18 }
    const objB = { address: 'beijing' }
    const objC = {} // 这个为目标对象
    const obj = Object.assign(objC, objA, objB)

    // 我们将 objA objB objC obj 分别输出看看
    console.log(objA)   // { name: 'cc', age: 18 }
    console.log(objB) // { address: 'beijing' }
    console.log(objC) // { name: 'cc', age: 18, address: 'beijing' }
    console.log(obj) // { name: 'cc', age: 18, address: 'beijing' }

    // 是的，目标对象ObjC的值被改变了。
    // so，如果objC也是你的一个源对象的话。请在objC前面填在一个目标对象{}
    Object.assign({}, objC, objA, objB)
```





## 展开运算符--Spreated Operator

组装数组或者对象，利用解构的方法获取部分数组，对象的值



```javascript
	//数组
    const color = ['red', 'yellow']
    const colorful = [...color, 'green', 'pink']		//组装新数组
    console.log(colorful) //[red, yellow, green, pink]
    
    //对象
    const alp = { fist: 'a', second: 'b'}
    const alphabets = { ...alp, third: 'c' }			//组装新对象
    console.log(alphabets) //{ "fist": "a", "second": "b", "third": "c"}


	//数组取值
    const number = [1,2,3,4,5]
    const [first, ...rest] = number
    console.log(rest) //2,3,4,5
    //对象取值
    const user = {
        username: 'lux',
        gender: 'female',
        age: 19,
        address: 'peking'
    }
    const { username, ...rest } = user
    console.log(rest) //{"address": "peking", "age": 19, "gender": "female"}


	//对象组装中，重名键名的覆盖！
	const first = {
        a: 1,
        b: 2,
        c: 6,
    }
    const second = {
        c: 3,
        d: 4
    }
    const total = { ...first, ...second }
    console.log(total) // { a: 1, b: 2, c: 3, d: 4 }

```



## import & export

注意：es6允许将整个模块整体导出

一个文件里，有且只能有一个export default，export可以有多个导出，并用{}接收

```javascript
import * as example from "./example.js"
console.log(example.name)
console.log(example.age)
console.log(example.getName())
```





## Promise

用同步的方式，写异步代码

```javascript
 setTimeout(function() {
      console.log(1)
    }, 0);
    new Promise(function executor(resolve) {
      console.log(2);
      for( var i=0 ; i<10000 ; i++ ) {
        i == 9999 && resolve();
      }
      console.log(3);
    }).then(function() {
      console.log(4);
    });
    console.log(5);
```

以上代码打印结果

![promise-1](D:\workspace\md\img-md\promise-1.png)



### Generators生成器和迭代器

当你调用一个generator时，它将返回一个迭代器对象。这个迭代器对象拥有一个叫做next的方法来帮助你重启generator函数并得到下一个值。next方法不仅返回值，它返回的对象具有两个属性：done和value。value是你获得的值，done用来表明你的generator是否已经停止提供值。

```javascript
    // 生成器
    function *createIterator() {
        yield 1;
        yield 2;
        yield 3;
    }
    
    // 生成器能像正规函数那样被调用，但会返回一个迭代器
    let iterator = createIterator();
    
    console.log(iterator.next().value); // 1
    console.log(iterator.next().value); // 2
    console.log(iterator.next().value); // 3
    
    
     function run(taskDef) { //taskDef即一个生成器函数

        // 创建迭代器，让它在别处可用
        let task = taskDef();

        // 启动任务
        let result = task.next();
    
        // 递归使用函数来保持对 next() 的调用
        function step() {
    
            // 如果还有更多要做的
            if (!result.done) {
                result = task.next();
                step();
            }
        }
    
        // 开始处理过程
        step();
    
    }
```

生成器可以让我们的代码进行等待。就不用嵌套的回调函数。使用generator可以确保当异步调用在我们的generator函数运行一下行代码之前完成时暂停函数的执行。



### 数组去重

```javascript
/*
* 数组去重
* */
unique(arr) {
  return Array.from(new Set(arr))
},
```



























