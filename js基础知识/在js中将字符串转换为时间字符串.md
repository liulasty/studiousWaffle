你可以使用 JavaScript 中的 Date 对象和相关方法来实现将字符串转换为指定格式的日期和时间。以下是一种可能的实现方法：

```javascript
// 原始字符串
const dateString = '2024-04-11T04:34:23.000Z';

// 创建 Date 对象
const date = new Date(dateString);

// 获取年、月、日、小时、分钟和秒
const year = date.getFullYear();
const month = date.getMonth() + 1; // 月份从 0 开始，需要加 1
const day = date.getDate();
const hours = date.getHours();
const minutes = date.getMinutes();
const seconds = date.getSeconds();

// 格式化为指定字符串
const formattedDateString = `${year}年${month < 10 ? '0' + month : month}月${day < 10 ? '0' + day : day}日${hours < 10 ? '0' + hours : hours}:${minutes < 10 ? '0' + minutes : minutes}:${seconds < 10 ? '0' + seconds : seconds}`;

console.log(formattedDateString);
```

运行以上代码，控制台输出的结果将为：`2024年04月11日12:34:23`。注意，这里假设原始字符串的时区为 UTC，因此转换后的时间也是相应的 UTC 时间。如果需要转换为本地时间，可以使用 Date 对象的其他方法来处理。