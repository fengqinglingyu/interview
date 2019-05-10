## 遇到的问题

### JavaScript 篇

- `let` `var` `const` 相关问题  
  `var`: 只有函数作用域和全局作用域，没有块级作用域，变量提升，在全局定义的话，可以通过 `window.定义的变量名` 找到，可以重复声明变量。  
  `let`: 有块级作用域，没有变量提升(这里网上很多不同意见)，但是存在暂时性死区，声明一个变量之后不能重复声明。  
  `const`: 和 `let` 一样，但是 `const` 声明的变量是常量，不可以再赋值，并且 `const` 声明变量的时候，必须初始化。

  **遇到的题目**

  > 请写出以下 name 的值，并且说明为什么

  ```js
  var name = 'a';
  (function() {
    console.log(name);
  })();
  // 输出 a， 因为在函数内并没有声明 name 属性，所以沿着作用域链去查找 name， 在全局环境找到，输出

  var name = 'a';
  (function(name) {
    console.log(name);
    name = 'd';
  })();
  // 输出 undefined ，因为函数有形参 name 相当于在函数体内定义了 var name; 运行函数的时候并没有传参，并且在没有赋值之前就输出，所以输出 undefined

  var name = 'a';
  (function() {
    console.log(name);
    var name = 'd';
  })();
  // 输出 undefined， 因为 var name 提前，实际上和下面的题目一样

  var name = 'a';
  (function() {
    var name;
    console.log(name);
    name = 'd';
  })();
  // 输出 undefined，没有赋值之前就读取值了

  var name = 'a';
  (function() {
    console.log(name);
    let name = 'd';
  })();
  // ReferenceError: name is not defined，因为 let name 声明之前，name 是一个暂时性死区，无法读取它的值

  var name = 'a';
  function name() {
    return 1;
  }
  (function() {
    console.log(name);
  })();
  // 函数声明解析先于变量声明，所以 name 首先是一个函数，然后被重新定义成一个 字符串，输出 a
  ```

- `esnext` 常用的新特性

  `let` `const` 箭头函数 `class` `Promise` `async/await` 展开运算符 `for...of` 循环 `rest` 参数 等。

- `cookie` `sessionStorage` `localStorage` 的区别

  这三个都是本地存储，可以把信息记录在客户端。  
  `cookie` 永久储存，可以设置过期时间，存储大小一般在 `4K` 左右，每次请求会附带在 `header` 上，会影响性能。  
  `sessionStorage` 页面关闭就清理，不能设置过期时间，不会附带在 `header`上，存储大小 `5M` 左右。  
  `localStorage` 永久存储，其他和 `sessionStorage` 一样。
  建议： 最好不要用 `cookie` 作为本地存储了，有可以完美代替的方案。

- `es5`的 `function` 和 `es6` 的箭头函数有什么区别

  `function` 内部可以使用 `arguments` 对象， `this` 指向函数运行时的对象，可以作为构造器使用，可以被 `new` ，可以使用 `prototype` 属性， 可以使用 `yield` 关键字。

  箭头函数内部没有 `arguments` 对象，如果想获取参数列表请使用 `rest` 参数，没有 `this` ，它只会从自己的作用域链的上一层继承 `this`，不能作为构造器使用，所以也不能被 `new`，不能使用 `prototype` 属性， 不能使用 `yield` 关键字。

  实际上一般答 `this` 和 `arguments` 这两个不同就好了。

- `this` 的定义

  在函数中，可以引用运行环境中的变量。因此就需要一个机制来让我们可以在函数体内部获取当前的运行环境，这便是 `this` 。
  因此要明白 `this` 指向，其实就是要搞清楚 函数的运行环境，说人话就是，谁调用了函数。例如:

  `obj.fn()` ，便是 `obj` 调用了函数，既函数中的 `this === obj`
  `fn()` ，这里可以看成 `window.fn()` ，因此 `this === window`

  但这种机制并不完全能满足我们的业务需求，因此提供了三种方式可以手动修改 `this` 的指向:

  `call: fn.call(target, 1, 2)`
  `apply: fn.apply(target, [1, 2])`
  `bind: fn.bind(target)(1,2)`

  箭头函数没有 `this`， 它只会从自己的作用域链的上一层继承 `this`

  **遇到的题目**

  ```js
  function a() {
    return () => {
      return () => {
        console.log(this);
      };
    };
  }
  console.log(a()()());
  // 输出 window 对象，因为箭头函数没有 this，所以一直延作用域链找到 a 函数定义的地方，this 和 a 函数内部的 this 是一样的，a在全局环境调用，所以是 window
  ```

