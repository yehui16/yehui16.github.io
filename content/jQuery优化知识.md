[返回首页](../README.md)

## jQuery优化知识

前端开发中循环遍历的方法有很多，往往我们分不清楚哪种情况该用哪种方法，下面就总结了常用循环遍历的使用方法。

- ### for

    for的基本用法如下：

    ```js
    for (语句 1; 语句 2; 语句 3) {
        //要执行的代码块
    }
    ```

    语句 1 在循环（代码块）开始之前执行。  
    语句 2 定义运行循环（代码块）的条件。  
    语句 3 会在循环（代码块）每次被执行后执行。  

    >注：使用时，不要把长度获取写在语句中，先缓存循环的长度，这样避免每次循环都会去计算循环对象的长度

    for **可以**正确响应break、continue,**不可以**响应return语句。

- ### for in

    ```js
    var obj = {a: 1, b: 2, c: 3};
    for (var i in obj) {
        console.log('键名：', i);
        console.log('键值：', obj[i]);
    }
    ```

    >输出：键名:a 键值:1， 键名:b 键值:2  

    **for in遍历数组的毛病:**  
    根据属性名遍历，所以key的类型是string；  
    遍历顺序有可能不是按照实际数组的内部顺序;  
    使用for in会遍历数组所有的可枚举属性，包括原型上属性。可以使用hasOwnProperty判断一个属性是不是对象自身上的属性。
    obj.hasOwnProperty(keys)==true 表示这个属性是对象的成员属性，而不是原型属性。  
    因此，for in更适合遍历对象，不要使用for in遍历数组。  

    ```js
    for (const key in arr) {
        if (arr.hasOwnProperty(key)) { // 这样就可以过滤掉原型链上的可枚举属性了
            console.log(key, arr[key]);
        }
    }
    ```

    for in **可以**正确响应break、continue,**不可以**响应return语句。

