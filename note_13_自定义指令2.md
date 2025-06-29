下面简单介绍一下 **Angular 中的自定义指令（Attribute 指令）**，并给出一个完整例子（基于 Angular 18 新语法）。

---

### ✅ 自定义指令用途

自定义指令可以让我们创建自己的“标签功能”，比如控制元素样式、行为等。

---

### ✅ 示例：`appHighlight` 指令

功能：当鼠标悬停时，让元素高亮显示。

---

#### 1. 指令文件（如 `highlight.directive.ts`）

```ts
import { Directive, HostListener, HostBinding } from '@angular/core';

@Directive({
  selector: '[appHighlight]',
  standalone: true
})
export class HighlightDirective {
  @HostBinding('style.backgroundColor') bg = '';

  @HostListener('mouseenter') onMouseEnter() {
    this.bg = 'yellow';
  }

  @HostListener('mouseleave') onMouseLeave() {
    this.bg = '';
  }
}
```

---

#### 2. 在组件中使用指令（通过 `imports` 引入）

```ts
import { Component } from '@angular/core';
import { HighlightDirective } from './highlight.directive';

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [HighlightDirective], // ✅ 这里引入
  template: `
    <p appHighlight>把鼠标移上来试试</p>
  `
})
export class AppComponent {}
```

---
### Q:所以为什么要用Directive而不是在component里面直接写逻辑呢？
### A:这是一个非常好的问题。我们来简单说清楚：

---

### ✅ 先说结论：

**用 `Directive` 是为了逻辑复用、职责分离和增强 HTML 行为**。
**Component 是“带模板的可视模块”，Directive 是“行为增强”工具。**

---

### ✅ 为什么不能都放在 Component 里？

| 问题       | 解释                                      |
| -------- | --------------------------------------- |
| ❌ 不利于复用  | 如果某段行为（比如高亮、拖拽、自动聚焦）在多个组件中用，写在组件里就得复制粘贴 |
| ❌ 污染组件逻辑 | 有些行为其实跟业务无关（如 DOM 交互），放组件里让逻辑变复杂        |
| ❌ 难维护    | 行为和展示耦合，今后要换一个组件使用同样功能，得重复代码或提取工具函数     |

---

### ✅ Directive 更适合哪些场景？

| 场景        | 举例                        |
| --------- | ------------------------- |
| 操作 DOM 行为 | 鼠标悬停高亮、拖动、自动聚焦            |
| 添加样式      | 控制 CSS 类或样式               |
| 条件渲染或模板控制 | 自定义 `*ifUserLogin` 等结构型指令 |
| 通用行为封装    | 多个组件都用的相同 UI 行为           |

---

### ✅ 举个简单例子对比

#### ❌ 全部写在组件里（重复用要复制）

```ts
@Component({ /*...*/ })
export class MyCardComponent {
  bg = '';
  @HostListener('mouseenter') onMouseEnter() { this.bg = 'yellow'; }
}
```

#### ✅ 抽成指令后，所有组件都能用

```html
<p appHighlight>文字1</p>
<div appHighlight>文字2</div>
```

---

### ✅ 总结一句话：

> **Directive 是“通用功能组件化”的方式**，适合封装行为，而不是界面。
> 用组件是“画界面”，用指令是“加能力”。

---

