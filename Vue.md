```js
{
var a = 10;  //全局变量
let b = 10;  // 局部变量
}
console.log(a) // 
console.log(b) //  Uncaught ReferenceError: b is not defined
```



```js
{
    let m = 3;
    let m = 4;
    console.log(b);//Identifier 'm' has already been declared
}
```

```js
        <input type="text"  :value="content"> 单向绑定
        <input type="text" v-model = "content"> 双向绑定

       //事件绑定
        <button v-on:click="search">search</button>
        <button @click="search">search</button>
```



==:value==单向绑定

node.js

js运行环境





