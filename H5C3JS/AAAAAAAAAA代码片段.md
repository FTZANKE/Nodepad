## JavaScript工具函数

AHao 2112141919

#### **01 为元素添加on方法**

```js
Element.prototype.on = Element.prototype.addEventListener;
NodeList.prototype.on = function(event, fn) {
    []['forEach'].call(this, function(el) {
        el.on(event, fn);
    });
    return this;
};
```

#### **02 为元素添加trigger方法**

```js
Element.prototype.trigger = function(type, data) {
    var event = document.createEvent("HTMLEvents");
    event.initEvent(type, true, true);
    event.data = data || {};
    event.eventName = type;
    event.target = this;
    this.dispatchEvent(event);
    return this;
};
NodeList.prototype.trigger = function(event) {
    []["forEach"].call(this, function(el) {
        el["trigger"](event);
    });
    return this;
};
```

#### **03 转义html标签**

```js
function HtmlEncode(text) {
    return text
        .replace(/&/g, "&")
        .replace(/\"/g, '"')
        .replace(/</g, "<")
        .replace(/>/g, ">");
}
```

#### **04 HTML标签转义**

```js
// HTML 标签转义
// @param {Array.<DOMString>} templateData 字符串类型的tokens
// @param {...} ..vals 表达式占位符的运算结果tokens
//
function SaferHTML(templateData) {
    var s = templateData[0];
    for (var i = 1; i < arguments.length; i++) {
        var arg = String(arguments[i]);
        // Escape special characters in the substitution.
        s += arg
            .replace(/&/g, "&")
            .replace(/</g, "<")
            .replace(/>/g, ">");
        // Don't escape special characters in the template.
        s += templateData[i];
    }
    return s;
}
// 调用
var html = SaferHTML`<p>这是关于字符串模板的介绍</p>`;
```

#### **05 跨浏览器绑定事件**

```js
function addEventSamp(obj, evt, fn) {
    if (!oTarget) {
        return;
    }
    if (obj.addEventListener) {
        obj.addEventListener(evt, fn, false);
    } else if (obj.attachEvent) {
        obj.attachEvent("on" + evt, fn);
    } else {
        oTarget["on" + sEvtType] = fn;
    }
}
```

#### **06 加入收藏夹**

```js
function addFavorite(sURL, sTitle) {
    try {
        window.external.addFavorite(sURL, sTitle);
    } catch (e) {
        try {
            window.sidebar.addPanel(sTitle, sURL, "");
        } catch (e) {
            alert("加入收藏失败，请使用Ctrl+D进行添加");
        }
    }
}
```

#### **07 提取页面代码中所有网址**

```js
var aa = document.documentElement.outerHTML
    .match(
        /(url\(|src=|href=)[\"\']*([^\"\'\(\)\<\>\[\] ]+)[\"\'\)]*|(http:\/\/[\w\-\.]+[^\"\'\(\)\<\>\[\] ]+)/gi
    )
    .join("\r\n")
    .replace(/^(src=|href=|url\()[\"\']*|[\"\'\>\) ]*$/gim, "");
alert(aa);
```

#### **08 动态加载脚本文件**

```js
function appendscript(src, text, reload, charset) {
    var id = hash(src + text);
    if (!reload && in_array(id, evalscripts)) return;
    if (reload && $(id)) {
        $(id).parentNode.removeChild($(id));
    }
    evalscripts.push(id);
    var scriptNode = document.createElement("script");
    scriptNode.type = "text/javascript";
    scriptNode.id = id;
    scriptNode.charset = charset ?
        charset :
        BROWSER.firefox ?
        document.characterSet :
        document.charset;
    try {
        if (src) {
            scriptNode.src = src;
            scriptNode.onloadDone = false;
            scriptNode.onload = function() {
                scriptNode.onloadDone = true;
                JSLOADED[src] = 1;
            };
            scriptNode.onreadystatechange = function() {
                if (
                    (scriptNode.readyState == "loaded" ||
                        scriptNode.readyState == "complete") &&
                    !scriptNode.onloadDone
                ) {
                    scriptNode.onloadDone = true;
                    JSLOADED[src] = 1;
                }
            };
        } else if (text) {
            scriptNode.text = text;
        }
        document.getElementsByTagName("head")[0].appendChild(scriptNode);
    } catch (e) {}
}
```

#### **09 返回顶部的通用方法**

```js
function backTop(btnId) {
    var btn = document.getElementById(btnId);
    var d = document.documentElement;
    var b = document.body;
    window.onscroll = set;
    btn.style.display = "none";
    btn.onclick = function() {
        btn.style.display = "none";
        window.onscroll = null;
        this.timer = setInterval(function() {
            d.scrollTop -= Math.ceil((d.scrollTop + b.scrollTop) * 0.1);
            b.scrollTop -= Math.ceil((d.scrollTop + b.scrollTop) * 0.1);
            if (d.scrollTop + b.scrollTop == 0)
                clearInterval(btn.timer, (window.onscroll = set));
        }, 10);
    };

    function set() {
        btn.style.display = d.scrollTop + b.scrollTop > 100 ? "block" : "none";
    }
}
backTop("goTop");
```

#### **10 实现base64解码**

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

#### **11 确认是否是键盘有效输入值**

```js
function checkKey(iKey) {
    if (iKey == 32 || iKey == 229) {
        return true;
    } /*空格和异常*/
    if (iKey > 47 && iKey < 58) {
        return true;
    } /*数字*/
    if (iKey > 64 && iKey < 91) {
        return true;
    } /*字母*/
    if (iKey > 95 && iKey < 108) {
        return true;
    } /*数字键盘1*/
    if (iKey > 108 && iKey < 112) {
        return true;
    } /*数字键盘2*/
    if (iKey > 185 && iKey < 193) {
        return true;
    } /*符号1*/
    if (iKey > 218 && iKey < 223) {
        return true;
    } /*符号2*/
    return false;
}
```

#### **12 全角半角转换**

```js
//iCase: 0全到半，1半到全，其他不转化
function chgCase(sStr, iCase) {
    if (
        typeof sStr != "string" ||
        sStr.length <= 0 ||
        !(iCase === 0 || iCase == 1)
    ) {
        return sStr;
    }
    var i,
        oRs = [],
        iCode;
    if (iCase) {
        /*半->全*/
        for (i = 0; i < sStr.length; i += 1) {
            iCode = sStr.charCodeAt(i);
            if (iCode == 32) {
                iCode = 12288;
            } else if (iCode < 127) {
                iCode += 65248;
            }
            oRs.push(String.fromCharCode(iCode));
        }
    } else {
        /*全->半*/
        for (i = 0; i < sStr.length; i += 1) {
            iCode = sStr.charCodeAt(i);
            if (iCode == 12288) {
                iCode = 32;
            } else if (iCode > 65280 && iCode < 65375) {
                iCode -= 65248;
            }
            oRs.push(String.fromCharCode(iCode));
        }
    }
    return oRs.join("");
}
```

#### **13 版本对比**

```js
function compareVersion(v1, v2) {
    v1 = v1.split(".");
    v2 = v2.split(".");
    var len = Math.max(v1.length, v2.length);
    while (v1.length < len) {
        v1.push("0");
    }
    while (v2.length < len) {
        v2.push("0");
    }
    for (var i = 0; i < len; i++) {
        var num1 = parseInt(v1[i]);
        var num2 = parseInt(v2[i]);
        if (num1 > num2) {
            return 1;
        } else if (num1 < num2) {
            return -1;
        }
    }
    return 0;
}
```

#### **14 压缩CSS样式代码**

```js
function compressCss(s) {
    //压缩代码
    s = s.replace(/\/\*(.|\n)*?\*\//g, ""); //删除注释
    s = s.replace(/\s*([\{\}\:\;\,])\s*/g, "$1");
    s = s.replace(/\,[\s\.\#\d]*\{/g, "{"); //容错处理
    s = s.replace(/;\s*;/g, ";"); //清除连续分号
    s = s.match(/^\s*(\S+(\s+\S+)*)\s*$/); //去掉首尾空白
    return s == null ? "" : s[1];
}
```

#### **15 获取当前路径**

```js
var currentPageUrl = "";
if (typeof this.href === "undefined") {
    currentPageUrl = document.location.toString().toLowerCase();
} else {
    currentPageUrl = this.href.toString().toLowerCase();
}
```

#### **16 字符串长度截取**

```js
function cutstr(str, len) {
    var temp,
        icount = 0,
        patrn = /[^\x00-\xff]/，
    strre = "";
    for (var i = 0; i < str.length; i++) {
        if (icount < len - 1) {
            temp = str.substr(i, 1);
            if (patrn.exec(temp) == null) {
                icount = icount + 1
            } else {
                icount = icount + 2
            }
            strre += temp
        } else {
            break;
        }
    }
    return strre + "..."
}
```

#### **17 时间日期格式转换**

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

#### **18 跨浏览器删除事件**

```js
function delEvt(obj, evt, fn) {
    if (!obj) {
        return;
    }
    if (obj.addEventListener) {
        obj.addEventListener(evt, fn, false);
    } else if (oTarget.attachEvent) {
        obj.attachEvent("on" + evt, fn);
    } else {
        obj["on" + evt] = fn;
    }
}
```

#### **19 判断是否以某个字符串结束**

```js
String.prototype.endWith = function(s) {
    var d = this.length - s.length;
    return d >= 0 && this.lastIndexOf(s) == d;
};
```

#### **20 返回脚本内容**

```js
function evalscript(s) {
    if (s.indexOf("<script") == -1) return s;
    var p = /<script[^\>]*?>([^\x00]*?)<\/script>/gi;
    var arr = [];
    while ((arr = p.exec(s))) {
        var p1 = /<script[^\>]*?src=\"([^\>]*?)\"[^\>]*?(reload=\"1\")?(?:charset=\"([\w\-]+?)\")?><\/script>/i;
        var arr1 = [];
        arr1 = p1.exec(arr[0]);
        if (arr1) {
            appendscript(arr1[1], "", arr1[2], arr1[3]);
        } else {
            p1 = /<script(.*?)>([^\x00]+?)<\/script>/i;
            arr1 = p1.exec(arr[0]);
            appendscript("", arr1[2], arr1[1].indexOf("reload=") != -1);
        }
    }
    return s;
}
```

#### **21 格式化CSS样式代码**

```js
function formatCss(s) {
    //格式化代码
    s = s.replace(/\s*([\{\}\:\;\,])\s*/g, "$1");
    s = s.replace(/;\s*;/g, ";"); //清除连续分号
    s = s.replace(/\,[\s\.\#\d]*{/g, "{");
    s = s.replace(/([^\s])\{([^\s])/g, "$1 {\n\t$2");
    s = s.replace(/([^\s])\}([^\n]*)/g, "$1\n}\n$2");
    s = s.replace(/([^\s]);([^\s\}])/g, "$1;\n\t$2");
    return s;
}
```

#### **22 获取cookie值**

```js
function getCookie(name) {
    var arr = document.cookie.match(new RegExp("(^| )" + name + "=([^;]*)(;|$)"));
    if (arr != null) return unescape(arr[2]);
    return null;
}
```

#### **23 获得URL中GET参数值**

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

#### **24 获取移动设备初始化大小**

```js
function getInitZoom() {
    if (!this._initZoom) {
        var screenWidth = Math.min(screen.height, screen.width);
        if (this.isAndroidMobileDevice() && !this.isNewChromeOnAndroid()) {
            screenWidth = screenWidth / window.devicePixelRatio;
        }
        this._initZoom = screenWidth / document.body.offsetWidth;
    }
    return this._initZoom;
}
```

#### **25 获取页面高度**

```js
function getPageHeight() {
    var g = document,
        a = g.body,
        f = g.documentElement,
        d = g.compatMode == "BackCompat" ? a : g.documentElement;
    return Math.max(f.scrollHeight, a.scrollHeight, d.clientHeight);
}
```

#### **26 获取页面scrollLeft**

```js
function getPageScrollLeft() {
    var a = document;
    return a.documentElement.scrollLeft || a.body.scrollLeft;
}
```

#### **27 获取页面scrollTop**

```js
function getPageScrollTop() {
    var a = document;
    return a.documentElement.scrollTop || a.body.scrollTop;
}
```

