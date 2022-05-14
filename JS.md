#### **提取页面代码中所有网址**

```js
var aa = document.documentElement.outerHTML
    .match(
        /(url\(|src=|href=)[\"\']*([^\"\'\(\)\<\>\[\] ]+)[\"\'\)]*|(http:\/\/[\w\-\.]+[^\"\'\(\)\<\>\[\] ]+)/gi
    )
    .join("\r\n")
    .replace(/^(src=|href=|url\()[\"\']*|[\"\'\>\) ]*$/gim, "");
alert(aa);
```

#### **实现base64解码**

```js
function base64_decode(data) {
    var b64 = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/=";
    var o1,
        o2,
        o3,
        h1,
        h2,
        h3,
        h4,
        bits,
        i = 0,
        ac = 0,
        dec = "",
        tmp_arr = [];
    if (!data) {
        return data;
    }
    data += "";
    do {
        h1 = b64.indexOf(data.charAt(i++));
        h2 = b64.indexOf(data.charAt(i++));
        h3 = b64.indexOf(data.charAt(i++));
        h4 = b64.indexOf(data.charAt(i++));
        bits = (h1 << 18) | (h2 << 12) | (h3 << 6) | h4;
        o1 = (bits >> 16) & 0xff;
        o2 = (bits >> 8) & 0xff;
        o3 = bits & 0xff;
        if (h3 == 64) {
            tmp_arr[ac++] = String.fromCharCode(o1);
        } else if (h4 == 64) {
            tmp_arr[ac++] = String.fromCharCode(o1, o2);
        } else {
            tmp_arr[ac++] = String.fromCharCode(o1, o2, o3);
        }
    } while (i < data.length);
    dec = tmp_arr.join("");
    dec = utf8_decode(dec);
    return dec;
}
```

#### **时间日期格式转换**

```js
Date.prototype.format = function(formatStr) {
    var str = formatStr;
    var Week = ["日", "一", "二", "三", "四", "五", "六"];
    str = str.replace(/yyyy|YYYY/, this.getFullYear());
    str = str.replace(
        /yy|YY/,
        this.getYear() % 100 > 9 ?
        (this.getYear() % 100).toString() :
        "0" + (this.getYear() % 100)
    );
    str = str.replace(
        /MM/,
        this.getMonth() + 1 > 9 ?
        (this.getMonth() + 1).toString() :
        "0" + (this.getMonth() + 1)
    );
    str = str.replace(/M/g, this.getMonth() + 1);
    str = str.replace(/w|W/g, Week[this.getDay()]);
    str = str.replace(
        /dd|DD/,
        this.getDate() > 9 ? this.getDate().toString() : "0" + this.getDate()
    );
    str = str.replace(/d|D/g, this.getDate());
    str = str.replace(
        /hh|HH/,
        this.getHours() > 9 ? this.getHours().toString() : "0" + this.getHours()
    );
    str = str.replace(/h|H/g, this.getHours());
    str = str.replace(
        /mm/,
        this.getMinutes() > 9 ?
        this.getMinutes().toString() :
        "0" + this.getMinutes()
    );
    str = str.replace(/m/g, this.getMinutes());
    str = str.replace(
        /ss|SS/,
        this.getSeconds() > 9 ?
        this.getSeconds().toString() :
        "0" + this.getSeconds()
    );
    str = str.replace(/s|S/g, this.getSeconds());
    return str;
};
// 或
Date.prototype.format = function(format) {
    var o = {
        "M+": this.getMonth() + 1, //month
        "d+": this.getDate(), //day
        "h+": this.getHours(), //hour
        "m+": this.getMinutes(), //minute
        "s+": this.getSeconds(), //second
        "q+": Math.floor((this.getMonth() + 3) / 3), //quarter
        S: this.getMilliseconds() //millisecond
    };
    if (/(y+)/.test(format))
        format = format.replace(
            RegExp.$1,
            (this.getFullYear() + "").substr(4 - RegExp.$1.length)
        );
    for (var k in o) {
        if (new RegExp("(" + k + ")").test(format))
            format = format.replace(
                RegExp.$1,
                RegExp.$1.length == 1 ? o[k] : ("00" + o[k]).substr(("" + o[k]).length)
            );
    }
    return format;
};
alert(new Date().format("yyyy-MM-dd hh:mm:ss"));
```

#### **获得URL中GET参数值**

