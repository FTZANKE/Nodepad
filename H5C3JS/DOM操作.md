### DOM操作

### 获取元素

1. `getElementById`：返回元素节点

   ```javascript
   document.getElementById('idName');//id名获取，单个id的定义不能重复
   ```

2. `getElementsByClassName`：返回HTMLCollection对象（IE9以下不支持）

   ```javascript
   document.getElementsByClassName('className');//class名获取一组HTMLDom元素
   ```

3. `getElementsByTagName`：返回HTMLCollection对象

   ```javascript
   document.getElementsByTagName('tagName');//标签名获取一组HTMLDom元素
   ```

4. `getElementsByName`：返回nodeList对象,第0项为元素节点

   ```javascript
   document.getElementsByName();//获取一组带有指定name属性的对象
   ```

5. `querySelector`：返回选择器匹配到的第一个元素节点

   ```javascript
   document.querySelector('#box em');//选择器同css用法一致,支持由外到内的嵌套写法
   ```

6. `querySelectorAll`：返回nodeList对象（类似数组对象,每个值为选中元素节点)

   ```javascript
   document.querySelectorAll();//获取HTMLDom中匹配指定 CSS 选择器的所有元素
   ```

7. `childNodes`：获取子元素集合（IE：只获取元素节点；非IE：获取元素节点与文本节点；)

   ```javascript
   document.getElementById().childNodes;//获取Id下的所有子元素集合
   //childNodes：获取指定元素的子元素集合(包括HTML节点，所有属性，文本),可以通过nodeType来判断是哪种类型的节点，当nodeType==1时是元素节点，2是属性节点，3是文本节点(IE9/Firefox不支持childNodes(i))
   ```

   `children`：获取指定元素的子元素节点集合（只获取元素节点，浏览器表现相同，Firefox下不支持()取集合元素）

   ```javascript
   document.getElementById().children;
   ```

8. `lastElementChild`：获取最后一个元素节点（IE<9不支持）

   ```javascript
   document.getElementById().lastElementChild;
   ```

9. `firstElementChild`：获取第一个元素节点（IE<9不支持)

   ```javascript
   document.getElementById().firstElementChild;
   ```

10. `nextSibling`：获取后一个兄弟元素节点（IE<9：后一个兄弟元素节点；非IE6,7,8：后一个兄弟节点，文本节点或者元素节点)

    ```js
    document.getElementById().nextSibling;
    document.getElementById().nextElementSibling;
    ```

      `nextElementSibling`：（IE<9不支持）

      `nextSibling`属性返回元素节点之后的兄弟节点（包括文本节点、注释节点即回车、换行、空格、文本等等)；

      `nextElementSibling`属性只返回元素节点之后的兄弟元素节点（不包括文本节点、注释节点)；

11. `previousSibling`：获取前一个兄弟元素节点（IE<9前一个兄弟元素节点；非IE6,7,8：前一个兄弟节点，文本节点或者元素节点）

    ```js
    document.getElementById().previousSibling;
    document.getElementById().previousElementSibling;
    ```

    `previousElementSibling`：获取指定元素的前一个兄弟元素（相同节点树层中的前一个元素节点,IE<9不支持)

    `previousSibling`属性返回元素节点之前的兄弟节点（包括文本节点、注释节点）；

    `previousElementSibling`属性只返回元素节点之前的兄弟元素节点（不包括文本节点、注释节点）；

12. `parentNode`：获取父元素节点（parentElement用法一致,仅IE支持）

    ```js
    document.getElementById().parentNode;
    ```

13. `offsetParent`：获取第一个设置定位的上级元素,返回元素节点

    ```javascript
    console.log(document.getElementById().offsetParent)
    ```

---

### 操作内容

- **`innerText`**

- `outerHTML`：对元素自身的快速替换，比如:

  ```javascript
  //<p id="hello">Hello, I am a demo</p>
  $('hello').outerHTML = '<p>Hello, I am a replacement</p>';
  ```

- `documentFragment`：能实现高效率的DOM节点插入操作

  ```javascript
     var docFragment = document.createDocumentFragment();
  ```

- **`textContent`**：针对元素中的文本内容的操作，提取元素本身和其子元素中文本内容， 比如：

  ```javascript
  //<div id="test">
  //	<p>whatever, blah blah.</p>
  //	hello，I am a <em>Demo</em>
  //</div>
  $('test').textContent
  //whatever, blah blah.hello, I am a Demo
  ```

  ✔把文本直接连接起来。IE9+和其他浏览器都很好的支持此方法。
  
  ---

### 操作属性

- src 、href

- title、alt 、id

- 表单标签属性：type、value、[checked、selected、disabled (boolean 类型的值)]

- 操作样式：style、className

- attribute：

  1. getAttribute()（返回元素上一个指定的属性值）
  2. setAttribute()（设置指定元素上的某个属性值。如果属性已经存在，则更新该值；否则，使用指定的名称和值添加一个新的属性）
  3. removeAttribute()（获取某个属性当前的值）
  4. attributes()（要删除某个属性）
  5. dataset()（获取自定义属性值的使用（date-*自定义属性））

### 节点操作

- 查找节点

  - parentNode（父节点）
  - childNodes（子节点）
  - previousSibling（下一个同胞元素）
  - nextSibling（上一个同胞元素）
  - firstChild（第一个元素）
  - lastChild（最后一个元素）
  - 元素节点：
    - parentElement（父元素节点）
    - children
    - previousElementSibling（返回当前节点的前一个兄弟节点，无返回null）
    - nextElementSibling（返回当前元素在其父元素的子元素节点中的后一个元素节点,如果该元素已经是最后一个元素节点，则返回null）
    - firstElementChild（返回对象的第一个子元素，如果没有子元素，则为null）
    - lastElementChild（返回对象的最后一个子元素， 如果没有子元素，则为null）

- 新增

  - **createElement（）标签节点**
  - createTextNode（）文本节点
  - createAttribute（）设置标签属性
  - **appendChild()**添加节点（末尾）
  - **inserBefore()**在参考节点之前插入一个拥有指定父节点的子节点。
  - append() 如果新增元素是从页面获取的，那么会执行移动 操作，而不是复制
  - cloneNode()  克隆节点

- 删除

  **remove() **删除元素本身及所有子内容

  **removeChild()**保留当前容器，删除所有子内容

- 修改

  **replaceChild()**替换节点