#### **28 获取页面可视宽度**

```js
function getPageViewWidth() {
    var d = document,
        a = d.compatMode == "BackCompat" ? d.body : d.documentElement;
    return a.clientWidth;
}
```

#### **29 获取页面可视高度**

```js
function getPageViewHeight() {
    var d = document,
        a = d.compatMode == "BackCompat" ? d.body : d.documentElement;
    return a.clientHeight;
}
```

#### **30 获取页面宽度**

```js
function getPageWidth() {
    var g = document,
        a = g.body,
        f = g.documentElement,
        d = g.compatMode == "BackCompat" ? a : g.documentElement;
    return Math.max(f.scrollWidth, a.scrollWidth, d.clientWidth);
}
```

#### **31 获取移动设备屏幕宽度**

```js
function getScreenWidth() {
    var smallerSide = Math.min(screen.width, screen.height);
    var fixViewPortsExperiment =
        rendererModel.runningExperiments.FixViewport ||
        rendererModel.runningExperiments.fixviewport;
    var fixViewPortsExperimentRunning =
        fixViewPortsExperiment && fixViewPortsExperiment.toLowerCase() === "new";
    if (fixViewPortsExperiment) {
        if (this.isAndroidMobileDevice() && !this.isNewChromeOnAndroid()) {
            smallerSide = smallerSide / window.devicePixelRatio;
        }
    }
    return smallerSide;
}
```

#### **32 获取移动设备最大化大小**

```js
function getZoom() {
    var screenWidth =
        Math.abs(window.orientation) === 90 ?
        Math.max(screen.height, screen.width) :
        Math.min(screen.height, screen.width);
    if (this.isAndroidMobileDevice() && !this.isNewChromeOnAndroid()) {
        screenWidth = screenWidth / window.devicePixelRatio;
    }
    var FixViewPortsExperiment =
        rendererModel.runningExperiments.FixViewport ||
        rendererModel.runningExperiments.fixviewport;
    var FixViewPortsExperimentRunning =
        FixViewPortsExperiment &&
        (FixViewPortsExperiment === "New" || FixViewPortsExperiment === "new");
    if (FixViewPortsExperimentRunning) {
        return screenWidth / window.innerWidth;
    } else {
        return screenWidth / document.body.offsetWidth;
    }
}
```

#### **33 获取网页被卷去的位置**

```js
function getScrollXY() {
    return document.body.scrollTop ? {
        x: document.body.scrollLeft,
        y: document.body.scrollTop
    } : {
        x: document.documentElement.scrollLeft,
        y: document.documentElement.scrollTop
    };
}
```

#### **34 判断是否为数字类型**

```js
function isDigit(value) {
    var patrn = /^[0-9]*$/;
    if (patrn.exec(value) == null || value == "") {
        return false;
    } else {
        return true;
    }
}
```

#### **35 检验URL链接是否有效**

```js
function getUrlState(URL) {
    var xmlhttp = new ActiveXObject("microsoft.xmlhttp");
    xmlhttp.Open("GET", URL, false);
    try {
        xmlhttp.Send();
    } catch (e) {} finally {
        var result = xmlhttp.responseText;
        if (result) {
            if (xmlhttp.Status == 200) {
                return true;
            } else {
                return false;
            }
        } else {
            return false;
        }
    }
}
```

#### **36 获取URL上的参数**

```js
// 获取URL中的某参数值,不区分大小写
// 获取URL中的某参数值,不区分大小写,
// 默认是取'hash'里的参数，
// 如果传其他参数支持取‘search’中的参数
// @param {String} name 参数名称
export function getUrlParam(name, type = "hash") {
    let newName = name,
        reg = new RegExp("(^|&)" + newName + "=([^&]*)(&|$)", "i"),
        paramHash = window.location.hash.split("?")[1] || "",
        paramSearch = window.location.search.split("?")[1] || "",
        param;
    type === "hash" ? (param = paramHash) : (param = paramSearch);
    let result = param.match(reg);
    if (result != null) {
        return result[2].split("/")[0];
    }
    return null;
}
```

#### **37 获取窗体可见范围的宽与高**

```js
function getViewSize() {
    var de = document.documentElement;
    var db = document.body;
    var viewW = de.clientWidth == 0 ? db.clientWidth : de.clientWidth;
    var viewH = de.clientHeight == 0 ? db.clientHeight : de.clientHeight;
    return Array(viewW, viewH);
}
```

#### **38 判断是否安卓移动设备访问**

```js
function isAndroidMobileDevice() {
    return /android/i.test(navigator.userAgent.toLowerCase());
}
```

#### **39 判断是否苹果移动设备访问**

```js
function isAppleMobileDevice() {
    return /iphone|ipod|ipad|Macintosh/i.test(navigator.userAgent.toLowerCase());
}
```

#### **40 是否是某类手机型号**

```js
// 用devicePixelRatio和分辨率判断
const isIphonex = () => {
    // X XS, XS Max, XR
    const xSeriesConfig = [{
            devicePixelRatio: 3,
            width: 375,
            height: 812
        },
        {
            devicePixelRatio: 3,
            width: 414,
            height: 896
        },
        {
            devicePixelRatio: 2,
            width: 414,
            height: 896
        }
    ];
    // h5
    if (typeof window !== "undefined" && window) {
        const isIOS = /iphone/gi.test(window.navigator.userAgent);
        if (!isIOS) return false;
        const {
            devicePixelRatio,
            screen
        } = window;
        const {
            width,
            height
        } = screen;
        return xSeriesConfig.some(
            item =>
            item.devicePixelRatio === devicePixelRatio &&
            item.width === width &&
            item.height === height
        );
    }
    return false;
};
```

#### **41 判断是否是移动设备访问**

```js
function isMobileUserAgent() {
    return /iphone|ipod|android.*mobile|windows.*phone|blackberry.*mobile/i.test(
        window.navigator.userAgent.toLowerCase()
    );
}
```

#### **42 判断是否移动设备**

```js
function isMobile() {
    if (typeof this._isMobile === "boolean") {
        return this._isMobile;
    }
    var screenWidth = this.getScreenWidth();
    var fixViewPortsExperiment =
        rendererModel.runningExperiments.FixViewport ||
        rendererModel.runningExperiments.fixviewport;
    var fixViewPortsExperimentRunning =
        fixViewPortsExperiment && fixViewPortsExperiment.toLowerCase() === "new";
    if (!fixViewPortsExperiment) {
        if (!this.isAppleMobileDevice()) {
            screenWidth = screenWidth / window.devicePixelRatio;
        }
    }
    var isMobileScreenSize = screenWidth < 600;
    var isMobileUserAgent = false;
    this._isMobile = isMobileScreenSize && this.isTouchScreen();
    return this._isMobile;
}
```

#### **43 判断是否手机号码**

```js
function isMobileNumber(e) {
    var i =
        "134,135,136,137,138,139,150,151,152,157,158,159,187,188,147,182,183,184,178",
        n = "130,131,132,155,156,185,186,145,176",
        a = "133,153,180,181,189,177,173,170",
        o = e || "",
        r = o.substring(0, 3),
        d = o.substring(0, 4),
        s = !!/^1\d{10}$/.test(o) &&
        (n.indexOf(r) >= 0 ?
            "联通" :
            a.indexOf(r) >= 0 ?
            "电信" :
            "1349" == d ?
            "电信" :
            i.indexOf(r) >= 0 ?
            "移动" :
            "未知");
    return s;
}
```

#### **44 判断鼠标是否移出事件**

```js
function isMouseOut(e, handler) {
    if (e.type !== "mouseout") {
        return false;
    }
    var reltg = e.relatedTarget ?
        e.relatedTarget :
        e.type === "mouseout" ?
        e.toElement :
        e.fromElement;
    while (reltg && reltg !== handler) {
        reltg = reltg.parentNode;
    }
    return reltg !== handler;
}
```

#### **45 判断是否Touch屏幕**

```js
function isTouchScreen() {
    return (
        "ontouchstart" in window ||
        (window.DocumentTouch && document instanceof DocumentTouch)
    );
}
```

#### **46 判断是否为网址**

```js
function isURL(strUrl) {
    var regular = /^\b(((https?|ftp):\/\/)?[-a-z0-9]+(\.[-a-z0-9]+)*\.(?:com|edu|gov|int|mil|net|org|biz|info|name|museum|asia|coop|aero|[a-z][a-z]|((25[0-5])|(2[0-4]\d)|(1\d\d)|([1-9]\d)|\d))\b(\/[-a-z0-9_:\@&?=+,.!\/~%\$]*)?)$/i;
    if (regular.test(strUrl)) {
        return true;
    } else {
        return false;
    }
}
```

#### **47 判断是否打开视窗**

```js
function isViewportOpen() {
    return !!document.getElementById("wixMobileViewport");
}
```

#### **48 加载样式文件**

```js
function loadStyle(url) {
    try {
        document.createStyleSheet(url);
    } catch (e) {
        var cssLink = document.createElement("link");
        cssLink.rel = "stylesheet";
        cssLink.type = "text/css";
        cssLink.href = url;
        var head = document.getElementsByTagName("head")[0];
        head.appendChild(cssLink);
    }
}
```

#### **49 替换地址栏**

```js
function locationReplace(url) {
    if (history.replaceState) {
        history.replaceState(null, document.title, url);
        history.go(0);
    } else {
        location.replace(url);
    }
}
```

#### **50 解决offsetX兼容性问题**

```js
// 针对火狐不支持offsetX/Y
function getOffset(e) {
    var target = e.target, // 当前触发的目标对象
        eventCoord,
        pageCoord,
        offsetCoord;
    // 计算当前触发元素到文档的距离
    pageCoord = getPageCoord(target);
    // 计算光标到文档的距离
    eventCoord = {
        X: window.pageXOffset + e.clientX,
        Y: window.pageYOffset + e.clientY
    };
    // 相减获取光标到第一个定位的父元素的坐标
    offsetCoord = {
        X: eventCoord.X - pageCoord.X,
        Y: eventCoord.Y - pageCoord.Y
    };
    return offsetCoord;
}

function getPageCoord(element) {
    var coord = {
        X: 0,
        Y: 0
    };
    // 计算从当前触发元素到根节点为止，
    // 各级 offsetParent 元素的 offsetLeft 或 offsetTop 值之和
    while (element) {
        coord.X += element.offsetLeft;
        coord.Y += element.offsetTop;
        element = element.offsetParent;
    }
    return coord;
}
```

#### **51 打开一个窗体通用方法**

```js
function openWindow(url, windowName, width, height) {
    var x = parseInt(screen.width / 2.0) - width / 2.0;
    var y = parseInt(screen.height / 2.0) - height / 2.0;
    var isMSIE = navigator.appName == "Microsoft Internet Explorer";
    if (isMSIE) {
        var p = "resizable=1,location=no,scrollbars=no,width=";
        p = p + width;
        p = p + ",height=";
        p = p + height;
        p = p + ",left=";
        p = p + x;
        p = p + ",top=";
        p = p + y;
        retval = window.open(url, windowName, p);
    } else {
        var win = window.open(
            url,
            "ZyiisPopup",
            "top=" +
            y +
            ",left=" +
            x +
            ",scrollbars=" +
            scrollbars +
            ",dialog=yes,modal=yes,width=" +
            width +
            ",height=" +
            height +
            ",resizable=no"
        );
        eval("try { win.resizeTo(width, height); } catch(e) { }");
        win.focus();
    }
}
```

#### **52 将键值对拼接成URL带参数**

```js
export default
const fnParams2Url = obj => {
    let aUrl = []
    let fnAdd = function(key, value) {
        return key + '=' + value
    }
    for (var k in obj) {
        aUrl.push(fnAdd(k, obj[k]))
    }
    return encodeURIComponent(aUrl.join('&'))
}
```

#### **53 去掉url前缀**

```js
function removeUrlPrefix(a) {
    a = a
        .replace(/：/g, ":")
        .replace(/．/g, ".")
        .replace(/／/g, "/");
    while (
        trim(a)
        .toLowerCase()
        .indexOf("http://") == 0
    ) {
        a = trim(a.replace(/http:\/\//i, ""));
    }
    return a;
}
```

