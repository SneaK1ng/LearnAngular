---

## ✅ 什么是 Pipe？

Pipe（管道）是 Angular 模板中的一种简洁语法，用来**格式化数据显示**。

语法：

```html
{{ 数据值 | 管道名[:参数1][:参数2] }}
```

---

## ✅ 常用内置 Pipe 一览

| Pipe 名称      | 作用                         | 是否带参数 | 示例 |
| ------------ | -------------------------- | ----- | -- |
| `date`       | 日期格式化                      | ✅     | ✅  |
| `currency`   | 货币格式化                      | ✅     | ✅  |
| `percent`    | 百分比格式化                     | ✅     | ✅  |
| `number`     | 数字格式化（千位分隔）                | ✅     | ✅  |
| `uppercase`  | 转大写                        | ❌     | ✅  |
| `lowercase`  | 转小写                        | ❌     | ✅  |
| `titlecase`  | 每个单词首字母大写                  | ❌     | ✅  |
| `json`       | 对象格式化为 JSON                | ❌     | ✅  |
| `slice`      | 截取字符串或数组                   | ✅     | ✅  |
| `async`      | 处理异步数据（Observable/Promise） | ❌     | ✅  |
| `i18nPlural` | 多数量表达处理（国际化）               | ✅     | ✅  |

---

## ✅ Pipe 使用详解（含例子）

### 🔸 1. `date` 日期格式化

```html
<!-- 原始值：2024-01-01T10:00:00 -->
{{ today | date }} <!-- 默认格式，如 Jan 1, 2024 -->
{{ today | date:'yyyy/MM/dd HH:mm:ss' }} <!-- 自定义格式 -->
{{ today | date:'fullDate' }} <!-- 内置预设格式 -->
```

常用格式字符串：

| 关键词            | 示例输出             |
| -------------- | ---------------- |
| `'short'`      | 1/1/24, 10:00 AM |
| `'mediumDate'` | Jan 1, 2024      |
| `'yyyy/MM/dd'` | 2024/01/01       |

---

### 🔸 2. `currency` 金额格式

```html
{{ 12345 | currency }} <!-- 默认 USD -->
{{ 12345 | currency:'JPY' }} <!-- 显示为日元 ￥ -->
{{ 12345 | currency:'CNY':'symbol':'1.2-2' }} <!-- 1.2-2 表示小数最少1位，最多2位 -->
```

---

### 🔸 3. `percent` 百分比格式

```html
{{ 0.256 | percent }} <!-- 输出：26% -->
{{ 0.256 | percent:'1.1-1' }} <!-- 输出：25.6% -->
```

---

### 🔸 4. `number` 数字格式

```html
{{ 1234567.891 | number }} <!-- 1,234,567.891 -->
{{ 1234.5 | number:'1.0-0' }} <!-- 保留整数：1,235 -->
{{ 1234.5 | number:'1.2-2' }} <!-- 保留两位小数：1,234.50 -->
```

---

### 🔸 5. `uppercase` / `lowercase` / `titlecase`

```html
{{ 'hello world' | uppercase }} <!-- HELLO WORLD -->
{{ 'HELLO WORLD' | lowercase }} <!-- hello world -->
{{ 'hello angular developer' | titlecase }} <!-- Hello Angular Developer -->
```

---

### 🔸 6. `json` 输出对象内容

```ts
user = { name: 'Alice', age: 25 };
```

```html
<pre>{{ user | json }}</pre>
```

输出：

```json
{
  "name": "Alice",
  "age": 25
}
```

---

### 🔸 7. `slice`（截取字符串/数组）

```ts
items = ['a', 'b', 'c', 'd', 'e'];
```

```html
{{ 'AngularPipe' | slice:0:7 }} <!-- Angular -->
{{ items | slice:1:4 }} <!-- ['b', 'c', 'd'] -->
```

---

### 🔸 8. `async`（异步管道）

常用于订阅 `Observable` 或 `Promise`，自动订阅并销毁。

```ts
// TypeScript
name$ = of('SneaK1ng');
```

```html
<p>{{ name$ | async }}</p> <!-- 自动订阅并显示 -->
```

适用于：HTTP 请求结果、RxJS 数据流等。

---

### 🔸 9. `i18nPlural`（国际化数量表达）

适合中英文“人称/数量”的变化。

```ts
mapping = {
  '=0': '没有人',
  '=1': '一个人',
  'other': '# 人'
};

people = 3;
```

```html
{{ people | i18nPlural: mapping }} <!-- 输出：3 人 -->
```

---

## ✅ 小结：什么时候使用 Pipe？

| 场景       | 推荐 Pipe                               |
| -------- | ------------------------------------- |
| 时间/日期显示  | `date`                                |
| 数字/货币格式  | `number`, `currency`, `percent`       |
| 显示大小写格式  | `uppercase`, `lowercase`, `titlecase` |
| 字符串/数组截取 | `slice`                               |
| 异步数据展示   | `async`                               |
| 多语言数量    | `i18nPlural`                          |
| 对象打印调试   | `json`                                |

---
### ✅ 自定义 Pipe（Angular 19 standalone 写法）

举例：创建一个 `appExclaim` 管道，把字符串加个感叹号。

```ts
// exclaim.pipe.ts

import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
  name: 'appExclaim',
  standalone: true  // ✅ Angular 19 推荐方式：standalone pipe
})
export class ExclaimPipe implements PipeTransform {
  transform(value: string, mark: string = '!'): string {
    return value + mark;
  }
}
```

---

### ✅ 在组件中引入 pipe 使用

```ts
import { Component } from '@angular/core';
import { ExclaimPipe } from './exclaim.pipe';

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [ExclaimPipe],
  template: `
    <p>{{ '你好' | appExclaim }}</p>
    <p>{{ '成功' | appExclaim:'!!!' }}</p>
  `
})
export class AppComponent {}
```

---

### ✅ Pipe vs 方法调用

| 比较 | Pipe     | 普通方法      |
| -- | -------- | --------- |
| 性能 | 高，默认是纯函数 | 可能每次变更都执行 |
| 写法 | 简洁清晰     | 方法多时模板复杂  |
| 推荐 | ✅ 强烈推荐   | 仅适合复杂逻辑   |

---

### ✅ 小结

| 特点        | 说明                                   |               |
| --------- | ------------------------------------ | ------------- |
| 用于模板数据格式化 | \`                                   | pipeName\` 语法 |
| 内置丰富      | 日期、数字、字符串等                           |               |
| 支持自定义     | 用 `@Pipe` 创建，推荐使用 `standalone: true` |               |
| 性能好       | 默认是“纯管道”，不会反复执行                      |               |

---