```js
// 用法：如果地址是 test.htm?t1=1&t2=2&t3=3, 那么能取得：GET["t1"], GET["t2"], GET["t3"]
function getGet() {
    querystr = window.location.href.split("?");
    if (querystr[1]) {
        GETs = querystr[1].split("&");
        GET = [];
        for (i = 0; i < GETs.length; i++) {
            tmp_arr = GETs.split("=");
            key = tmp_arr[0];
            GET[key] = tmp_arr[1];
        }
    }
    return querystr[1];
}
```

#### **时间个性化输出功能**

```js
/*
1、< 60s, 显示为“刚刚”
2、>= 1min && < 60 min, 显示与当前时间差“XX分钟前”
3、>= 60min && < 1day, 显示与当前时间差“今天 XX:XX”
4、>= 1day && < 1year, 显示日期“XX月XX日 XX:XX”
5、>= 1year, 显示具体日期“XXXX年XX月XX日 XX:XX”
*/
function timeFormat(time) {
    var date = new Date(time),
        curDate = new Date(),
        year = date.getFullYear(),
        month = date.getMonth() + 10,
        day = date.getDate(),
        hour = date.getHours(),
        minute = date.getMinutes(),
        curYear = curDate.getFullYear(),
        curHour = curDate.getHours(),
        timeStr;
    if (year < curYear) {
        timeStr = year + "年" + month + "月" + day + "日 " + hour + ":" + minute;
    } else {
        var pastTime = curDate - date,
            pastH = pastTime / 3600000;
        if (pastH > curHour) {
            timeStr = month + "月" + day + "日 " + hour + ":" + minute;
        } else if (pastH >= 1) {
            timeStr = "今天 " + hour + ":" + minute + "分";
        } else {
            var pastM = curDate.getMinutes() - minute;
            if (pastM > 1) {
                timeStr = pastM + "分钟前";
            } else {
                timeStr = "刚刚";
            }
        }
    }
    return timeStr;
}
```

#### **金额大写转换函数**

```js
const transform = (tranvalue) => {
    try {
        var i = 1;
        var dw2 = ["", "万", "亿"]; //大单位
        var dw1 = ["拾", "佰", "仟"]; //小单位
        var dw = ["零","壹","贰","叁","肆","伍","陆","柒","捌","玖"];
        //整数部分用
        //以下是小写转换成大写显示在合计大写的文本框中
        //分离整数与小数
        var source = splits(tranvalue);
        var num = source[0];
        var dig = source[1];
        //转换整数部分
        var k1 = 0; //计小单位
        var k2 = 0; //计大单位
        var sum = 0;
        var str = "";
        var len = source[0].length; //整数的长度
        for (i = 1; i <= len; i++) {
            var n = source[0].charAt(len - i); //取得某个位数上的数字
            var bn = 0;
            if (len - i - 1 >= 0) {
                bn = source[0].charAt(len - i - 1); //取得某个位数前一位上的数字
            }
            sum = sum + Number(n);
            if (sum != 0) {
                str = dw[Number(n)].concat(str); //取得该数字对应的大写数字，并插入到str字符串的前面
                if (n == "0") sum = 0;
            }
            if (len - i - 1 >= 0) {
                //在数字范围内
                if (k1 != 3) {
                    //加小单位
                    if (bn != 0) {
                        str = dw1[k1].concat(str);
                    }
                    k1++;
                } else {
                    //不加小单位，加大单位
                    k1 = 0;
                    var temp = str.charAt(0);
                    if (temp == "万" || temp == "亿")
                        //若大单位前没有数字则舍去大单位
                        str = str.substr(1, str.length - 1);
                    str = dw2[k2].concat(str);
                    sum = 0;
                }
            }
            if (k1 == 3) {
                //小单位到千则大单位进一
                k2++;
            }
        }
        //转换小数部分
        var strdig = "";
        if (dig != "") {
            var n = dig.charAt(0);
            if (n != 0) {
                strdig += dw[Number(n)] + "角"; //加数字
            }
            var n = dig.charAt(1);
            if (n != 0) {
                strdig += dw[Number(n)] + "分"; //加数字
            }
        }
        str += "元" + strdig;
    } catch (e) {
        return "0元";
    }
    return str;
}
//拆分整数与小数
function splits(tranvalue) {
    var value = new Array("", "");
    temp = tranvalue.split(".");
    for (var i = 0; i < temp.length; i++) {
        value = temp;
    }
    return value;
}
```