#### **54 resize的操作**

```js
(function() {
    var fn = function() {
        var w = document.documentElement ?
            document.documentElement.clientWidth :
            document.body.clientWidth,
            r = 1255,
            b = Element.extend(document.body),
            classname = b.className;
        if (w < r) {
            //当窗体的宽度小于1255的时候执行相应的操作
        } else {
            //当窗体的宽度大于1255的时候执行相应的操作
        }
    };
    if (window.addEventListener) {
        window.addEventListener("resize", function() {
            fn();
        });
    } else if (window.attachEvent) {
        window.attachEvent("onresize", function() {
            fn();
        });
    }
    fn();
})();
```

#### **55 替换全部**

```js
String.prototype.replaceAll = function(s1, s2) {
    return this.replace(new RegExp(s1, "gm"), s2);
};
```

#### **56 设置cookie值**

```js
function setCookie(name, value, Hours) {
    var d = new Date();
    var offset = 8;
    var utc = d.getTime() + d.getTimezoneOffset() * 60000;
    var nd = utc + 3600000 * offset;
    var exp = new Date(nd);
    exp.setTime(exp.getTime() + Hours * 60 * 60 * 1000);
    document.cookie =
        name +
        "=" +
        escape(value) +
        ";path=/;expires=" +
        exp.toGMTString() +
        ";domain=360doc.com;";
}
```

#### **57 滚动到顶部**

```js
// 使用document.documentElement.scrollTop 或 document.body.scrollTop 获取到顶部的距离，从顶部
// 滚动一小部分距离。使用window.requestAnimationFrame()来滚动。
function scrollToTop() {
    var c = document.documentElement.scrollTop || document.body.scrollTop;
    if (c > 0) {
        window.requestAnimationFrame(scrollToTop);
        window.scrollTo(0, c - c / 8);
    }
}
```

#### **58 设为首页**

```js
function setHomepage() {
    if (document.all) {
        document.body.style.behavior = "url(#default#homepage)";
        document.body.setHomePage("http://w3cboy.com");
    } else if (window.sidebar) {
        if (window.netscape) {
            try {
                netscape.security.PrivilegeManager.enablePrivilege(
                    "UniversalXPConnect"
                );
            } catch (e) {
                alert(
                    "该操作被浏览器拒绝，如果想启用该功能，请在地址栏内输入 about:config,然后将项 signed.applets.codebase_principal_support 值该为true"
                );
            }
        }
        var prefs = Components.classes[
            "@mozilla.org/preferences-service;1"
        ].getService(Components.interfaces.nsIPrefBranch);
        prefs.setCharPref("browser.startup.homepage", "http://w3cboy.com");
    }
}
```

#### **59 按字母排序，对每行进行数组排序**

```js
function setSort() {
    var text = K1.value
        .split(/[\r\n]/)
        .sort()
        .join("\r\n"); //顺序
    var test = K1.value
        .split(/[\r\n]/)
        .sort()
        .reverse()
        .join("\r\n"); //反序
    K1.value = K1.value != text ? text : test;
}
```

#### **60 延时执行**

```js
// 比如 sleep(1000) 意味着等待1000毫秒，还可从 Promise、Generator、Async/Await 等角度实现。
// Promise
const sleep = time => {
    return new Promise(resolve => setTimeout(resolve, time));
};
sleep(1000).then(() => {
    console.log(1);
});
// Generator
function* sleepGenerator(time) {
    yield new Promise(function(resolve, reject) {
        setTimeout(resolve, time);
    });
}
sleepGenerator(1000)
    .next()
    .value.then(() => {
        console.log(1);
    });
//async
function sleep(time) {
    return new Promise(resolve => setTimeout(resolve, time));
}
async function output() {
    let out = await sleep(1000);
    console.log(1);
    return out;
}
output();

function sleep(callback, time) {
    if (typeof callback === "function") {
        setTimeout(callback, time);
    }
}

function output() {
    console.log(1);
}
sleep(output, 1000);
```

#### **61 清除脚本内容**

```js
function stripscript(s) {
    return s.replace(/<script.*?>.*?<\/script>/gi, "");
}
```

#### **62 判**断是否以某个字符串开头

```js
String.prototype.startWith = function(s) {
    return this.indexOf(s) == 0;
};
```

#### **63 时间个性化输出功能**

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

#### **64 半角转换为全角函数**

```js
function toDBC(str) {
    var result = "";
    for (var i = 0; i < str.length; i++) {
        code = str.charCodeAt(i);
        if (code >= 33 && code <= 126) {
            result += String.fromCharCode(str.charCodeAt(i) + 65248);
        } else if (code == 32) {
            result += String.fromCharCode(str.charCodeAt(i) + 12288 - 32);
        } else {
            result += str.charAt(i);
        }
    }
    return result;
}
```

#### **65 全角转换为半角函数**

```js
function toCDB(str) {
    var result = "";
    for (var i = 0; i < str.length; i++) {
        code = str.charCodeAt(i);
        if (code >= 65281 && code <= 65374) {
            result += String.fromCharCode(str.charCodeAt(i) - 65248);
        } else if (code == 12288) {
            result += String.fromCharCode(str.charCodeAt(i) - 12288 + 32);
        } else {
            result += str.charAt(i);
        }
    }
    return result;
}
```

#### **66 金额大写转换函数**

```js
function transform(tranvalue) {
    try {
        var i = 1;
        var dw2 = new Array("", "万", "亿"); //大单位
        var dw1 = new Array("拾", "佰", "仟"); //小单位
        var dw = new Array(
            "零",
            "壹",
            "贰",
            "叁",
            "肆",
            "伍",
            "陆",
            "柒",
            "捌",
            "玖"
        );
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

#### **67 清除空格**

```js
String.prototype.trim = function() {
    var reExtraSpace = /^\s*(.*?)\s+$/;
    return this.replace(reExtraSpace, "$1");
};
// 清除左空格
function ltrim(s) {
    return s.replace(/^(\s*|　*)/, "");
}
// 清除右空格
function rtrim(s) {
    return s.replace(/(\s*|　*)$/, "");
}
```

#### **68 随机数时间戳**

```js
function uniqueId() {
    var a = Math.random,
        b = parseInt;
    return (
        Number(new Date()).toString() + b(10 * a()) + b(10 * a()) + b(10 * a())
    );
}
```

#### **69 实现utf8解码**

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

以下的几个函数，用作常见的输入值校验和替换操作，主要针对中国大陆地区的校验规则：

#### **70 校验是否为一个数字，以及该数字小数点位数是否与参数floats一致**

校验规则：

* 若参数floats有值，则校验该数字小数点后的位数。
* 若参数floats没有值，则仅仅校验是否为数字。

```js
function isNum(value, floats = null) {
    let regexp = new RegExp(`^[1-9][0-9]*.[0-9]{${floats}}$|^0.[0-9]{${floats}}$`);
    return typeof value === 'number' && floats ? regexp.test(String(value)) : true;
}

function anysicIntLength(minLength, maxLength) {
    let result_str = '';
    if (minLength) {
        switch (maxLength) {
            case undefined:
                result_str = result_str.concat(`{${minLength-1}}`);
                break;
            case null:
                result_str = result_str.concat(`{${minLength-1},}`);
                break;
            default:
                result_str = result_str.concat(`{${minLength-1},${maxLength-1}}`);
        }
    } else {
        result_str = result_str.concat('*');
    }
    return result_str;
}
```

#### **71 校验是否为非零的正整数**

```js
function isInt(value, minLength = null, maxLength = undefined) {
    if (!isNum(value)) return false;
    let regexp = new RegExp(`^-?[1-9][0-9]${anysicIntLength(minLength,maxLength)}$`);
    return regexp.test(value.toString());
}
```

#### **72 校验是否为非零的正整数**

```js
function isPInt(value, minLength = null, maxLength = undefined) {
    if (!isNum(value)) return false;
    let regexp = new RegExp(`^[1-9][0-9]${anysicIntLength(minLength,maxLength)}$`);
    return regexp.test(value.toString());
}
```

#### **73 校验是否为非零的负整数**

```js
function isNInt(value, minLength = null, maxLength = undefined) {
    if (!isNum(value)) return false;
    let regexp = new RegExp(`^-[1-9][0-9]${anysicIntLength(minLength,maxLength)}$`);
    return regexp.test(value.toString());
}
```

#### **74 校验整数是否在取值范围内**

校验规则：

* minInt为在取值范围中最小的整数
* maxInt为在取值范围中最大的整数

```js
function checkIntRange(value, minInt, maxInt = 9007199254740991) {
    return Boolean(isInt(value) && (Boolean(minInt != undefined && minInt != null) ? value >= minInt : true) && (value <= maxInt));
}
```

#### **75 校验是否为中国大陆传真或固定电话号码**

```js
function isFax(str) {
    return /^([0-9]{3,4})?[0-9]{7,8}$|^([0-9]{3,4}-)?[0-9]{7,8}$/.test(str);
}
```

#### **76 校验是否为中国大陆手机号**

```js
function isTel(value) {
    return /^1[3,4,5,6,7,8,9][0-9]{9}$/.test(value.toString());
}
```

#### **77 校验是否为邮箱地址**

```js
function isEmail(str) {
    return /^[a-zA-Z0-9_-]+@[a-zA-Z0-9_-]+(\.[a-zA-Z0-9_-]+)+$/.test(str);
}
```

#### **78 校验是否为QQ号码**

校验规则：

* 非0开头的5位-13位整数

```js
function isQQ(value) {
    return /^[1-9][0-9]{4,12}$/.test(value.toString());
}
```

#### **79 校验是否为网址**

校验规则：

* 以`https://`, `http://`, `ftp://`, `rtsp://`, `mms://`开头、或者没有这些开头
* 可以没有www开头(或其他二级域名)，仅域名
* 网页地址中允许出现/%*?@&等其他允许的符

```js
function isURL(str) {
    return /^(https:\/\/|http:\/\/|ftp:\/\/|rtsp:\/\/|mms:\/\/)?[a-zA-Z0-9_-]+(\.[a-zA-Z0-9_-]+)+[\/=\?%\-&_~`@[\]\':+!]*([^<>\"\"])*$/.test(str);
}
```

#### **80 校验是否为不含端口号的IP地址**

校验规则：

IP格式为xxx.xxx.xxx.xxx，每一项数字取值范围为0-255

除0以外其他数字不能以0开头，比如02

```js
function isIP(str) {
    return /^((25[0-5]|2[0-4][0-9]|1[0-9]{2}|[1-9]?[0-9])\.){3}(25[0-5]|2[0-4][0-9]|1[0-9]{2}|[1-9]?[0-9])$/.test(str);
}
```

#### **81 校验是否为IPv6地址**

校验规则：

支持IPv6正常格式

支持IPv6压缩格式

```js
function isIPv6(str) {
    return Boolean(str.match(/:/g) ? str.match(/:/g).length <= 7 : false && /::/.test(str) ? /^([\da-f]{1,4}(:|::)){1,6}[\da-f]{1,4}$/i.test(str) : /^([\da-f]{1,4}:){7}[\da-f]{1,4}$/i.test(str));
}
```

#### **82 校验是否为中国大陆第二代居民身份证**

校验规则：

共18位，最后一位可为X(大小写均可)

不能以0开头

出生年月日会进行校验：年份只能为18/19/2*开头，月份只能为01-12，日只能为01-31

```js
function isIDCard(str) {
    return /^[1-9][0-9]{5}(18|19|(2[0-9]))[0-9]{2}((0[1-9])|(10|11|12))(([0-2][1-9])|10|20|30|31)[0-9]{3}[0-9Xx]$/.test(str);
}
```

#### **83 校验是否为中国大陆邮政编码**

参数value为数字或字符串

校验规则：

* 共6位，且不能以0开头

```js
function isPostCode(value) {
    return /^[1-9][0-9]{5}$/.test(value.toString());
}
```

#### **84 校验两个参数是否完全相同，包括类型**

校验规则：

* 值相同，数据类型也相同

```js
function same(firstValue, secondValue) {
    return firstValue === secondValue;
}
```

#### **85 校验字符的长度是否在规定的范围内**

校验规则：

* minInt为在取值范围中最小的长度
* maxInt为在取值范围中最大的长度

```js
function lengthRange(str, minLength, maxLength = 9007199254740991) {
    return Boolean(str.length >= minLength && str.length <= maxLength);
}
```

#### **86 校验字符是否以字母开头**

校验规则：

* 必须以字母开头
* 开头的字母不区分大小写

```js
function letterBegin(str) {
    return /^[A-z]/.test(str);
}
```

#### **87 校验字符是否为纯数字(整数)**

校验规则：

* 字符全部为正整数(包含0)
* 可以以0开头

```js
function pureNum(str) {
    return /^[0-9]*$/.test(str);
}