- `.call` `.apply` `.bind` 三者之间的区别

  简单点来说，这三个都可以改变函数运行的作用域，第一个参数都是函数运行需要绑定的 `this` ， `.call` 后面的参数列表是函数运行时的参数列表， `.apply` 后面的参数是一个数组或者类数组，执行函数，`.bind` 和 `.call`一样，后面的参数列表是初始参数，执行后返回一个原函数的拷贝。

  `.call` 根据输入的参数运行函数

  > fun.call(thisArg, arg1, arg2, ...)

  `thisArg`
  在 fun 函数运行时指定的 `this` 值， 为空的时候指向 `window`

  `arg1, arg2, ...`
  指定的参数列表。

  `.apply` 根据输入的参数运行函数

  > fun.apply(thisArg, [argsArray])

  `thisArg`
  在 fun 函数运行时指定的 `this` 值， 为空的时候指向 `window`

  `argsArray ...`
  数组或者类数组，函数运行时的参数列表。

  `.bind` 返回一个原函数的拷贝，并拥有指定的 this 值和初始参数。

  > function.bind(thisArg[, arg1[, arg2[, ...]]])

  `thisArg`
  调用绑定函数时作为 this 参数传递给目标函数的值。 如果使用 new 运算符构造绑定函数，则忽略该值。

  `arg1, arg2, ...`
  当目标函数被调用时，预先添加到绑定函数的参数列表中的参数。

* `async/await` 的错误处理机制

  `try...catch`

* 在有 `Promise` 和 `Generator`的条件下，为什么还加了 `async/await`

  1.更好的语义，`async` 和 `await` ，比起 `*` 和 `yield`，语义更清楚了。  
  2.`async` 函数内置执行器，函数调用之后，会自动执行。  
  3.`async` 函数的返回值是 `Promise` 对象。  
  4.更广的适用性。`co` 模块约定，`yield` 命令后面只能是 `Thunk` 函数或 `Promise` 对象，而 `async` 函数的 `await` 命令后面，可以是 `Promise` 对象和原始类型的值。