#### **实现utf8解码**

```js
function utf8_decode(str_data) {
    var tmp_arr = [],
        i = 0,
        ac = 0,
        c1 = 0,
        c2 = 0,
        c3 = 0;
    str_data += "";
    while (i < str_data.length) {
        c1 = str_data.charCodeAt(i);
        if (c1 < 128) {
            tmp_arr[ac++] = String.fromCharCode(c1);
            i++;
        } else if (c1 > 191 && c1 < 224) {
            c2 = str_data.charCodeAt(i + 1);
            tmp_arr[ac++] = String.fromCharCode(((c1 & 31) << 6) | (c2 & 63));
            i += 2;
        } else {
            c2 = str_data.charCodeAt(i + 1);
            c3 = str_data.charCodeAt(i + 2);
            tmp_arr[ac++] = String.fromCharCode(
                ((c1 & 15) << 12) | ((c2 & 63) << 6) | (c3 & 63)
            );
            i += 3;
        }
    }
    return tmp_arr.join("");
}
```

#### **清除所有空格**

```js
const clearSpaces = (str) => {
    return str.replace(/[ ]/g, '');
}
```

#### **校验是否包含空格**

```js
const haveSpace = (str) => {
    return /[ ]/.test(str);
}
```

#### **校验是否包含中文字符(包括中文标点符号)**

```js
const haveCNChars = (str) => {
    return /[\u4e00-\u9fa5]/.test(str);
}
```

#### **复制内容到剪切板**

```javascript
const copyToClipboard = (text) => navigator.clipboard.writeText(text);

copyToClipboard("Hello World");
```

#### **清除所有cookie**

```javascript
const clearCookies = document.cookie.split(';').forEach(cookie => document.cookie = cookie.replace(/^ +/, '').replace(/=.*/, `=;expires=${new Date(0).toUTCString()};path=/`));
```

#### **获取选中的文本**

```javascript
const getSelectedText = () => window.getSelection().toString();

getSelectedText();
```

#### **滚动到页面顶部**

```javascript
const goToTop = () => window.scrollTo(0, 0);

goToTop();
```

#### **判断是否是苹果设备**

```javascript
const isAppleDevice = () => /Mac|iPod|iPhone|iPad/.test(navigator.platform);

isAppleDevice();
```

#### **华氏度和摄氏度之间的转化**

```javascript
const celsiusToFahrenheit = (celsius) => celsius * 9 / 5 + 32;
const fahrenheitToCelsius = (fahrenheit) => (fahrenheit - 32) * 5 / 9;

celsiusToFahrenheit(15); // 59
celsiusToFahrenheit(0); // 32
celsiusToFahrenheit(-20); // -4
fahrenheitToCelsius(59); // 15
fahrenheitToCelsius(32); // 0
```

#### **获取元素中的所有图像**

```js
const getImages = (el, includeDuplicates = false) => {
    const images = [...el.getElementsByTagName('img')].map(img => img.getAttribute('src'));
    return includeDuplicates ? images : [...new Set(images)];
};

// 事例：includeDuplicates 为 true 表示需要排除重复元素
getImages(document, true); // ['image1.jpg', 'image2.png', 'image1.png', '...']
getImages(document, false); // ['image1.jpg', 'image2.png', '...']
```

#### **创建一个包含当前URL参数的对象**

```js
const getURLParameters = url =>
    (url.match(/([^?=&]+)(=([^&]*))/g) || []).reduce(
        (a, v) => ((a[v.slice(0, v.indexOf('='))] = v.slice(v.indexOf('=') + 1)), a), {}
    );
getURLParameters('http://url.com/page?n=Adam&s=Smith'); // {n: 'Adam', s: 'Smith'}
getURLParameters('google.com'); // {}
```

#### **GET请求**

```js
const httpGet = (url, callback, err = console.error) => {
    const request = new XMLHttpRequest();
    request.open('GET', url, true);
    request.onload = () => callback(request.responseText);
    request.onerror = () => err(request);
    request.send();
};

httpGet(
    'https://jsonplaceholder.typicode.com/posts/1',
    console.log
);

// {"userId": 1, "id": 1, "title": "sample title", "body": "my text"}
```

#### **POST请求**