function anysicPunctuation(str) {
    if (!str) return null;
    let arr = str.split('').map(item => {
        return item = '\\' + item;
    });
    return arr.join('|');
}

function getPunctuation(str) {
    return anysicPunctuation(str) || '\\~|\\`|\\!|\\@|\\#|\\$|\\%|\\^|\\&|\\*|\\(|\\)|\\-|\\_|\\+|\\=|\\||\\\|\\[|\\]|\\{|\\}|\\;|\\:|\\"|\\\'|\\,|\\<|\\.|\\>|\\/|\\?';
}

function getExcludePunctuation(str) {
    let regexp = new RegExp(`[${anysicPunctuation(str)}]`, 'g');
    return getPunctuation(' ~`!@#$%^&*()-_+=\[]{};:"\',<.>/?'.replace(regexp, ''));
}
```

#### **88 返回字符串构成种类(字母，数字，标点符号)的数量**

LIP缩写的由来：L(letter 字母) + I(uint 数字) + P(punctuation 标点符号)

参数punctuation的说明：

* punctuation指可接受的标点符号集
* 若需自定义符号集，例如“仅包含中划线和下划线”，将参数设置为"-_"即可
* 若不传值或默认为null，则内部默认标点符号集为除空格外的其他英文标点符号：~`!@#$%^&*()-_+=[]{}; :"', <.>/?

```js
function getLIPTypes(str, punctuation = null) {
    let p_regexp = new RegExp('[' + getPunctuation(punctuation) + ']');
    return /[A-z]/.test(str) + /[0-9]/.test(str) + p_regexp.test(str);
}
```

#### **89 校验字符串构成的种类数量是否大于或等于参数num的值。**

通常用来校验用户设置的密码复杂程度。

校验规则：

* 参数num为需要构成的种类(字母、数字、标点符号)，该值只能是1-3。
* 默认参数num的值为1，即表示：至少包含字母，数字，标点符号中的1种
* 若参数num的值为2，即表示：至少包含字母，数字，标点符号中的2种
* 若参数num的值为3，即表示：必须同时包含字母，数字，标点符号
* 参数punctuation指可接受的标点符号集，具体设定可参考getLIPTypes()方法中关于标点符号集的解释。

```js
function pureLIP(str, num = 1, punctuation = null) {
    let regexp = new RegExp(`[^A-z0-9|${getPunctuation(punctuation)}]`);
    return Boolean(!regexp.test(str) && getLIPTypes(str, punctuation) >= num);
}
```

#### **90 清除所有空格**

```js
function clearSpaces(str) {
    return str.replace(/[ ]/g, '');
}
```

#### **91 清除所有中文字符(包括中文标点符号)**

```js
function clearCNChars(str) {
    return str.replace(/[\u4e00-\u9fa5]/g, '');
}
```

#### **92 清除所有中文字符及空格**

```js
function clearCNCharsAndSpaces(str) {
    return str.replace(/[\u4e00-\u9fa5 ]/g, '');
}
```

#### **93 除保留标点符号集以外，清除其他所有英文的标点符号(含空格)**

全部英文标点符号为：~`!@#$%^&*()-_+=[]{}; :"', <.>/?

参数excludePunctuation指需要保留的标点符号集，例如若传递的值为'_'，即表示清除_以外的其他所有英文标点符号。

```js
function clearPunctuation(str, excludePunctuation = null) {
    let regexp = new RegExp(`[${getExcludePunctuation(excludePunctuation)}]`, 'g');
    return str.replace(regexp, '');
}
```

#### **94 校验是否包含空格**

```js
function haveSpace(str) {
    return /[ ]/.test(str);
}
```

#### **95 校验是否包含中文字符(包括中文标点符号)**

```js
function haveCNChars(str) {
    return /[\u4e00-\u9fa5]/.test(str);
}
```

#### 96 检查日期是否有效

该方法用于检测给出的日期是否有效：

```javascript
const isDateValid = (...val) => !Number.isNaN(new Date(...val).valueOf());

isDateValid("December 17, 1995 03:24:00"); // true
```

#### 97 计算两个日期之间的间隔

该方法用于计算两个日期之间的间隔时间：

```javascript
const dayDif = (date1, date2) => Math.ceil(Math.abs(date1.getTime() - date2.getTime()) / 86400000)

dayDif(new Date("2021-11-3"), new Date("2022-2-1")) // 90
```

#### 98 查找日期位于一年中的第几天

该方法用于检测给出的日期位于今年的第几天：

```javascript
const dayOfYear = (date) => Math.floor((date - new Date(date.getFullYear(), 0, 0)) / 1000 / 60 / 60 / 24);

dayOfYear(new Date()); // 307
复制代码
```

2021年已经过去300多天了~

#### 99 时间格式化

该方法可以用于将时间转化为hour:minutes:seconds的格式：

```javascript
const timeFromDate = date => date.toTimeString().slice(0, 8);

timeFromDate(new Date(2021, 11, 2, 12, 30, 0)); // 12:30:00
timeFromDate(new Date()); // 返回当前时间 09:00:00
```

#### 100 字符串首字母大写

该方法用于将英文字符串的首字母大写处理：

```javascript
const capitalize = str => str.charAt(0).toUpperCase() + str.slice(1)

capitalize("hello world") // Hello world
```

#### 101 翻转字符串

该方法用于将一个字符串进行翻转操作，返回翻转后的字符串：

```javascript
const reverse = str => str.split('').reverse().join('');

reverse('hello world'); // 'dlrow olleh'
```

#### 102 随机字符串

该方法用于生成一个随机的字符串：

```javascript
const randomString = () => Math.random().toString(36).slice(2);

randomString();
```

#### 103 截断字符串

该方法可以从指定长度处截断字符串:

```javascript
const truncateString = (string, length) => string.length < length ? string : `${string.slice(0, length - 3)}...`;

truncateString('Hi, I should be truncated because I am too loooong!', 36)
// 'Hi, I should be truncated because...'
```

#### 104 去除字符串中的HTML

该方法用于去除字符串中的HTML元素：

```javascript
const stripHtml = html => (new DOMParser().parseFromString(html, 'text/html')).body.textContent || '';
```

#### 105 从数组中移除重复项

该方法用于移除数组中的重复项：

```javascript
const removeDuplicates = (arr) => [...new Set(arr)];

console.log(removeDuplicates([1, 2, 2, 3, 3, 4, 4, 5, 5, 6]));
```

#### 106 判断数组是否为空

该方法用于判断一个数组是否为空数组，它将返回一个布尔值：

```javascript
const isNotEmpty = arr => Array.isArray(arr) && arr.length > 0;

isNotEmpty([1, 2, 3]); // true
```

#### 107 合并两个数组

可以使用下面两个方法来合并两个数组：

```javascript
const merge = (a, b) => a.concat(b);

const merge = (a, b) => [...a, ...b];
```

#### 108 判断一个数是奇数还是偶数

该方法用于判断一个数字是奇数还是偶数：

```javascript
const isEven = num => num % 2 === 0;

isEven(996);
```

#### 109 获得一组数的平均值

```javascript
const average = (...args) => args.reduce((a, b) => a + b) / args.length;

average(1, 2, 3, 4, 5); // 3
```

#### 110 获取两个整数之间的随机整数

该方法用于获取两个整数之间的随机整数

```javascript
const random = (min, max) => Math.floor(Math.random() * (max - min + 1) + min);

random(1, 50);
```

#### 111 指定位数四舍五入

该方法用于将一个数字按照指定位进行四舍五入：

```javascript
const round = (n, d) => Number(Math.round(n + "e" + d) + "e-" + d)

round(1.005, 2) //1.01
round(1.555, 2) //1.56
```

#### 112 将RGB转化为十六机制

该方法可以将一个RGB的颜色值转化为16进制值：

```javascript
const rgbToHex = (r, g, b) => "#" + ((1 << 24) + (r << 16) + (g << 8) + b).toString(16).slice(1);

rgbToHex(255, 255, 255); // '#ffffff'
```

#### 114 获取随机十六进制颜色

该方法用于获取一个随机的十六进制颜色值：

```javascript
const randomHex = () => `#${Math.floor(Math.random() * 0xffffff).toString(16).padEnd(6, "0")}`;

randomHex();
```

#### 115 复制内容到剪切板

该方法使用 navigator.clipboard.writeText 来实现将文本复制到剪贴板：

```javascript
const copyToClipboard = (text) => navigator.clipboard.writeText(text);

copyToClipboard("Hello World");
```

#### 116 清除所有cookie

该方法可以通过使用 document.cookie 来访问 cookie 并清除存储在网页中的所有 cookie：

```javascript
const clearCookies = document.cookie.split(';').forEach(cookie => document.cookie = cookie.replace(/^ +/, '').replace(/=.*/, `=;expires=${new Date(0).toUTCString()};path=/`));
```

#### 117 获取选中的文本

该方法通过内置的 getSelection 属性获取用户选择的文本：

```javascript
const getSelectedText = () => window.getSelection().toString();

getSelectedText();
```

#### 118 检测是否是黑暗模式

该方法用于检测当前的环境是否是黑暗模式，它是一个布尔值：

```javascript
const isDarkMode = window.matchMedia && window.matchMedia('(prefers-color-scheme: dark)').matches

console.log(isDarkMode)
```

#### 119 滚动到页面顶部

该方法用于在页面中返回顶部：

```javascript
const goToTop = () => window.scrollTo(0, 0);

goToTop();
```

#### 120 判断当前标签页是否激活

该方法用于检测当前标签页是否已经激活：

```javascript
const isTabInView = () => !document.hidden;
```

#### 121 判断当前是否是苹果设备

该方法用于检测当前的设备是否是苹果的设备：

```javascript
const isAppleDevice = () => /Mac|iPod|iPhone|iPad/.test(navigator.platform);

isAppleDevice();
```

#### 122 是否滚动到页面底部

该方法用于判断页面是否已经底部：

```javascript
const scrolledToBottom = () => document.documentElement.clientHeight + window.scrollY >= document.documentElement.scrollHeight;
```

#### 123 重定向到一个URL

该方法用于重定向到一个新的URL：

```javascript
const redirect = url => location.href = url

redirect("https://www.google.com/")
```

#### 124 打开浏览器打印框

该方法用于打开浏览器的打印框：

```javascript
const showPrintDialog = () => window.print()
```

#### 125 随机布尔值

该方法可以返回一个随机的布尔值，使用Math.random()可以获得0-1的随机数，与0.5进行比较，就有一半的概率获得真值或者假值。

```javascript
const randomBoolean = () => Math.random() >= 0.5;

randomBoolean();
```

#### 126 变量交换

可以使用以下形式在不适用第三个变量的情况下，交换两个变量的值：

```javascript
[foo, bar] = [bar, foo];
```

#### 127. 获取变量的类型

该方法用于获取一个变量的类型：

```javascript
const trueTypeOf = (obj) => Object.prototype.toString.call(obj).slice(8, -1).toLowerCase();

trueTypeOf(''); // string
trueTypeOf(0); // number
trueTypeOf(); // undefined
trueTypeOf(null); // null
trueTypeOf({}); // object
trueTypeOf([]); // array
trueTypeOf(0); // number
trueTypeOf(() => {}); // function
```

#### 128 华氏度和摄氏度之间的转化

该方法用于摄氏度和华氏度之间的转化：

```javascript
const celsiusToFahrenheit = (celsius) => celsius * 9 / 5 + 32;
const fahrenheitToCelsius = (fahrenheit) => (fahrenheit - 32) * 5 / 9;