- ### for of  

    根据值遍历，用来补充for in在遍历时不能根据值遍历的不足，由于是ES6,兼容性不是很好。  

    for of适用遍历数/数组对象/字符串/map/set等拥有迭代器对象的集合，但是不能遍历对象,因为没有迭代器对象，也就是说for of可以遍历拥有Symbol.iterator的对象。

    forEach方法与map方法很相似，也是对数组的所有成员依次执行参数函数。但是，forEach方法不返回值，只用来操作数据。也就是说，如果数组遍历的目的是为了得到返回值，那么使用map方法，否则使用forEach方法。

    > 语法：for (variable of iterable) { //statements }  
     > variable 在每次迭代中，将不同属性的值分配给变量  
     > iterable 被迭代枚举其属性的对象
     >  
     > 返回值：undefined

    for of循环不支持普通对象，但如果你想迭代一个对象的属性，你可以用for in循环（这也是它的本职工作）或内建的Object.keys()方法:  

    ```js
    for (var key of Object.keys(someObject)) {
        console.log(key + ": " + someObject[key]);
    }
    ```

    for of **可以**正确响应break、continue,**不可以**响应return语句。

- ### forEach  

    forEach方法与map方法很相似，也是对数组的所有成员依次执行参数函数。但是，forEach方法不返回值，只用来操作数据。也就是说，如果数组遍历的目的是为了得到返回值，那么使用map方法，否则使用forEach方法。

    > 语法：array.map(function(currentValue,index,arr), thisValue)  
     > currentValue 必须,当前元素的值  
     > index 可选,当前元素的索引值  
     > arr 可选,当前元素属于的数组对象  
     > thisValue 可选。传递给函数的值一般用 "this" 值。如果这个参数为空， "undefined" 会传递给 "this" 值
     >  
     > 返回值：undefined

    ```js
    var arrs = [2, 5, 9];
    arrs.forEach(function(element, index, array){
        console.log('[' + index + '] = ' + element);
    });
    // [0] = 2 // [1] = 5 // [2] = 9
    ```

    此外，forEach循环和map循环一样也可以用绑定回调函数内部的this变量，间接操作其它变量（参考下面的map()循环例子）。

    forEach **不可以**正确响应break、continue，return会跳出当前循环，但是不会终止循环的继续。

- ### map

    map方法将数组的所有成员依次传入参数函数，然后把每一次的执行结果组成一个新数组返回。

    > 语法：array.map(function(currentValue,index,arr), thisValue)  
     > currentValue 必须,当前元素的值  
     > index 可选,当前元素的索引值  
     > arr 可选,当前元素属于的数组对象  
     > thisValue 可选。对象作为该执行回调时使用，传递给函数，用作 "this" 的值。如果省略了 thisValue ，"this" 的值为 "undefined"
     >  
     > 返回值：返回一个新数组，数组中的元素为原始数组元素调用函数处理后的值。

    ```js
    var numbers = [1, 2, 3];
    var newNumbers = numbers.map(function (n) {
        return n + 1;
    });
    //newNumbers [2, 3, 4]
    //numbers [1, 2, 3]
    ```

    map方法接受一个函数作为参数。该函数调用时，map方法向它传入三个参数：当前成员、当前位置和数组本身。

    ```js
    [1, 2, 3].map(function(elem, index, arr) {
        return elem * index;
    });
    // [0, 2, 6]
    ```

    此外，map()循环还可以接受第二个参数，用来绑定回调函数内部的this变量，将回调函数内部的this对象，指向第二个参数，间接操作这个参数（一般是数组）。

    ```js
    var arr = ['a', 'b', 'c'];
    [1, 2].map(function (e) {
        return this[e];
    }, arr)
    // ['b', 'c']
    ```

    上面代码通过map方法的第二个参数，将回调函数内部的this对象，指向arr数组。间接操作了数组arr; forEach同样具有这个功能。

    map **不可以**正确响应break、continue，return会跳出当前循环，当前位置会用undefined替换，不会改变数组的长度，但是不会终止循环的继续。

- ### filter

    filter()也是一个常用的操作，它用于把Array的某些元素过滤掉，然后返回剩下的元素,具有返回值。

    > 语法：array.filter(function(currentValue,index,arr), thisValue)  
     > currentValue 必须,当前元素的值  
     > index 可选,当前元素的索引值  
     > arr 可选,当前元素属于的数组对象  
     > thisValue 可选。对象作为该执行回调时使用，传递给函数，用作 "this" 的值。如果省略了 thisValue ，"this" 的值为 "undefined"
     >  
     > 返回值：返回数组，包含了符合条件的所有元素。如果没有符合条件的元素则返回空数组。  

    和map()类似，Array的filter()也接收一个函数。和map()不同的是，filter()把传入的函数依次作用于每个元素，然后根据返回值是true还是false决定保留还是丢弃该元素。

    ```js
    //返回数组 ages 中所有元素都大于 18 的元素
    let ages = [3, 10, 18, 20];
    let newAges = ages.filter((age) => {
        return age >= 18;
    });
    console.log(newAges);//[18, 20]
    ```

    filter()接收的回调函数，其实可以有多个参数。通常仅使用第一个参数，表示Array的某个元素。回调函数还可以接收另外两个参数，表示元素的位置和数组本身：

    ```js
    var arr = ['A','B','C','d'];
    var arr_filter = arr.filter(function(element,index,self){
        console.log(element);/* 依次打印'A','B','C','d' */
        console.log(index);/* 依次打印0,1,2,3 */
        console.log(self);/* 打印数组本身即arr */
        return true;
    })
    ```

    filter **不可以**正确响应break、continue，return会跳出当前循环，并且会去除当前位置，改变数组的长度，但是不会终止循环的继续.

- ### some

    some()是对数组中每一项运行给定函数，如果该函数对任一项返回true，则返回true。some一直在找符合条件的值，一旦找到，则不会继续迭代下去。

     > 语法：array.some(function(currentValue,index,arr),thisValue)  
     > currentValue 必须,当前元素的值  
     > index 可选,当前元素的索引值  
     > arr 可选,当前元素属于的数组对象  
     > thisValue 可选。对象作为该执行回调时使用，传递给函数，用作 "this" 的值。如果省略了 thisValue ，"this" 的值为 "undefined"
     >  
     > 返回值：true/false

    ```js
    //检测数组中是否有元素大于 18
    let ages = [3, 10, 18, 20];
    let is = ages.some((age) => {
        return age >= 18;
    });
    console.log(is);//true
    ```

- ### every

    every()是对数组中每一项运行给定函数，如果该函数每一项返回true,则返回true。every从迭代开始，一旦有一个不符合条件，则不会继续迭代下去。

    > 语法：array.every(function(currentValue,index,arr),thisValue)  
     > currentValue 必须,当前元素的值  
     > index 可选,当前元素的索引值  
     > arr 可选,当前元素属于的数组对象  
     > thisValue 可选。对象作为该执行回调时使用，传递给函数，用作 "this" 的值。如果省略了 thisValue ，"this" 的值为 "undefined"
     >  
     > 返回值：true/false

    ```js
    //检测数组 ages 的所有元素是否都大于等于 18
    let ages = [3, 10, 18, 20];
    let is = ages.every((age) => {
        return age >= 18;
    });
    console.log(is);//false
    ```

另外，reduce()、reduceRight()也具有循环遍历的作用。