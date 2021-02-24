# JS代码片段



## 字符串转换数组

> 引用地址：[How to get character array from a string?](https://stackoverflow.com/a/34717402)

正常情况下，我们可能会想到直接使用`str.split('')`的方式将字符串转换成数组，但是当存在下面这种情况的时候，输出将会异常

```js
const a = '𝟘𝟙𝟚𝟛'.split('');
console.log(a);

// [
//   "�",
//   "�",
//   "�",
//   "�",
//   "�",
//   "�",
//   "�",
//   "�"
// ]
```

可以使用 ES6 的语法解决这个问题：

```js
const a = [...'𝟘𝟙𝟚𝟛'];
console.log(a);
//或者
const a = Array.from('𝟘𝟙𝟚𝟛');
console.log(a);
```

如果你的环境不支持 ES6，则可以使用正则表达式的方式：

```js
const a = '𝟘𝟙𝟚𝟛'.split(/(?=(?:[\0-\uD7FF\uE000-\uFFFF]|[\uD800-\uDBFF][\uDC00-\uDFFF]|[\uD800-\uDBFF](?![\uDC00-\uDFFF])|(?:[^\uD800-\uDBFF]|^)[\uDC00-\uDFFF]))/);
console.log(a);
```



## Fisher–Yates shuffle洗牌算法

> 引用地址：[Fisher–Yates shuffle 洗牌算法](https://gaohaoyang.github.io/2016/10/16/shuffle-algorithm/)

它给出了 1 到 N 的数字的的随机排列，具体步骤如下：

1. 写下从 1 到 N 的数字
2. 取一个从 1 到剩下的数字（包括这个数字）的随机数 k
3. 从低位开始，得到第 k 个数字（这个数字还没有被取出），把它写在独立的一个列表的最后一位
4. 重复第 2 步，直到所有的数字都被取出
5. 第 3 步写出的这个序列，现在就是原始数字的随机排列

已经证明如果第 2 步取出的数字是真随机的，那么最后得到的排序一定也是。



```js
/**
 * Fisher–Yates shuffle
 */
Array.prototype.shuffle = function() {
    var input = this;

    for (var i = input.length-1; i >=0; i--) {

        var randomIndex = Math.floor(Math.random()*(i+1));
        var itemAtIndex = input[randomIndex];

        input[randomIndex] = input[i];
        input[i] = itemAtIndex;
    }
    return input;
}
```