* 浏览器中 `JavaScript` 的 `eventloop`

  笔者的语言能力非常弱，我觉得我表述的不是很清楚。可以参考这里面的说法，相关题目也可以找到：  
  [常见异步笔试题](https://github.com/Advanced-Frontend/Daily-Interview-Question/issues/7)  
  发现这个被提问的还是比较多的，面试了不少公司都有类似的题目。

* `Common.js` 和 `ES6` 的模块处理的区别

  `require` 支持 动态导入，`import` 不支持，正在提案 (`babel` 可以通过插件获得支持)  
  `require` 是 同步 导入，`import` 属于 异步 导入  
  `require` 是 值拷贝，导出值变化不会影响导入值；`import` 指向 内存地址，导入值会随导出值而变化

* `JS` 常用的跨越有哪些

  `img` 标签，`JSONP`， `CORS`， `PostMessage`

* `Debounce` 手写

  防抖和节流问到的都挺多的

  ```js
  // 既然是手写就写最简单的就好
  function debounce(fn, delay) {
    let timer;
    return (...args) => {
      clearTimeout(timer);
      timer = setTimeout(() => {
        fn(...args);
      }, delay);
    };
  }
  // 或者面试官说，如果要 ES5 环境下的
  function debounce(fn, delay) {
    var timer;
    return function() {
      var args = arguments;
      var _this = this;
      clearTimeout(timer);
      timer = setTimeout(function() {
        fn.apply(_this, args);
      }, delay);
    };
  }
  ```

- `try...catch` 和抛错相关的问题

  请写出这段代码的问题，请至少写出两个。

  ```js
  function fn() {
    try {
      setTimeout(() => {
        throw 'error';
      }, 0);
    } catch (e) {
      console.error(e);
    }
  }
  fn();
  ```

  我笔试的时候写出了两个，真写不出第三个是啥  
  1.直接抛字符型的错误，不用 `Error` 对象抛出，错误被截获的时候，没有任何堆栈信息。  
  2.这里的抛错，没有被 `catch`，其实很简单的能懂的一个道理，`setTimeout`里面函数执行的时候，`fn` 早执行完了。

* `斐波那契数列` 手写

  很简单的递归题目，在红宝书里面有解

  ```js
  function fn(n) {
    if (n < 3) {
      return 1;
    }
    return fn(n - 1) + fn(n - 2);
    // return arguments.callee(n - 1) + arguments.callee(n - 2);
  }

  // 面试官：如果不用递归呢？
  function fn(n) {
    let a = 0;
    let b = 1;
    for (let i = 0; i < n; i++) {
      [a, b] = [b, a + b];
    }
    return a;
  }
  ```

* `Throttle` 手写

  防抖和节流问到的都挺多的，但是很少遇到两个一起出的，最多另外一个问原理，一个实现。

  ```js
  // 既然是手写就写最简单的就好
  function throttle(fn, delay) {
    let timer;
    return (...args) => {
      if (!timer) {
        timer = setTimeout(() => {
          fn(...args);
          timer = null;
        }, delay);
      }
    };
  }
  // 或者面试官说，如果要 ES5 环境下的
  function throttle(fn, delay) {
    var timer;
    return function() {
      var args = arguments;
      var _this = this;
      if (!timer) {
        timer = setTimeout(function() {
          fn.apply(_this, args);
          timer = null;
        }, delay);
      }
    };
  }
  ```

* `JS` 类型转换问题

  - 对象转基本类型

    对象转基本类型的问题，首先会调用对象的 `[Symbol.toPrimitive]`，然后到 `valueOf`，然后是 `toString`。这三个方法都可以重写。

    ```js
    const a = {
      valueOf() {
        return 0;
      },
      toString() {
        return '1';
      },
      [Symbol.toPrimitive]() {
        return 2;
      }
    };
    1 + a; // => 3
    '1' + a; // => '12'
    ```

  - 四则运算符

    除了加法以外，其他运算都会把参与运算的值转换成数字，转换不了就 `NaN`，如果是加法的话，运算中其中一方是字符串，也会把运算另外一方转成字符串。 `+` `-` 运算符，也可以把值转换成数字，并且运算符等级比四则运算高。

    ```js
    1 + '1'; // '11'
    2 * '2'; // 4
    [1, 2] + [2, 1]; // '1,22,1'
    'a' + +'b'; // -> 'aNaN' for + 'b' = NaN -> 'a' + NaN = 'aNaN'
    1 + -[2, 1]; // -> NaN for -[2, 1] = NaN -> 1 + NaN = NaN
    ```

  - `==` 运算符

    1.当比较数字和字符串时，字符串会转换成数字值。 `JavaScript` 尝试将数字字面量转换为数字类型的值。 首先, 一个数学上的值会从数字字面量中衍生出来，然后得到被四舍五入后的数字类型的值。  
     2.如果其中一个操作数为布尔类型，那么布尔操作数如果为`true`，那么会转换为`1`，如果为`false`，会转换为整数`0`，即`0`。  
     3.如果一个对象与数字或字符串相比较，`JavaScript`会尝试返回对象的默认值。操作符会尝试通过方法`valueOf`和`toString`将对象转换为其原始值（一个字符串或数字类型的值）。如果尝试转换失败，会产生一个运行时错误。 4.注意：当且仅当与原始值比较时，对象会被转换为原始值。当两个操作数均为对象时，它们作为对象进行比较，仅当它们引用相同对象时返回`true`。

    ```js
    [0] == false
    [1] == [1]
    [] == ![]

    // 试着求结果并且解释为什么，真实面试题。
    ```

- 数组去重手写

  ```js
  // 最简单
  function fn(arr) {
    return [...new Set(...arr)];
  }
  // 往往面试官要的不是这个答案
  ```

- 数组铺平实现

  ```js
  // 实际写代码
  function fn(arr) {
    return arr.flat(Infinity);
  }
  // 但是面试官说，这么写就没意思了，于是可以换一个递归实现的
  function fn(arr) {
    const res = [];
    arr.forEach(item => {
      if (Array.isArray(item)) {
        res.push(...fn(item));
      } else {
        res.push(item);
      }
    });
    return res;
  }
  // 面试官说，如果不用递归呢，那就迭代吧
  function fn(arr) {
    const res = [];
    // 由于会破坏原数组，最好浅复制一下，保护原数组
    const tmp = [...arr];
    while (tmp.length) {
      const item = tmp.shift();
      if (Array.isArray(item)) {
        tmp.unshift(...item);
      } else {
        res.push(item);
      }
    }
    return res;
  }
  // 方法很多，但是千万别写这种，面试官说如果我里面什么数据类型都有，下面的写法只会返回字符串
  function fn(arr) {
    return arr.toString().split(',');
  }
  ```

- 函数签名的问题，怎么处理

  ```ts
  // 面试官问了这个问题，千万别答 [1, 2, 3]
  [1, 2, 3].map(parseInt); // [1, NaN, NaN]

  // 看看 parseInt 的参数声明，人家是有两个参数的。
  parseInt: (s: string, radix?: number): number
  ```

  `parseInt` 接受了 `map` 的值作为第一个参数，索引作为第二个参数，然后数组本身这个参数没用到，被忽略了，所以计算过程是

  ```js
  parseInt(1, 0); // 1
  parseInt(2, 1); // 基数没有 1 的，返回 NaN
  parseInt(3, 2); // 二进制不可能出现 3 的，只可能有 0 和 1 ，所以还是 NaN
  ```

  想输出预期结果很简单 `[1, 2, 3].map(i => parseInt(i))` 或者 `[1, 2, 3].map(Number)`

  面试官又说，如果我以上两种都不想写，并且要在 `parseInt` 上面修改，这时候，面试官的意图就出来了，面试官想要的答案是函数柯里化。写一个返回基数是 `10` 的方法就好。

  ```ts
  function radix10ParseInt(s: string): number {
    return parseInt(s, 10);
  }
  [1, 2, 3].map(radix10ParseInt);
  ```

* 实现一个函数柯里化

  因为你答出了函数柯里化，面试官就会问你怎么实现类似的效果。如果面试官出的题目，参数不多的情况下，可以不写通用的，毕竟功能实现了，但是通用的更好。

  ```js
  function curry(fn, ...args) {
    const len = fn.length;
    return (...innerArgs) => {
      const mergedArgs = [...args, ...innerArgs];
      if (len !== mergedArgs.length) {
        return curry(fn, ...mergedArgs);
      }
      return fn(...mergedArgs);
    };
  }

  const addOrigin = (a, b, c, d, e) => a + b + c + d + e;
  const add = curry(addOrigin);
  const add2 = curry(addOrigin, 1, 2);

  add(1)(2, 3)(4, 5);
  add(1)(2)(3)(4)(5);
  add2(3, 4)(5);
  ```

* `Vue` 双向绑定的原理，有什么缺陷，`Vue 3.0` 中做了什么处理，有什么优点

  通过 `Object.defineProperty` 深度遍历所有每个需要双向绑定的对象的属性，重写 `get`, `set` 方法，在获取属性被获取的时候添加订阅者，在属性修改的时候通知所有订阅者更新。  
   写一个简单的实现代码，别说，我还真遇到手写简单的，写起来不算难。

  ```js
  function observe(obj) {
    for (let prop in obj) {
      define(obj, prop, obj[prop]);
    }
  }

  function define(obj, prop, value) {
    if (typeof obj[prop] === 'object') {
      observe(obj[prop]);
    }
    const dep = new Dep();
    Object.defineProperty(obj, prop, {
      enumerable: true,
      configurable: true,
      get() {
        // 当有获取该属性时，证明依赖于该对象，因此被添加进收集器中
        if (Dep.target) {
          dep.addSub(Dep.target);
        }
        return value;
      },
      // 重新设置值时，触发收集器的通知机制
      set(newVal) {
        value = newVal;
        dep.notify();
      }
    });
  }

  // 依赖收集器
  class Dep {
    constructor() {
      this.subs = [];
    }
    addSub(sub) {
      this.subs.push(sub);
    }
    notify() {
      this.subs.forEach(sub => {
        sub.update();
      });
    }
  }

  Dep.target = null;

  class Watcher {
    constructor(obj, key, cb) {
      Dep.target = this;
      this.cb = cb;
      this.key = key;
      this.obj = obj;
      // 触发 get 方法，把这个观察者添加到依赖列表
      this.value = obj[key];
      Dep.target = null;
    }
    update() {
      this.value = this.obj[this.key];
      this.cb(this.value);
    }
  }

  // key
  const data = { key: 1 };
  observe(data);
  new Watcher(data, 'key', val => {
    console.log(val);
  });

  data.key = '2';

  // 复制到 浏览器控制台，或者直接用node可以测试
  ```

  `Object.defineProperty` 的缺陷，其实， `Vue`教程列表渲染那章其实说的很清楚了。  
   1.`Object.defineProperty` 无法监控到数组下标的变化，导致通过数组下标添加元素，不能实时响应；

  > 补充 Vue 对 push,pop,splice 等八种方法进行了 hack 处理，这些方法引起的变化是可以检测的，所以其他数组的属性也是检测不到的。

  2.`Object.defineProperty` 只能劫持对象的属性，从而需要对每个对象，每个属性进行遍历，如果，属性值是对象，还需要深度遍历。

  `Vue` 3.0 用 `Proxy` 替代了 `Object.defineProperty`

  `Proxy` 优势：  
   1.`Proxy` 可以劫持整个对象，并返回一个新的对象。并且可以代理动态增加的属性。  
   2.能监听数组的变化。  
   3.有 13 种挟持操作。  
   劣势：  
   `Proxy` 是 `ES6` 新增的属性，并且无法 `polyfill`。

- `List diff` 实例问题

  比如原来的 `List` `[a, b, c, d]` 怎么变成 `[a, c, e, d, f]` 这类问题，这里就不提供答案了。

- 虚拟 `DOM` 的好处

  一个很简单的总结： 用 `js` 对象模拟 `DOM` 结构，通过 `diff` 在 `JS` 对象处理数据更新，避免频繁操作 `DOM`，提高性能。  
  [参考链接](https://github.com/Advanced-Frontend/Daily-Interview-Question/issues/47)

### 其他

- 浏览器的渲染原理

  渲染过程也是问的比较多的。

  1. `HTML` 解析器解析 `HTML` 代码， 生成 `DOM` 树
  2. `CSS` 解析器解析 `CSS` 代码，生成 `CCSOM` 树
  3. `DOM` 树和 `CSSOM` 树合并，生成渲染树
  4. 布局 + 绘制

  总结： 构建`DOM` -> 构建`CSSOM` -> 构建渲染树 -> 布局 -> 绘制

  构建 `DOM` 和构建 `CSSOM` 是并行的。  
  `JavaScript` 的加载、解析与执行会阻塞 DOM 的构建。 并且由于 `JavaScript` 可以操作 `CSSOM`，不完整的 CSSOM 是无法使用的 ，这时候就先等 `CSSOM` 构建完成才会执行 `JS` 文件，因此在遇到有 `JS` 加载的时候 `CSSOM` 也会阻塞 `DOM` 构建。

  面向面试的话，答出这些就好了。详细请点击这里：
  [参考链接](https://blog.fundebug.com/2019/01/03/understand-browser-rendering/)

* `DOMContentLoaded` 和 `Load` 事件的差别

  可能面试官问你的问题不是这个，可能是 `window.load` 和 `$(document).ready` 的差别，其实问题差不多，当然，这个需要答的更多，核心还是一样的。
  `DOMContentLoaded` 在 `DOM` 构建完成就触发。  
  `load` 在页面所有资源加载完成后才触发，包括静态资源。

* `Http` 缓存策略

  强缓存(`Expires` 和 `Cache-Control`)和协商缓存(`Last-Modified` 和 `Etag`)

  强缓存表示在缓存期间不需要请求，`state code` 为 `200`

  `Expires`

        // 表示文件 `Fri, 09 May 2019 09:00:00 GMT` 过期
        Expires: Fri, 09 May 2019 09:00:00 GMT

  `Cache-control`

        // 表示文件 31536000 秒后过期，也就是一年
        Cache-control: max-age=31536000

  如果缓存过期了，我们就可以使用协商缓存来解决问题。协商缓存需要请求，如果缓存有效会返回 `304`。

  `Last-Modified`(res header) 和 `If-Modified-Since`(req header)

  Last-Modified 表示本地文件最后修改日期，`If-Modified-Since` 会将 `Last-Modified` 的值发送给服务器，询问服务器在该日期后资源是否有更新，有更新的话就会将新的资源发送回来。缺点是，精度只能到秒，秒内的修改没法检测到，如果文件修改，内容没变，缓存还是会刷新。所以就有了 `Etag`。

  `ETag`(res header) 和 `If-None-Match`(req header)

  `ETag` 类似于文件指纹，`If-None-Match` 会将当前 `ETag` 发送给服务器，询问该资源 `ETag` 是否变动，有变动的话就将新的资源发送回来。并且 `ETag` 优先级比 `Last-Modified` 高。

- 观察者和发布订阅模式的差别

  这个在电话面试里面被问到，可以参考 [这个链接](https://github.com/Advanced-Frontend/Daily-Interview-Question/issues/25)。  
  可以想一想，`Vue` 双向绑定用的是观察者，还是发布订阅。`Vuex`呢。

- `Cors` 如何实现同一个主域下全部子域都可以跨域

  这个被问到，这里就不提供答案了，

- 如何实现轮询

  不停的发请求和 `WebSocket` 等。

### 写在最后

今年我遇到的面试题大概就这些，有一些零零碎碎的不写了，一些比较热门的比如对象，继承这些没遇到过，反而一些挺冷门的问题遇到了，面试，基础还是很重要，现场编程能力也很重要的，面试过程中和面试官聊天还是很不错的，沟通还是很重要，必须锻炼自己的沟通能力。
