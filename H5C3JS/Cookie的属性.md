### cookie的属性

| 属性     | 说明                                | 示例                               |
| :------- | ----------------------------------- | ---------------------------------- |
| name     | cookie的key值                       | 'id=sjjdjdd'                       |
| expires  | 到期时间                            | 'expires=21 Oct 2015 07:28:00 GMT' |
| domin    | cookie生效的域                      | 'domin=im.baidu.com'               |
| path     | cookie生效的路径                    | 'path=/'                           |
| secure   | 是否在https下生效                   | 'secure'                           |
| httponly | 是否允许JS获取                      | 'httponly'                         |
| max-age  | 以秒为单位设置过期时间(IE678不生效) | '31536000'                         |

1. 创建 cookie :  

   ```javascript
   // 创建 cookie
   document.cookie="username=John Doe";
   // 为 cookie 添加一个过期时间（以 UTC 或 GMT 时间）。默认情况下，cookie 在浏览器关闭时删除
   document.cookie="username=John Doe; expires=Thu, 18 Dec 2043 12:00:00 GMT";
   // path 参数告诉浏览器 cookie 的路径。默认情况下，cookie 属于当前页面
   document.cookie="username=John Doe; expires=Thu, 18 Dec 2043 12:00:00 GMT; path=/";
   ```

   

2. 修改 cookie：

   ```javascript
   // cookie 的 name 属性是唯一的，重新创建一样的 name 属性值可做到修改
   document.cookie="username=John Smith; expires=Thu, 18 Dec 2043 12:00:00 GMT; path=/";
   ```

   

3. 删除 Cookie： 

   ```javascript
   // 设置 expires 参数为以前的时间，到期将被删除，name 的属性值可不写
   document.cookie = "username=; expires=Thu, 01 Jan 1970 00:00:00 GMT";
   ```

4. 一个小方法：toGMTString()

   ```javascript
   // 当前的时间
   var data = new Date();
   console.log(data); // Sat Oct 16 2021 14:44:41 GMT+0800 (中国标准时间)
   console.log(data.toGMTString()); //Sat, 16 Oct 2021 06:44:41 GMT
   ```

   **总结**：toGMTString() 方法可根据格林威治时间 (GMT) 把 Date 对象转换为字符串，并返回结果。

5. 设置 cookie 属性按照表格的顺序写

6. 会话 cookie：如果 cookie 不包含到期日期，则可视为会话 cookie。 会话 cookie 存储在内存中，决不会写入磁盘。 当浏览器关闭时，cookie 将从此永久丢失

   持久性 cookie: 如果 cookie 包含到期日期，则可视为持久性 cookie。 在指定的到期日期，cookie 将从磁盘中删除。

7. cookie 的优点：
   对数据安全性要求不高。
   跨页面共享数据。
   存储空间约4kb左右

   cookie 的缺点
   1）cookie可能被禁用；
   2）cookie与浏览器相关，不能互相访问；（cookie跨域可以实现，存储安全性风险）
   3）cookie可能被用户删除；
   4）cookie安全性不够高；
   5）cookie存储空间很小，大约4kb左右。