```js
const httpPost = (url, data, callback, err = console.error) => {
    const request = new XMLHttpRequest();
    request.open('POST', url, true);
    request.setRequestHeader('Content-type', 'application/json; charset=utf-8');
    request.onload = () => callback(request.responseText);
    request.onerror = () => err(request);
    request.send(data);
};

const newPost = {
    userId: 1,
    id: 1337,
    title: 'Foo',
    body: 'bar bar bar'
};
const data = JSON.stringify(newPost);
httpPost(
    'https://jsonplaceholder.typicode.com/posts',
    data,
    console.log
);

// {"userId": 1, "id": 1337, "title": "Foo", "body": "bar bar bar"}
```

#### **将一组表单元素转化为对象**

```js
const formToObject = form =>
    Array.from(new FormData(form)).reduce(
        (acc, [key, value]) => ({
            ...acc,
            [key]: value
        }), {}
    );

// 事例
formToObject(document.querySelector('#form'));
// { email: 'test@email.com', name: 'Test Name' }
```

#### 判断设备是移动设备还是台式机/笔记本电脑？

```js
const detectDeviceType = () =>
    /Android|webOS|iPhone|iPad|iPod|BlackBerry|IEMobile|Opera Mini/i.test(navigator.userAgent) ?
    'Mobile' :
    'Desktop';

// 事例
detectDeviceType(); // "Mobile" or "Desktop"
```

#### **金额千分位  - 逆**

```js
//去除千分位中的‘，’ 保留.00
function delcommafy(num){
	num=num.replace(/,/gi,'');
	return num;
}
// 千分位转数字 //去除千分位中的‘，’ 和 ‘.00’
const changeNumber = (val) => {
    if (val) {
        let a = (val +"").replace(/,/g,"");
        return Number(a)
    }else {
        return 0
    }
};
```

#### **检测页面是否滚动到页面底部**

```js
const bottomVisible = () =>
    document.documentElement.clientHeight + window.scrollY >=
    (document.documentElement.scrollHeight || document.documentElement.clientHeight);

bottomVisible(); // true
```

#### **当前是今年的第几天**

```js
const dayOfYear = date =>
    Math.floor((date - new Date(date.getFullYear(), 0, 0)) / 1000 / 60 / 60 / 24);

dayOfYear(new Date()); // 272
```

#### **返回两个日期之间相差多少天**

```js
const getDaysDiffBetweenDates = (dateInitial, dateFinal) =>
    (dateFinal - dateInitial) / (1000 * 3600 * 24);

getDaysDiffBetweenDates(new Date('2019-01-13'), new Date('2019-01-15')); // 2
```

#### **返回数据的类型**

```js
const getType = v =>
    v === undefined ? 'undefined' : v === null ? 'null' : v.constructor.name.toLowerCase();

getType(new Set([1, 2, 3])); // 'set'
```

#### **判断程序运行环境**

```js
const isBrowser = () => ![typeof window, typeof document].includes('undefined');

isBrowser(); // true (browser)
isBrowser(); // false (Node)
```

#### 检查当前的值是否为数字类型

```js
const isNumber = (n) = {
    return !isNaN(parseFloat(n)) && isFinite(n);
}

isNumber('1'); // false
isNumber(1); // true
```

#### **判断给定的两个日期是否是同一天**

```js
const isSameDate = (dateA, dateB) => dateA.toISOString() === dateB.toISOString();

isSameDate(new Date(2010, 10, 20), new Date(2010, 10, 20)); // true
```

#### **比较两个对象，以确定第一个对象是否包含与第二个对象相同的属性值**

```js
onst matches = (obj, source) =>
    Object.keys(source).every(key => obj.hasOwnProperty(key) && obj[key] === source[key]);

matches({
    age: 25,
    hair: 'long',
    beard: true
}, {
    hair: 'long',
    beard: true
}); // true
matches({
    hair: 'long',
    beard: true
}, {
    age: 25,
    hair: 'long',
    beard: true
}); // false
```

#### **查找日期数组中最大的日期**

```js
const maxDate = (...dates) => new Date(Math.max.apply(null, ...dates));

const array = [
    new Date(2017, 4, 13),
    new Date(2018, 2, 12),
    new Date(2016, 0, 10),
    new Date(2016, 0, 9)
];
maxDate(array); // 2018-03-11T22:00:00.000Z
```

#### **查找日期数组中最早的日期**

