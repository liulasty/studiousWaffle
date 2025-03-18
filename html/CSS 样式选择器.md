CSS 样式选择器是用于选择 HTML 元素并为其应用样式的一种机制。CSS 提供了多种选择器类型，可以根据元素的标签名、类名、ID、属性、状态等进行选择。以下是常见的 CSS 样式选择器及其用法：

---

### **1. 基本选择器**
#### **1.1 元素选择器**
根据 HTML 标签名选择元素。
```css
p {
    color: blue;
}
```
- 选择所有 `<p>` 元素，并将文本颜色设置为蓝色。

#### **1.2 类选择器**
根据元素的 `class` 属性选择元素。
```css
.text-red {
    color: red;
}
```
- 选择所有 `class="text-red"` 的元素，并将文本颜色设置为红色。

#### **1.3 ID 选择器**
根据元素的 `id` 属性选择元素。
```css
#header {
    background-color: yellow;
}
```
- 选择 `id="header"` 的元素，并将背景颜色设置为黄色。

#### **1.4 通配符选择器**
选择所有元素。
```css
* {
    margin: 0;
    padding: 0;
}
```
- 选择页面中的所有元素，并将它们的 `margin` 和 `padding` 设置为 0。

---

### **2. 组合选择器**
#### **2.1 后代选择器**
选择某个元素的后代元素。
```css
div p {
    font-size: 16px;
}
```
- 选择所有在 `<div>` 元素内部的 `<p>` 元素。

#### **2.2 子元素选择器**
选择某个元素的直接子元素。
```css
ul > li {
    list-style-type: none;
}
```
- 选择所有 `<ul>` 元素的直接子元素 `<li>`。

#### **2.3 相邻兄弟选择器**
选择紧接在某个元素后的同级元素。
```css
h1 + p {
    margin-top: 0;
}
```
- 选择紧接在 `<h1>` 元素后的第一个 `<p>` 元素。

#### **2.4 通用兄弟选择器**
选择某个元素后的所有同级元素。
```css
h1 ~ p {
    color: green;
}
```
- 选择所有在 `<h1>` 元素后的 `<p>` 元素。

---

### **3. 属性选择器**
#### **3.1 根据属性选择**
选择具有特定属性的元素。
```css
a[target] {
    color: purple;
}
```
- 选择所有具有 `target` 属性的 `<a>` 元素。

#### **3.2 根据属性值选择**
选择属性值等于特定值的元素。
```css
input[type="text"] {
    border: 1px solid #ccc;
}
```
- 选择所有 `type="text"` 的 `<input>` 元素。

#### **3.3 根据属性值前缀选择**
选择属性值以特定字符串开头的元素。
```css
a[href^="https"] {
    color: green;
}
```
- 选择所有 `href` 属性值以 `https` 开头的 `<a>` 元素。

#### **3.4 根据属性值后缀选择**
选择属性值以特定字符串结尾的元素。
```css
img[src$=".png"] {
    border: 2px solid red;
}
```
- 选择所有 `src` 属性值以 `.png` 结尾的 `<img>` 元素。

#### **3.5 根据属性值包含选择**
选择属性值包含特定字符串的元素。
```css
div[class*="box"] {
    background-color: yellow;
}
```
- 选择所有 `class` 属性值包含 `box` 的 `<div>` 元素。

---

### **4. 伪类选择器**
伪类选择器用于选择元素的特定状态。
#### **4.1 链接状态**
```css
a:link {
    color: blue;
}
a:visited {
    color: purple;
}
a:hover {
    color: red;
}
a:active {
    color: green;
}
```
- `:link`：未访问的链接。
- `:visited`：已访问的链接。
- `:hover`：鼠标悬停时的链接。
- `:active`：鼠标点击时的链接。

#### **4.2 表单元素状态**
```css
input:focus {
    border-color: blue;
}
input:disabled {
    background-color: #eee;
}
```
- `:focus`：获取焦点的输入框。
- `:disabled`：被禁用的输入框。

#### **4.3 结构伪类**
```css
li:first-child {
    font-weight: bold;
}
li:last-child {
    color: red;
}
li:nth-child(2n) {
    background-color: #f0f0f0;
}
```
- `:first-child`：选择第一个子元素。
- `:last-child`：选择最后一个子元素。
- `:nth-child(n)`：选择第 n 个子元素。

---

### **5. 伪元素选择器**
伪元素选择器用于选择元素的特定部分。
#### **5.1 在元素前插入内容**
```css
h1::before {
    content: "★";
    color: gold;
}
```
- 在每个 `<h1>` 元素前插入一个金色的星号。

#### **5.2 在元素后插入内容**
```css
p::after {
    content: "（完）";
    color: gray;
}
```
- 在每个 `<p>` 元素后插入灰色的“（完）”。

#### **5.3 选择首字母**
```css
p::first-letter {
    font-size: 24px;
    color: red;
}
```
- 选择每个 `<p>` 元素的第一个字母。

#### **5.4 选择首行**
```css
p::first-line {
    font-weight: bold;
}
```
- 选择每个 `<p>` 元素的第一行。

---

### **6. 组合使用选择器**
#### **6.1 多选择器**
同时选择多个元素。
```css
h1, h2, h3 {
    font-family: Arial, sans-serif;
}
```
- 选择所有 `<h1>`、`<h2>` 和 `<h3>` 元素。

#### **6.2 复合选择器**
结合多个条件选择元素。
```css
button.primary {
    background-color: blue;
    color: white;
}
```
- 选择 `class="primary"` 的 `<button>` 元素。

---

### **总结**
CSS 选择器是前端开发中非常重要的工具，合理使用选择器可以高效地为页面元素应用样式。掌握这些选择器的用法，能够帮助你编写更简洁、可维护的 CSS 代码。