celsiusToFahrenheit(15); // 59
celsiusToFahrenheit(0); // 32
celsiusToFahrenheit(-20); // -4
fahrenheitToCelsius(59); // 15
fahrenheitToCelsius(32); // 0
```

#### 129 检测对象是否为空

该方法用于检测一个JavaScript对象是否为空：

```javascript
const isEmpty = obj => Reflect.ownKeys(obj).length === 0 && obj.constructor === Object;
```

#### 130 隐藏所有指定的元素

```js
const hide2 = (el) => Array.from(el).forEach(e => (e.style.display = 'none'));

// 事例:隐藏页面上所有`<img>`元素?
hide(document.querySelectorAll('img'))
```

#### 131 检查元素是否具有指定的类

页面DOM里的每个节点上都有一个classList对象，程序员可以使用里面的方法新增、删除、修改节点上的CSS类。使用classList，程序员还可以用它来判断某个节点是否被赋予了某个CSS类。

```js
const hasClass = (el, className) => el.classList.contains(className)

// 事例
hasClass(document.querySelector('p.special'), 'special') // true
```

#### 132 切换元素的类

```js
const toggleClass = (el, className) => el.classList.toggle(className)

// 事例 移除 p 具有类`special`的 special 类
toggleClass(document.querySelector('p.special'), 'special')
```

#### 133 检查父元素是否包含子元素

```js
const elementContains = (parent, child) = parent !== child && parent.contains(child);

// 事例
elementContains(document.querySelector('head'), document.querySelector('title'));
// true
elementContains(document.querySelector('body'), document.querySelector('body'));
// false
```

#### 134 检查指定的元素在视口中是否可见

```js
const elementIsVisibleInViewport = (el, partiallyVisible = false) => {
    const {
        top,
        left,
        bottom,
        right
    } = el.getBoundingClientRect();
    const {
        innerHeight,
        innerWidth
    } = window;
    return partiallyVisible ?
        ((top > 0 && top < innerHeight) || (bottom > 0 && bottom < innerHeight)) &&
        ((left > 0 && left < innerWidth) || (right > 0 && right < innerWidth)) :
        top >= 0 && left >= 0 && bottom <= innerHeight && right <= innerWidth;
};

// 事例
elementIsVisibleInViewport(el); // 需要左右可见
elementIsVisibleInViewport(el, true); // 需要全屏(上下左右)可以见
```

#### 135 获取元素中的所有图像

```js
const getImages = (el, includeDuplicates = false) => {
    const images = [...el.getElementsByTagName('img')].map(img => img.getAttribute('src'));
    return includeDuplicates ? images : [...new Set(images)];
};

// 事例：includeDuplicates 为 true 表示需要排除重复元素
getImages(document, true); // ['image1.jpg', 'image2.png', 'image1.png', '...']
getImages(document, false); // ['image1.jpg', 'image2.png', '...']
```

#### 136 获取当前URL

```js
const currentURL = () => window.location.href

// 事例
currentURL() // 'https://google.com'
```

#### 137 创建一个包含当前URL参数的对象

```js
const getURLParameters = url =>
    (url.match(/([^?=&]+)(=([^&]*))/g) || []).reduce(
        (a, v) => ((a[v.slice(0, v.indexOf('='))] = v.slice(v.indexOf('=') + 1)), a), {}
    );

// 事例
getURLParameters('http://url.com/page?n=Adam&s=Smith'); // {n: 'Adam', s: 'Smith'}
getURLParameters('google.com'); // {}
```

#### 138 向传递的URL发出GET请求

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

#### 139 向传递的URL发出POST请求

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

#### 140 将一组表单元素转化为对象

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

#### 141 从对象检索给定选择器指示的一组属性

```js
const get = (from, ...selectors) => [...selectors].map(s =>
    s
    .replace(/\[([^\[\]]*)\]/g, '.$1.')
    .split('.')
    .filter(t => t !== '')
    .reduce((prev, cur) => prev && prev[cur], from)
);
const obj = {
    selector: {
        to: {
            val: 'val to select'
        }
    },
    target: [1, 2, {
        a: 'test'
    }]
};

// Example
get(obj, 'selector.to.val', 'target[0]', 'target[2].a');
// ['val to select', 1, 'test']
```

#### 142 在等待指定时间后调用提供的函数

```js
const delay = (fn, wait, ...args) => setTimeout(fn, wait, ...args);
delay(
    function(text) {
        console.log(text);
    },
    1000,
    'later'
);

// 1秒后打印 'later'
```

#### 143 在给定元素上触发特定事件且能选择地传递自定义数据

```js
const triggerEvent = (el, eventType, detail) =>
    el.dispatchEvent(new CustomEvent(eventType, {
        detail
    }));

// 事例
triggerEvent(document.getElementById('myId'), 'click');
triggerEvent(document.getElementById('myId'), 'click', {
    username: 'bob'
});
```

自定义事件的函数有 Event、CustomEvent 和 dispatchEvent

```js
// 向 window派发一个resize内置事件
window.dispatchEvent(new Event('resize'))

// 直接自定义事件，使用 Event 构造函数：
var event = new Event('build');
var elem = document.querySelector('#id')
// 监听事件
elem.addEventListener('build', function(e) {
    ...
}, false);
// 触发事件.
elem.dispatchEvent(event);
```

CustomEvent 可以创建一个更高度自定义事件，还可以附带一些数据，具体用法如下：

```js
var myEvent = new CustomEvent(eventname, options);
其中 options 可以是： {
    detail: {
        ...
    },
    bubbles: true, //是否冒泡
    cancelable: false //是否取消默认事件
}
```

其中 detail 可以存放一些初始化的信息，可以在触发的时候调用。其他属性就是定义该事件是否具有冒泡等等功能。

内置的事件会由浏览器根据某些操作进行触发，自定义的事件就需要人工触发。 dispatchEvent 函数就是用来触发某个事件：

```js
element.dispatchEvent(customEvent);
```

上面代码表示，在 element 上面触发 customEvent 这个事件。

```js
// add an appropriate event listener
obj.addEventListener("cat", function(e) {
    process(e.detail)
});

// create and dispatch the event
var event = new CustomEvent("cat", {
    "detail": {
        "hazcheeseburger": true
    }
});
obj.dispatchEvent(event);
使用自定义事件需要注意兼容性问题， 而使用 jQuery 就简单多了：

// 绑定自定义事件
$(element).on('myCustomEvent', function() {});

// 触发事件
$(element).trigger('myCustomEvent');
// 此外，你还可以在触发自定义事件时传递更多参数信息：

$("p").on("myCustomEvent", function(event, myName) {
    $(this).text(myName + ", hi there!");
});
$("button").click(function() {
    $("p").trigger("myCustomEvent", ["John"]);
});
```

#### 144 从元素中移除事件监听器

```js
const off = (el, evt, fn, opts = false) => el.removeEventListener(evt, fn, opts);

const fn = () => console.log('!');
document.body.addEventListener('click', fn);
off(document.body, 'click', fn);
```

#### 145 获得给定毫秒数的可读格式

```js
const formatDuration = ms => {
    if (ms < 0) ms = -ms;
    const time = {
        day: Math.floor(ms / 86400000),
        hour: Math.floor(ms / 3600000) % 24,
        minute: Math.floor(ms / 60000) % 60,
        second: Math.floor(ms / 1000) % 60,
        millisecond: Math.floor(ms) % 1000
    };
    return Object.entries(time)
        .filter(val => val[1] !== 0)
        .map(([key, val]) => `${val} ${key}${val !== 1 ? 's' : ''}`)
        .join(', ');
};

// 事例
formatDuration(1001); // '1 second, 1 millisecond'
formatDuration(34325055574);
// '397 days, 6 hours, 44 minutes, 15 seconds, 574 milliseconds'
```

#### 146 如何获得两个日期之间的差异（以天为单位）

```js
const getDaysDiffBetweenDates = (dateInitial, dateFinal) =>
    (dateFinal - dateInitial) / (1000 * 3600 * 24);

// 事例
getDaysDiffBetweenDates(new Date('2017-12-13'), new Date('2017-12-22')); // 9
```

#### 147 为指定选择器创建具有指定范围，步长和持续时间的计数器

```js
const counter = (selector, start, end, step = 1, duration = 2000) => {
    let current = start,
        _step = (end - start) * step < 0 ? -step : step,
        timer = setInterval(() => {
            current += _step;
            document.querySelector(selector).innerHTML = current;
            if (current >= end) document.querySelector(selector).innerHTML = end;
            if (current >= end) clearInterval(timer);
        }, Math.abs(Math.floor(duration / (end - start))));
    return timer;
};

// 事例
counter('#my-id', 1, 1000, 5, 2000);
// 让 `id=“my-id”`的元素创建一个2秒计时器
```

#### 148 获取当前页面的滚动位置

```js
const getScrollPosition = (el = window) => ({
    x: el.pageXOffset !== undefined ? el.pageXOffset : el.scrollLeft,
    y: el.pageYOffset !== undefined ? el.pageYOffset : el.scrollTop
});

// 事例
getScrollPosition(); // {x: 0, y: 200}
```

#### 149 平滑滚动到页面顶部

```js
const scrollToTop = () => {
    const c = document.documentElement.scrollTop || document.body.scrollTop;
    if (c > 0) {
        window.requestAnimationFrame(scrollToTop);
        window.scrollTo(0, c - c / 8);
    }
}

// 事例
scrollToTop()
```

window.requestAnimationFrame() 告诉浏览器——你希望执行一个动画，并且要求浏览器在下次重绘之前调用指定的回调函数更新动画。该方法需要传入一个回调函数作为参数，该回调函数会在浏览器下一次重绘之前执行。

requestAnimationFrame：优势：由系统决定回调函数的执行时机。60Hz的刷新频率，那么每次刷新的间隔中会执行一次回调函数，不会引起丢帧，不会卡顿。

#### 150 将字符串复制到剪贴板

```js
  const el = document.createElement('textarea');
  el.value = str;
  el.setAttribute('readonly', '');
  el.style.position = 'absolute';
  el.style.left = '-9999px';
  document.body.appendChild(el);
  const selected =
      document.getSelection().rangeCount > 0 ? document.getSelection().getRangeAt(0) : false;
  el.select();
  document.execCommand('copy');
  document.body.removeChild(el);
  if (selected) {
      document.getSelection().removeAllRanges();
      document.getSelection().addRange(selected);
  }
  };

  // 事例
  copyToClipboard('Lorem ipsum');
  // 'Lorem ipsum' copied to clipboard
```

#### 151 判断页面的浏览器选项卡是否聚焦

```js
const isBrowserTabFocused = () => !document.hidden;

// 事例
isBrowserTabFocused(); // true
```

#### 152 判断设备是移动设备还是台式机/笔记本电脑？

```js
const detectDeviceType = () =>
    /Android|webOS|iPhone|iPad|iPod|BlackBerry|IEMobile|Opera Mini/i.test(navigator.userAgent) ?
    'Mobile' :
    'Desktop';

// 事例
detectDeviceType(); // "Mobile" or "Desktop"
```

#### 153 如何创建目录（如果不存在）[Node服务端]

```js
const fs = require('fs');
const createDirIfNotExists = dir => (!fs.existsSync(dir) ? fs.mkdirSync(dir) : undefined);

// 事例
createDirIfNotExists('test');
```

## 代码片段

#### 1、all

如果数组所有元素满足函数条件，则返回true。调用时，如果省略第二个参数，则默认传递布尔值。

```js
const all = (arr, fn = Boolean) => arr.every(fn);

all([4, 2, 3], x => x > 1); // true
all([1, 2, 3]); // true
```

#### 2、allEqual

判断数组中的元素是否都相等

```js
const allEqual = arr => arr.every(val => val === arr[0]);

allEqual([1, 2, 3, 4, 5, 6]); // false
allEqual([1, 1, 1, 1]); // true
```

#### 3、approximatelyEqual

此代码示例检查两个数字是否近似相等，差异值可以通过传参的形式进行设置

```js
const approximatelyEqual = (v1, v2, epsilon = 0.001) => Math.abs(v1 - v2) < epsilon;