```js
const minDate = (...dates) => new Date(Math.min.apply(null, ...dates));

const array = [
    new Date(2017, 4, 13),
    new Date(2018, 2, 12),
    new Date(2016, 0, 10),
    new Date(2016, 0, 9)
];
minDate(array); // 2016-01-08T22:00:00.000Z
```

#### 查找数组中前 n 位最大的数

```js
const maxN = (arr, n = 1) => [...arr].sort((a, b) => b - a).slice(0, n);

maxN([1, 2, 3]); // [3]
maxN([1, 2, 3], 2); // [3,2]
```

#### **查找数组中前 n 位最小的数**

```js
const minN = (arr, n = 1) => [...arr].sort((a, b) => a - b).slice(0, n);

minN([1, 2, 3]); // [1]
minN([1, 2, 3], 2); // [1,2]
```

#### **将读取到的文本内容，按行分割组成数组进行输出(Node.js 的 fs模块)**

```js
const fs = require('fs');
const readFileLines = filename =>
    fs
    .readFileSync(filename)
    .toString('UTF8')
    .split('\n');

let arr = readFileLines('test.txt');
console.log(arr); // ['line1', 'line2', 'line3']
```

#### **颠倒字符串**

```js
const reverseString = str => [...str].reverse().join('');

reverseString('foobar'); // 'raboof'
```

#### **将小数按照指定的位数，进行四舍五入保留**

```js
const round = (n, decimals = 0) => Number(`${Math.round(`${n}e${decimals}`)}e-${decimals}`);

round(1.005, 2); // 1.01
```

#### **通过数组的形式，连续运行多个promise**

```js
const runPromisesInSeries = ps => ps.reduce((p, next) => p.then(next), Promise.resolve());
const delay = d => new Promise(r => setTimeout(r, d));

runPromisesInSeries([() => delay(1000), () => delay(2000)]);
// Executes each promise sequentially, taking a total of 3 seconds to complete
```

#### **页面平滑滚动到顶部**

```js
const scrollToTop = () => {
    const c = document.documentElement.scrollTop || document.body.scrollTop;
    if (c > 0) {
        window.requestAnimationFrame(scrollToTop);
        window.scrollTo(0, c - c / 8);
    }
};

scrollToTop();
```

#### **洗牌算法打乱数组**

使用 Fisher–Yates shuffle 洗牌算法对数组的内容进行随机排序，生成新的数组。

什么是 Fisher–Yates shuffle 洗牌算法? 算法是一个用来将一个有限集合生成一个随机排列的算法（数组随机排序）。这个算法生成的随机排列是等概率的。同时这个算法非常高效。

```js
const shuffle = ([...arr]) => {
    let m = arr.length;
    while (m) {
        const i = Math.floor(Math.random() * m--);
        [arr[m], arr[i]] = [arr[i], arr[m]];
    }
    return arr;
};

const foo = [1, 2, 3];
shuffle(foo); // [2, 3, 1], foo = [1, 2, 3]
```

#### **查找两个数组交集**

```js
const similarity = (arr, values) => arr.filter(v => values.includes(v));

similarity([1, 2, 3], [1, 2, 4]); // [1, 2]
```

#### **按照指定的货币类型格式化货币数字**

```js
const toCurrency = (n, curr, LanguageFormat = undefined) =>
    Intl.NumberFormat(LanguageFormat, {
        style: 'currency',
        currency: curr
    }).format(n);

toCurrency(123456.789, 'EUR');
// €123,456.79  | currency: Euro | currencyLangFormat: Local
toCurrency(123456.789, 'USD', 'en-us');
// $123,456.79  | currency: US Dollar | currencyLangFormat: English (United States)
toCurrency(123456.789, 'USD', 'fa');
// ۱۲۳٬۴۵۶٫۷۹ ؜$ | currency: US Dollar | currencyLangFormat: Farsi
toCurrency(322342436423.2435, 'JPY');
// ¥322,342,436,423 | currency: Japanese Yen | currencyLangFormat: Local
toCurrency(322342436423.2435, 'JPY', 'fi');
// 322 342 436 423 ¥ | currency: Japanese Yen | currencyLangFormat: Finnish
```

#### **获取明天的日期**

```js
const tomorrow = () => {
    let t = new Date();
    t.setDate(t.getDate() + 1);
    return t.toISOString().split('T')[0];
};

tomorrow();
// 2019-09-08 (if current date is 2018-09-08)
```
