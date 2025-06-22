在 Angular 18 中，生命周期钩子（Lifecycle Hooks）主要用于组件和指令，用于在组件创建、变更、销毁等不同阶段执行逻辑。虽然 Angular 18 引入了一些结构性语法改进，但生命周期的基本流程并没有变化，以下是整理后的生命周期钩子及其触发时机：

---

## ✅ Angular 18 生命周期钩子一览

| 生命周期钩子                  | 触发时机                               |
| ----------------------- | ---------------------------------- |
| `constructor`           | 构造函数初始化时触发（Angular 还未绑定输入属性）       |
| `ngOnChanges`           | 输入属性（`@Input`）变化时触发（首次和每次变更时都会调用）  |
| `ngOnInit`              | 初始化时调用，仅调用一次（输入属性已准备好）             |
| `ngDoCheck`             | 每次脏值检测时都会触发（适合手动检测变更）              |
| `ngAfterContentInit`    | 内容投影（`<ng-content>`）初始化完成时调用，仅调用一次 |
| `ngAfterContentChecked` | 每次脏值检测后，对内容投影进行检查之后调用              |
| `ngAfterViewInit`       | 组件视图（包括子组件）初始化完成后调用，仅调用一次          |
| `ngAfterViewChecked`    | 每次脏值检测后，对组件视图进行检查之后调用              |
| `ngOnDestroy`           | 组件/指令销毁前调用（用于清理资源）                 |

---

## 🔁 脏值检查发生的时机：
```text
Angular 会在以下操作后自动触发脏值检查：
用户事件（点击、输入等）
HTTP 请求返回
setTimeout / setInterval
Promise / Observable 完成
表单输入等
```
### ✅ 区别一览表：
| 项目       | 脏值检查（Dirty Checking） | `ngOnChanges()`         |
| -------- | -------------------- | ----------------------- |
| **目的**   | 检查数据变化（不管来自哪里）       | 监听 `@Input()` 输入属性的变化   |
| **触发条件** | 每次检测周期都会执行           | 只有当 `@Input()` 绑定值发生变化时 |
| **作用范围** | 所有绑定的数据（模板中的变量等）     | 仅限于 `@Input()` 修饰的属性    |
| **自动调用** | 是                    | 是                       |
| **性能影响** | 多次调用，可能影响性能          | 只在输入变化时调用，影响较小          |

## ✅ 生命周期顺序图（初始化阶段）

```text
constructor
↓
ngOnChanges（如果有@Input）
↓
ngOnInit
↓
ngDoCheck
↓
ngAfterContentInit
↓
ngAfterContentChecked
↓
ngAfterViewInit
↓
ngAfterViewChecked
```

---

## ✅ 生命周期顺序图（变更检测阶段）

```text
ngOnChanges（如果@Input变化）
↓
ngDoCheck
↓
ngAfterContentChecked
↓
ngAfterViewChecked
```

---

## ✅ 生命周期示例（Angular 18 语法）

```ts
import { Component, input } from '@angular/core';

@Component({
  selector: 'app-demo',
  standalone: true,
  template: `<p>生命周期组件</p>`,
})
export class DemoComponent {
  // 使用 @input 装饰器（新语法，替代 @Input 装饰器）
  @input() data: string = '';

  constructor() {
    // 构造函数：只能做轻量初始化，不能访问输入属性
    console.log('constructor');
  }

  ngOnChanges() {
    // 输入属性变化时触发
    console.log('ngOnChanges');
  }

  ngOnInit() {
    // 初始化阶段：此时可以使用输入属性
    console.log('ngOnInit');
  }

  ngDoCheck() {
    // 脏值检测阶段
    console.log('ngDoCheck');
  }

  ngAfterContentInit() {
    // 内容投影初始化完成
    console.log('ngAfterContentInit');
  }

  ngAfterContentChecked() {
    // 内容投影检测完成
    console.log('ngAfterContentChecked');
  }

  ngAfterViewInit() {
    // 视图初始化完成
    console.log('ngAfterViewInit');
  }

  ngAfterViewChecked() {
    // 视图检测完成
    console.log('ngAfterViewChecked');
  }

  ngOnDestroy() {
    // 组件销毁前
    console.log('ngOnDestroy');
  }
}
```

---