approximatelyEqual(Math.PI / 2.0, 1.5708); // true
```

#### 4、arrayToCSV

此段代码将没有逗号或双引号的元素转换成带有逗号分隔符的字符串即CSV格式识别的形式。

```js
const arrayToCSV = (arr, delimiter = ',') =>
    arr.map(v => v.map(x => `"${x}"`).join(delimiter)).join('\n');

arrayToCSV([
    ['a', 'b'],
    ['c', 'd']
]); // '"a","b"\n"c","d"'
arrayToCSV([
    ['a', 'b'],
    ['c', 'd']
], ';'); // '"a";"b"\n"c";"d"'
```

#### 5、arrayToHtmlList

此段代码将数组元素转换成\<li>标记，并将此元素添加至给定的ID元素标记内。

```js
const arrayToHtmlList = (arr, listID) =>
    (el => (
        (el = document.querySelector('#' + listID)),
        (el.innerHTML += arr.map(item => `<li>${item}</li>`).join(''))
    ))();

arrayToHtmlList(['item 1', 'item 2'], 'myListID');
```

#### 6、attempt

此段代码执行一个函数，将剩余的参数传回函数当参数，返回相应的结果，并能捕获异常。

```js
const attempt = (fn, ...args) => {
    try {
        return fn(...args);
    } catch (e) {
        return e instanceof Error ? e : new Error(e);
    }
};
var elements = attempt(function(selector) {
    return document.querySelectorAll(selector);
}, '>_>');
if (elements instanceof Error) elements = []; // elements = []
```

#### 7、average

此段代码返回两个或多个数的平均数。

```js
const average = (...nums) => nums.reduce((acc, val) => acc + val, 0) / nums.length;
average(...[1, 2, 3]); // 2
average(1, 2, 3); // 2
```

#### 8、averageBy

一个 map()函数和 reduce()函数结合的例子，此函数先通过 map() 函数将对象转换成数组，然后在调用reduce()函数进行累加，然后根据数组长度返回平均值。

```js
const averageBy = (arr, fn) =>
    arr.map(typeof fn === 'function' ? fn : val => val[fn]).reduce((acc, val) => acc + val, 0) /
    arr.length;

averageBy([{
    n: 4
}, {
    n: 2
}, {
    n: 8
}, {
    n: 6
}], o => o.n); // 5
averageBy([{
    n: 4
}, {
    n: 2
}, {
    n: 8
}, {
    n: 6
}], 'n'); // 5
```

#### 9、bifurcate

此函数包含两个参数，类型都为数组，依据第二个参数的真假条件，将一个参数的数组进行分组，条件为真的放入第一个数组，其它的放入第二个数组。这里运用了Array.prototype.reduce() 和 Array.prototype.push() 相结合的形式。

```js
const bifurcate = (arr, filter) =>
    arr.reduce((acc, val, i) => (acc[filter[i] ? 0 : 1].push(val), acc), [
        [],
        []
    ]);
bifurcate(['beep', 'boop', 'foo', 'bar'], [true, true, false, true]);
// [ ['beep', 'boop', 'bar'], ['foo'] ]
```

#### 10、bifurcateBy

此段代码将数组按照指定的函数逻辑进行分组，满足函数条件的逻辑为真，放入第一个数组中，其它不满足的放入第二个数组 。这里运用了Array.prototype.reduce() 和 Array.prototype.push() 相结合的形式，基于函数过滤逻辑，通过 Array.prototype.push() 函数将其添加到数组中。

```js
const bifurcateBy = (arr, fn) =>
    arr.reduce((acc, val, i) => (acc[fn(val, i) ? 0 : 1].push(val), acc), [
        [],
        []
    ]);

bifurcateBy(['beep', 'boop', 'foo', 'bar'], x => x[0] === 'b');
// [ ['beep', 'boop', 'bar'], ['foo'] ]
```

#### 11、bottomVisible

用于检测页面是否滚动到页面底部。

```js
const bottomVisible = () =>
    document.documentElement.clientHeight + window.scrollY >=
    (document.documentElement.scrollHeight || document.documentElement.clientHeight);

bottomVisible(); // true
```

#### 12、byteSize

此代码返回字符串的字节长度。这里用到了Blob对象，Blob（Binary Large Object）对象代表了一段二进制数据，提供了一系列操作接口。其他操作二进制数据的API（比如File对象），都是建立在Blob对象基础上的，继承了它的属性和方法。生成Blob对象有两种方法：一种是使用Blob构造函数，另一种是对现有的Blob对象使用slice方法切出一部分。

```js
const byteSize = str => new Blob([str]).size;

byteSize(''); // 4
byteSize('Hello World'); // 11
```

#### 13、capitalize

将字符串的首字母转成大写, 这里主要运用到了ES6的展开语法在数组中的运用。

```js
const capitalize = ([first, ...rest]) =>
    first.toUpperCase() + rest.join('');

capitalize('fooBar'); // 'FooBar'
capitalize('fooBar', true); // 'FooBar'
```

#### 14、capitalizeEveryWord

将一个句子中每个单词首字母转换成大写字母，这里中要运用了正则表达式进行替换。

```js
const capitalizeEveryWord = str => str.replace(/\b[a-z]/g, char => char.toUpperCase());

capitalizeEveryWord('hello world!'); // 'Hello World!'
```

#### 15、castArray

此段代码将非数值的值转换成数组对象。

```js
const castArray = val => (Array.isArray(val) ? val : [val]);

castArray('foo'); // ['foo']
castArray([1]); // [1]
```

#### 16、compact

将数组中移除值为 false 的内容。

```js
const compact = arr => arr.filter(Boolean);

compact([0, 1, false, 2, '', 3, 'a', 'e' * 23, NaN, 's', 34]);
// [ 1, 2, 3, 'a', 's', 34 ]
```

#### 17、countOccurrences

统计数组中某个值出现的次数

```js
const countOccurrences = (arr, val) => arr.reduce((a, v) => (v === val ? a + 1 : a), 0);
countOccurrences([1, 1, 2, 1, 2, 3], 1); // 3
```

#### 18、Create Directory

此代码段使用 existSync() 检查目录是否存在，然后使用 mkdirSync() 创建目录（如果不存在）。

```js
const fs = require('fs');
const createDirIfNotExists = dir => (!fs.existsSync(dir) ? fs.mkdirSync(dir) : undefined);
createDirIfNotExists('test');
// creates the directory 'test', if it doesn't exist
```

#### 19、currentURL

返回当前访问的 URL 地址。

```js
const currentURL = () => window.location.href;

currentURL(); // 'https://medium.com/@fatosmorina'
```

#### 20、dayOfYear

返回当前是今年的第几天

```js
const dayOfYear = date =>
    Math.floor((date - new Date(date.getFullYear(), 0, 0)) / 1000 / 60 / 60 / 24);

dayOfYear(new Date()); // 272
```

#### 21、decapitalize

将字符串的首字母转换成小写字母

```js
const decapitalize = ([first, ...rest]) =>
    first.toLowerCase() + rest.join('')

decapitalize('FooBar'); // 'fooBar'
```

#### 22、deepFlatten

通过递归的形式，将多维数组展平成一维数组。

```js
const deepFlatten = arr => [].concat(...arr.map(v => (Array.isArray(v) ? deepFlatten(v) : v)));

deepFlatten([1, [2],
    [
        [3], 4
    ], 5
]); // [1,2,3,4,5]
```

#### 23、default

去重对象的属性，如果对象中含有重复的属性，以前面的为准。

```js
const defaults = (obj, ...defs) => Object.assign({}, obj, ...defs.reverse(), obj);

defaults({
    a: 1
}, {
    b: 2
}, {
    b: 6
}, {
    a: 3
}); // { a: 1, b: 2 }
```

#### 24、defer

延迟函数的调用，即异步调用函数。

```js
const defer = (fn, ...args) => setTimeout(fn, 1, ...args);

defer(console.log, 'a'), console.log('b'); // logs 'b' then 'a'
```

#### 25、degreesToRads

此段代码将标准的度数，转换成弧度。

```js
const degreesToRads = deg => (deg * Math.PI) / 180.0;

degreesToRads(90.0); // ~1.5708
```

#### 26、difference

此段代码查找两个给定数组的差异，查找出前者数组在后者数组中不存在元素。

```js
const difference = (a, b) => {
    const s = new Set(b);
    return a.filter(x => !s.has(x));
};

difference([1, 2, 3], [1, 2, 4]); // [3]
```

#### 27、differenceBy

通过给定的函数来处理需要对比差异的数组，查找出前者数组在后者数组中不存在元素。

```js
const differenceBy = (a, b, fn) => {
    const s = new Set(b.map(fn));
    return a.filter(x => !s.has(fn(x)));
};

differenceBy([2.1, 1.2], [2.3, 3.4], Math.floor); // [1.2]
differenceBy([{
    x: 2
}, {
    x: 1
}], [{
    x: 1
}], v => v.x); // [ { x: 2 } ]
```

#### 28、differenceWith

此段代码按照给定函数逻辑筛选需要对比差异的数组，查找出前者数组在后者数组中不存在元素。

```js
const differenceWith = (arr, val, comp) => arr.filter(a => val.findIndex(b => comp(a, b)) === -1);

differenceWith([1, 1.2, 1.5, 3, 0], [1.9, 3, 0], (a, b) => Math.round(a) === Math.round(b));
// [1, 1.2]
```

#### 29、digitize

将输入的数字拆分成单个数字组成的数组。

```js
const digitize = n => [...`${n}`].map(i => parseInt(i));

digitize(431); // [4, 3, 1]
```

#### 30、distance

计算两点之间的距离

```js
const distance = (x0, y0, x1, y1) => Math.hypot(x1 - x0, y1 - y0);

distance(1, 1, 2, 3); // 2.23606797749979
```

#### 31、drop

此段代码将给定的数组从左边开始删除 n 个元素

```js
const drop = (arr, n = 1) => arr.slice(n);

drop([1, 2, 3]); // [2,3]
drop([1, 2, 3], 2); // [3]
drop([1, 2, 3], 42); // []
```

#### 32、dropRight

此段代码将给定的数组从右边开始删除 n 个元素

```js
const dropRight = (arr, n = 1) => arr.slice(0, -n);

dropRight([1, 2, 3]); // [1,2]
dropRight([1, 2, 3], 2); // [1]
dropRight([1, 2, 3], 42); // []
```

#### 33、dropRightWhile

此段代码将给定的数组按照给定的函数条件从右开始删除，直到当前元素满足函数条件为True时，停止删除，并返回数组剩余元素。

```js
const dropRightWhile = (arr, func) => {
    while (arr.length > 0 && !func(arr[arr.length - 1])) arr = arr.slice(0, -1);
    return arr;
};

dropRightWhile([1, 2, 3, 4], n => n < 3); // [1, 2]
```

#### 34、dropWhile

按照给的的函数条件筛选数组，不满足函数条件的将从数组中移除。

```js
const dropWhile = (arr, func) => {
    while (arr.length > 0 && !func(arr[0])) arr = arr.slice(1);
    return arr;
};

dropWhile([1, 2, 3, 4], n => n >= 3); // [3,4]
```

#### 35、elementContains

接收两个DOM元素对象参数，判断后者是否是前者的子元素。

```js
const elementContains = (parent, child) => parent !== child && parent.contains(child);

elementContains(document.querySelector('head'), document.querySelector('title')); // true
elementContains(document.querySelector('body'), document.querySelector('body')); // false
```

#### 36、filterNonUnique

移除数组中重复的元素

```js
const filterNonUnique = arr => […new Set(arr)];
filterNonUnique([1, 2, 2, 3, 4, 4, 5]); // [1, 2, 3, 4, 5]
```

#### 37、findKey

按照给定的函数条件，查找第一个满足条件对象的键值。

```js
const findKey = (obj, fn) => Object.keys(obj).find(key => fn(obj[key], key, obj));

findKey({
        barney: {
            age: 36,
            active: true
        },
        fred: {
            age: 40,
            active: false
        },
        pebbles: {
            age: 1,
            active: true
        }
    },
    o => o['active']
); // 'barney'
```

#### 38、findLast

按照给定的函数条件筛选数组，将最后一个满足条件的元素进行删除。

```js
const findLast = (arr, fn) => arr.filter(fn).pop();

findLast([1, 2, 3, 4], n => n % 2 === 1); // 3
```

#### 39、flatten

按照指定数组的深度，将嵌套数组进行展平。

```js
const flatten = (arr, depth = 1) =>
    arr.reduce((a, v) => a.concat(depth > 1 && Array.isArray(v) ? flatten(v, depth - 1) : v), []);

flatten([1, [2], 3, 4]); // [1, 2, 3, 4]
flatten([1, [2, [3, [4, 5], 6], 7], 8], 2); // [1, 2, 3, [4, 5], 6, 7, 8]
```

#### 40、forEachRight

按照给定的函数条件，从数组的右边往左依次进行执行。

```js
const forEachRight = (arr, callback) =>
    arr
    .slice(0)
    .reverse()
    .forEach(callback);

forEachRight([1, 2, 3, 4], val => console.log(val)); // '4', '3', '2', '1'
```

#### 41、forOwn

此段代码按照给定的函数条件，进行迭代对象。

```js
const forOwn = (obj, fn) => Object.keys(obj).forEach(key => fn(obj[key], key, obj));
forOwn({
    foo: 'bar',
    a: 1
}, v => console.log(v)); // 'bar', 1
```

#### 42、functionName

此段代码输出函数的名称。

```js
const functionName = fn => (console.debug(fn.name), fn);

functionName(Math.max); // max (logged in debug channel of console)
```

#### 43、getColonTimeFromDate

此段代码从Date对象里获取当前时间。

```js
const getColonTimeFromDate = date => date.toTimeString().slice(0, 8);
getColonTimeFromDate(new Date()); // "08:38:00"
```

#### 44、getDaysDiffBetweenDates

此段代码返回两个日期之间相差多少天

```js
const getDaysDiffBetweenDates = (dateInitial, dateFinal) =>
    (dateFinal - dateInitial) / (1000 * 3600 * 24);

getDaysDiffBetweenDates(new Date('2019-01-13'), new Date('2019-01-15')); // 2
```

#### 45、getStyle

此代码返回DOM元素节点对应的属性值。

```js
const getStyle = (el, ruleName) => getComputedStyle(el)[ruleName];

getStyle(document.querySelector('p'), 'font-size'); // '16px'
```

#### 46、getType

此段代码的主要功能就是返回数据的类型。

```js
const getType = v =>
    v === undefined ? 'undefined' : v === null ? 'null' : v.constructor.name.toLowerCase();

getType(new Set([1, 2, 3])); // 'set'
```

#### 47、hasClass

此段代码返回DOM元素是否包含指定的Class样式。

```js
const hasClass = (el, className) => el.classList.contains(className);
hasClass(document.querySelector('p.special'), 'special'); // true
```

#### 48、head

此段代码输出数组的第一个元素。

```js
const head = arr => arr[0];

head([1, 2, 3]); // 1
```

#### 49、hide

此段代码隐藏指定的DOM元素。

```js
const hide = (...el) => [...el].forEach(e => (e.style.display = 'none'));

hide(document.querySelectorAll('img')); // Hides all <img> elements on the page
```

#### 50、httpsRedirect

此段代码的功能就是将http网址重定向https网址。

```js
const httpsRedirect = () => {
    if (location.protocol !== 'https:') location.replace('https://' + location.href.split('//')[1]);
};

httpsRedirect(); // If you are on http://mydomain.com, you are redirected to https://mydomain.com
```

#### 51、indexOfAll

此代码可以返回数组中某个值对应的所有索引值，如果不包含该值，则返回一个空数组。

```js
const indexOfAll = (arr, val) => arr.reduce((acc, el, i) => (el === val ? [...acc, i] : acc), []);

indexOfAll([1, 2, 3, 1, 2, 3], 1); // [0,3]
indexOfAll([1, 2, 3], 4); // []
```

#### 52、initial

此段代码返回数组中除最后一个元素的所有元素

```js
const initial = arr => arr.slice(0, -1);

initial([1, 2, 3]); // [1,2]const initial = arr => arr.slice(0, -1);
initial([1, 2, 3]); // [1,2]
```

#### 53、insertAfter

此段代码的功能主要是在给定的DOM节点后插入新的节点内容

```js
const insertAfter = (el, htmlString) => el.insertAdjacentHTML('afterend', htmlString);

insertAfter(document.getElementById('myId'), '<p>after</p>'); // <div id="myId">...</div> <p>after</p>
```

#### 54、insertBefore

此段代码的功能主要是在给定的DOM节点前插入新的节点内容

```js
const insertBefore = (el, htmlString) => el.insertAdjacentHTML('beforebegin', htmlString);

insertBefore(document.getElementById('myId'), '<p>before</p>'); // <p>before</p> <div id="myId">...</div>
```

#### 55、intersection

此段代码返回两个数组元素之间的交集。

```js
const intersection = (a, b) => {
    const s = new Set(b);
    return a.filter(x => s.has(x));
};

intersection([1, 2, 3], [4, 3, 2]); // [2, 3]
```

#### 56、intersectionBy

按照给定的函数处理需要对比的数组元素，然后根据处理后的数组，找出交集，最后从第一个数组中将对应的元素输出。

```js
const intersectionBy = (a, b, fn) => {
    const s = new Set(b.map(fn));
    return a.filter(x => s.has(fn(x)));
};

intersectionBy([2.1, 1.2], [2.3, 3.4], Math.floor); // [2.1]
```

#### 57、intersectionBy

按照给定的函数对比两个数组的差异，然后找出交集，最后从第一个数组中将对应的元素输出。

```js
const intersectionWith = (a, b, comp) => a.filter(x => b.findIndex(y => comp(x, y)) !== -1);

intersectionWith([1, 1.2, 1.5, 3, 0], [1.9, 3, 0, 3.9], (a, b) => Math.round(a) === Math.round(b)); // [1.5, 3, 0]
```

#### 58、is

此段代码用于判断数据是否为指定的数据类型，如果是则返回true。

```js
const is = (type, val) => ![, null].includes(val) && val.constructor === type;

is(Array, [1]); // true
is(ArrayBuffer, new ArrayBuffer()); // true
is(Map, new Map()); // true
is(RegExp, /./g); // true
is(Set, new Set()); // true
is(WeakMap, new WeakMap()); // true
is(WeakSet, new WeakSet()); // true
is(String, ''); // true
is(String, new String('')); // true
is(Number, 1); // true
is(Number, new Number(1)); // true
is(Boolean, true); // true
is(Boolean, new Boolean(true)); // true
```

#### 59、isAfterDate

接收两个日期类型的参数，判断前者的日期是否晚于后者的日期。

```js
const isAfterDate = (dateA, dateB) => dateA > dateB;

isAfterDate(new Date(2010, 10, 21), new Date(2010, 10, 20)); // true
```

#### 60、isAnagram

用于检测两个单词是否相似。

```js
const isAnagram = (str1, str2) => {
    const normalize = str =>
        str
        .toLowerCase()
        .replace(/[^a-z0-9]/gi, '')
        .split('')
        .sort()
        .join('');
    return normalize(str1) === normalize(str2);
};

isAnagram('iceman', 'cinema'); // true
```

#### 61、isArrayLike

此段代码用于检测对象是否为类数组对象, 是否可迭代。

```js
const isArrayLike = obj => obj != null && typeof obj[Symbol.iterator] === 'function';

isArrayLike(document.querySelectorAll('.className')); // true
isArrayLike('abc'); // true
isArrayLike(null); // false
```

#### 62、isBeforeDate

接收两个日期类型的参数，判断前者的日期是否早于后者的日期。

```js
const isBeforeDate = (dateA, dateB) => dateA < dateB;

isBeforeDate(new Date(2010, 10, 20), new Date(2010, 10, 21)); // true
```

#### 63、isBoolean

此段代码用于检查参数是否为布尔类型。

```js
const isBoolean = val => typeof val === 'boolean';

isBoolean(null); // false
isBoolean(false); // true
```

#### 64、getColonTimeFromDate

用于判断程序运行环境是否在浏览器，这有助于避免在node环境运行前端模块时出错。

```js
const isBrowser = () => ![typeof window, typeof document].includes('undefined');

isBrowser(); // true (browser)
isBrowser(); // false (Node)
```

#### 65、isBrowserTabFocused

用于判断当前页面是否处于活动状态（显示状态）。

```js
const isBrowserTabFocused = () => !document.hidden;
isBrowserTabFocused(); // true
```

#### 66、isLowerCase

用于判断当前字符串是否都为小写。

```js
const isLowerCase = str => str === str.toLowerCase();

isLowerCase('abc'); // true
isLowerCase('a3@$'); // true
isLowerCase('Ab4'); // false
```

#### 67、isNil

用于判断当前变量的值是否为 null 或 undefined 类型。

```js
const isNil = val => val === undefined || val === null;

isNil(null); // true
isNil(undefined); // true
```

#### 68、isNull

用于判断当前变量的值是否为 null 类型

```js
const isNull = val => val === null;

isNull(null); // true
```

#### 69、isNumber

用于检查当前的值是否为数字类型

```js
function isNumber(n) {
    return !isNaN(parseFloat(n)) && isFinite(n);
}

isNumber('1'); // false
isNumber(1); // true
```

#### 70、isObject

用于判断参数的值是否是对象，这里运用了Object 构造函数创建一个对象包装器，如果是对象类型，将会原值返回。

```js
const isObject = obj => obj === Object(obj);

isObject([1, 2, 3, 4]); // true
isObject([]); // true
isObject(['Hello!']); // true
isObject({
    a: 1
}); // true
isObject({}); // true
isObject(true); // false
```

#### 71、isObjectLike

用于检查参数的值是否为null以及类型是否为对象。

```js
const isObjectLike = val => val !== null && typeof val === 'object';

isObjectLike({}); // true
isObjectLike([1, 2, 3]); // true
isObjectLike(x => x); // false
isObjectLike(null); // false
```

#### 72、isPlainObject

此代码段检查参数的值是否是由Object构造函数创建的对象。

```js
const isPlainObject = val => !!val && typeof val === 'object' && val.constructor === Object;

isPlainObject({
    a: 1
}); // true
isPlainObject(new Map()); // false
```

#### 73、isPromiseLike

用于检查当前的对象是否类似Promise函数。

```js
const isPromiseLike = obj =>
    obj !== null &&
    (typeof obj === 'object' || typeof obj === 'function') &&
    typeof obj.then === 'function';

isPromiseLike({
    then: function() {
        return '';
    }
}); // true
isPromiseLike(null); // false
isPromiseLike({}); // false
```

#### 74、isSameDate

用于判断给定的两个日期是否是同一天。

```js
const isSameDate = (dateA, dateB) => dateA.toISOString() === dateB.toISOString();

isSameDate(new Date(2010, 10, 20), new Date(2010, 10, 20)); // true
```

#### 75、isString

用于检查当前的值是否为字符串类型。

```js
const isString = val => typeof val === 'string';

isString('10'); // true
```

#### 76、isSymbol

用于判断参数的值是否是 Symbol 类型。

```js
const isSymbol = val => typeof val === 'symbol';

isSymbol(Symbol('x')); // true
```

#### 77、isUndefined

用于判断参数的类型是否是 Undefined 类型。

```js
const isUndefined = val => val === undefined;

isUndefined(undefined); // true
```

#### 78、isUpperCase

用于判断当前字符串的字母是否都为大写。

```js
const isUpperCase = str => str === str.toUpperCase();

isUpperCase('ABC'); // true
isLowerCase('A3@$'); // true
isLowerCase('aB4'); // false
```

#### 79、isValidJSON

用于判断给定的字符串是否是 JSON 字符串。

```js
const isValidJSON = str => {
    try {
        JSON.parse(str);
        return true;
    } catch (e) {
        return false;
    }
};

isValidJSON('{"name":"Adam","age":20}'); // true
isValidJSON('{"name":"Adam",age:"20"}'); // false
isValidJSON(null); // true
```

#### 80、last

此函数功能返回数组的最后一个元素。

```js
const last = arr => arr[arr.length - 1];

last([1, 2, 3]); // 3
```

#### 81、matches

此函数功能用于比较两个对象，以确定第一个对象是否包含与第二个对象相同的属性值。

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

#### 82、maxDate

此代码段查找日期数组中最大的日期进行输出。

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

#### 83、maxN

此段代码输出数组中前 n 位最大的数。

```js
const maxN = (arr, n = 1) => [...arr].sort((a, b) => b - a).slice(0, n);

maxN([1, 2, 3]); // [3]
maxN([1, 2, 3], 2); // [3,2]
```

#### 84、minDate

此代码段查找日期数组中最早的日期进行输出。

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

#### 85、minN

此段代码输出数组中前 n 位最小的数。

```js
const minN = (arr, n = 1) => [...arr].sort((a, b) => a - b).slice(0, n);

minN([1, 2, 3]); // [1]
minN([1, 2, 3], 2); // [1,2]
```

#### 86、negate

此函数功能将不满足函数条件的内容筛选出来。

```js
const negate = func => (...args) => !func(...args);

[1, 2, 3, 4, 5, 6].filter(negate(n => n % 2 === 0)); // [ 1, 3, 5 ]
```

#### 87. nodeListToArray

此函数返回给定的DOM节点，并以数组的形式输出。

```js
const nodeListToArray = nodeList => [...nodeList];

nodeListToArray(document.childNodes); // [ <!DOCTYPE html>, html ]
```

#### 88. pad

按照指定的长度生成字符串，如果字符串不够，可以按照设定的字符串内容在左右两边进行填充，默认空格为填充字符。

```js
const pad = (str, length, char = ' ') =>
    str.padStart((str.length + length) / 2, char).padEnd(length, char);

pad('cat', 8); // '  cat   '
pad(String(42), 6, '0'); // '004200'
pad('foobar', 3); // 'foobar'
```

#### 89. radsToDegrees

此函数功能将弧度转换成度数。

```js
const radsToDegrees = rad => (rad * 180.0) / Math.PI;

radsToDegrees(Math.PI / 2); // 90
```

#### 90、randomHexColorCode

此段代码用于生成随机的16进制颜色代码。

```js
const randomHexColorCode = () => {
    let n = (Math.random() * 0xfffff * 1000000).toString(16);
    return '#' + n.slice(0, 6);
};

randomHexColorCode(); // "#e34155"
```

#### 91、randomIntArrayInRange

按照指定的数字范围随机生成长度为 n 的数组。

```js
const randomIntArrayInRange = (min, max, n = 1) =>
    Array.from({
        length: n
    }, () => Math.floor(Math.random() * (max - min + 1)) + min);

randomIntArrayInRange(12, 35, 10); // [ 34, 14, 27, 17, 30, 27, 20, 26, 21, 14 ]
```

#### 92、randomIntegerInRange

按照指定的范围内生成一个随机整数。

```js
const randomIntegerInRange = (min, max) => Math.floor(Math.random() * (max - min + 1)) + min;

randomIntegerInRange(0, 5); // 3
```

#### 93、randomNumberInRange

该代码段可用于返回指定范围内的随机数（包含小数部分）。

```js
const randomNumberInRange = (min, max) => Math.random() * (max - min) + min;

randomNumberInRange(2, 10); // 6.0211363285087005
```

#### 94、readFileLines

此段代码将读取到的文本内容，按行分割组成数组进行输出。

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

#### 95、Redirect to a URL

此函数功能将页面导向一个指定的网站地址。

```js
const redirect = (url, asLink = true) =>
    asLink ? (window.location.href = url) : window.location.replace(url);

redirect('https://www.qianduandaren.com');
```

#### 96、reverse

此段代码用于将一段字符串进行颠倒。

```js
const reverseString = str => [...str].reverse().join('');

reverseString('foobar'); // 'raboof'
```

#### 97、round

将小数按照指定的位数，进行四舍五入保留。

```js
const round = (n, decimals = 0) => Number(`${Math.round(`${n}e${decimals}`)}e-${decimals}`);

round(1.005, 2); // 1.01
```

#### 98、runPromisesInSeries

通过数组的形式，连续运行多个promise。

```js
const runPromisesInSeries = ps => ps.reduce((p, next) => p.then(next), Promise.resolve());
const delay = d => new Promise(r => setTimeout(r, d));

runPromisesInSeries([() => delay(1000), () => delay(2000)]);
// Executes each promise sequentially, taking a total of 3 seconds to complete
```

#### 99、sample

从数组中获取一个随机数。

```js
const sample = arr => arr[Math.floor(Math.random() * arr.length)];

sample([3, 7, 9, 11]); // 9
```

#### 100、sampleSize

在数组中随机生选择 n 个元素生成新的数组，如果n大于数组长度，则为随机整个数组的排序。这里使用到了 Fisher–Yates shuffle 洗牌算法。

简单来说 Fisher–Yates shuffle 算法是一个用来将一个有限集合生成一个随机排列的算法（数组随机排序）。这个算法生成的随机排列是等概率的。同时这个算法非常高效。

```js
const sampleSize = ([...arr], n = 1) => {
    let m = arr.length;
    while (m) {
        const i = Math.floor(Math.random() * m--);
        [arr[m], arr[i]] = [arr[i], arr[m]];
    }
    return arr.slice(0, n);
};

sampleSize([1, 2, 3], 2); // [3,1]
sampleSize([1, 2, 3], 4); // [2,3,1]
```

#### 101、 scrollToTop

此函数功能将页面平滑的滚动到页面的顶部。

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

#### 102、serializeCookie

此段代码用于将 cookie 序列化成 name-value 的形式方便你存储在 Set-Cookie 头信息里。

```js
const serializeCookie = (name, val) => `${encodeURIComponent(name)}=${encodeURIComponent(val)}`;

serializeCookie('foo', 'bar'); // 'foo=bar'
```

#### 103、serializeCookie

此段代码用于在相应的DOM节点添加属性和值。

```js
const setStyle = (el, ruleName, val) => (el.style[ruleName] = val);

setStyle(document.querySelector('p'), 'font-size', '20px');
// The first <p> element on the page will have a font-size of 20px
```

#### 104、 shallowClone

此段代码用于浅复制一个对象。

```js
const shallowClone = obj => Object.assign({}, obj);

const a = {
    x: true,
    y: 1
};
const b = shallowClone(a); // a !== b
```

#### 105、show

从段代码用于显示所有指定的 DOM 元素。

```js
const show = (...el) => [...el].forEach(e => (e.style.display = ''));

show(...document.querySelectorAll('img')); // Shows all <img> elements on the page
```

#### 106、shuffle

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

#### 107、similarity

查找两个数组之间的交集。

```js
const similarity = (arr, values) => arr.filter(v => values.includes(v));

similarity([1, 2, 3], [1, 2, 4]); // [1, 2]
```

#### 108、sleep

用于延迟异步函数的执行。

```js
const sleep = ms => new Promise(resolve => setTimeout(resolve, ms));

async function sleepyWork() {
    console.log("I'm going to sleep for 1 second.");
    await sleep(1000);
    console.log('I woke up after 1 second.');
}
```

#### 109、smoothScroll

此段代码用于让指定的DOM节点平滑滚动到可视区域。

```js
const smoothScroll = element =>
    document.querySelector(element).scrollIntoView({
        behavior: 'smooth'
    });

smoothScroll('#fooBar');
// scrolls smoothly to the element with the id fooBar
smoothScroll('.fooBar');
// scrolls smoothly to the first element with a class of fooBar
```

#### 110、sortCharactersInString

此段代码将单词的内容按照字母的顺序进行重新排序。

```js
const sortCharactersInString = str => [...str].sort((a, b) => a.localeCompare(b)).join('');

sortCharactersInString('cabbage');
// 'aabbceg'
```

#### 111、splitLines

用于将一段字符串按照”换行符“分割成数组进行输出。

```js
const splitLines = str => str.split(/\r?\n/);
splitLines('This\nis a\nmultiline\nstring.\n');
// ['This', 'is a', 'multiline', 'string.' , '']
```

#### 112、tripHTMLTags

格式化去掉 HTML 代码内容，输出文本内容。

```js
const stripHTMLTags = str => str.replace(/<[^>]*>/g, '');

stripHTMLTags('<p><em>lorem</em> <strong>ipsum</strong></p>');
// 'lorem ipsum'
```

#### 113、sum

此段代码用于计算数字之和。

```js
const sum = (...arr) => [...arr].reduce((acc, val) => acc + val, 0);

sum(1, 2, 3, 4); // 10
sum(...[1, 2, 3, 4]); // 10
```

#### 114、tail

用于获取数组除第一个元素之外的所有元素，如果数组只有一个元素，则返回本数组。

```js
const tail = arr => (arr.length > 1 ? arr.slice(1) : arr);

tail([1, 2, 3]); // [2,3]
tail([1]); // [1]
```

#### 115、take

从数组开始位置截取n个元素，生成新的数组。

```js
const take = (arr, n = 1) => arr.slice(0, n);

take([1, 2, 3], 5); // [1, 2, 3]
take([1, 2, 3], 0); // []
```

#### 116、takeRight

从数组开始末尾截取n个元素，生成新的数组。

```js
const takeRight = (arr, n = 1) => arr.slice(arr.length - n, arr.length);

takeRight([1, 2, 3], 2); // [ 2, 3 ]
takeRight([1, 2, 3]); // [3]
```

#### 117、timeTaken

用来计算函数执行的时间。

```js
const timeTaken = callback => {
    console.time('timeTaken');
    const r = callback();
    console.timeEnd('timeTaken');
    return r;
};

timeTaken(() => Math.pow(2, 10));
// 1024, (logged): timeTaken: 0.02099609375ms
```

#### 118、times

按照指定的次数，进行回调函数。

```js
const times = (n, fn, context = undefined) => {
    let i = 0;
    while (fn.call(context, i) !== false && ++i < n) {}
};

var output = '';
times(5, i => (output += i));
console.log(output); // 01234
```

#### 119、toCurrency

此段代码用于按照指定的货币类型格式化货币数字。

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

#### 120、toDecimalMark

用于格式化数字，将其转换成逗号分隔的数字字符串。

```js
const toDecimalMark = num => num.toLocaleString('en-US');

toDecimalMark(12305030388.9087);
// "12,305,030,388.909"
```

#### 121、toggleClass

使用 element.classList.toggle() 来切换元素中指定样式类。

```js
const toggleClass = (el, className) => el.classList.toggle(className);

toggleClass(document.querySelector('p.special'), 'special');
// The paragraph will not have the 'special' class anymore
```

#### 122、tomorrow

用于获取明天的日期。

```js
const tomorrow = () => {
    let t = new Date();
    t.setDate(t.getDate() + 1);
    return t.toISOString().split('T')[0];
};

tomorrow();
// 2019-09-08 (if current date is 2018-09-08)
```

#### 123、unfold

基于初始值，和步长生成一个新的数组。

```js
const unfold = (fn, seed) => {
    let result = [],
        val = [null, seed];
    while ((val = fn(val[1]))) result.push(val[0]);
    return result;
};

var f = n => (n > 50 ? false : [-n, n + 10]);
unfold(f, 10);
// [-10, -20, -30, -40, -50]
```

#### 124、union

合并两个数组，并删除重复的内容。

```js
const union = (a, b) => Array.from(new Set([...a, ...b]));
union([1, 2, 3], [4, 3, 2]);
// [1,2,3,4]
```

#### 125、uniqueElements

使用 ES6 的 set 和 …rest 运算去除数组中重复的元素。

```js
onst uniqueElements = arr => [...new Set(arr)];

uniqueElements([1, 2, 2, 3, 4, 4, 5]);
// [1, 2, 3, 4, 5]
```

#### 126. validateNumber

用于检查参数类型是否是数字。

```js
const validateNumber = n => !isNaN(parseFloat(n)) && isFinite(n) && Number(n) == n;

validateNumber('10'); // true
```

#### 127. words

将一段英文字符串拆分成单词数组（去除一些特殊符号）。

```js
const words = (str, pattern = /[^a-zA-Z-]+/) => str.split(pattern).filter(Boolean);

words('I love javaScript!!');
// ["I", "love", "javaScript"]
words('python, javaScript & coffee');
// ["python", "javaScript", "coffee"]
```
