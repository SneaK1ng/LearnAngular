# 信号：Signal
| **函数**       | **用途**           | **特点**                | **示例**                                 |
| ------------ | ---------------- | --------------------- | -------------------------------------- |
| `signal()`   | 创建一个可读写的响应式信号    | 可读、可写，修改时自动更新依赖的值     | `count = signal(0)`                    |
| `computed()` | 创建一个从其他信号派生的只读信号 | 只读，自动计算并更新依赖的信号值      | `double = computed(() => count() * 2)` |
| `effect()`   | 当依赖项变化时执行副作用代码   | 执行副作用逻辑，依赖信号的变化自动触发执行 | `effect(() => console.log(count()))`   |

```ts
import { Component, signal, computed, effect } from '@angular/core';

// 定义一个组件
@Component({
  selector: 'app-root',         // HTML 中用 <app-root></app-root> 标签
  standalone: true,             // 该组件是独立的，不依赖 AppModule
  imports: [],                  // 引入其他模块（如 NgIf、FormsModule）可以放这里
  template: `                   // 模板部分，包含 HTML 和 Angular 语法
    <h1>Angular 18 Signal 示例</h1>
    <p>当前计数：{{ count() }}</p>
    <p>双倍计数：{{ double() }}</p>  <!-- 显示通过 computed 创建的派生值 -->

    @if (count() > 0) {           // 使用新语法 @if
      <p>计数大于零，值为：{{ count() }}</p>
    } @else {
      <p>还没有开始计数</p>
    }

    <button (click)="increment()">加一</button>   <!-- 按钮点击时调用 increment() 方法 -->
  `
})
export class AppComponent {
  // 定义响应式变量 count，初始值为 0
  count = signal(0);

  // 定义一个 computed（派生值）变量 double，它会自动根据 count 更新
  double = computed(() => {
    return this.count() * 2;  // double 是 count 的两倍
  });

  // 使用 effect 来执行副作用操作，监听 count 的变化
  effect(() => {
    console.log('count 的值变化了！当前值为：', this.count());  // 当 count 改变时，打印新的值
  });

  // increment 方法用于增加 count 的值
  increment() {
    this.count.update(c => c + 1);   // 通过 update 修改 count 的值，每次加 1
  }
}

```

很好，你问得很关键：**Angular 中的 `signal()` 能不能跨组件传值？**

### ✅ 简单回答：

> **不能直接跨组件传值**，但可以通过“共享服务”来实现**跨组件共享 signal 值**。

---

## 🧠 背后原理简要解释：

* `signal()` 是组件内响应式状态，默认只在定义它的组件内可用。
* 如果你希望不同组件共享它，需要把它放在一个\*\*服务（Service）\*\*中，然后注入使用。

---

## ✅ 示例：使用服务实现 signal 跨组件传值

下面展示：

* 一个服务中定义一个 signal 状态
* 父组件和子组件都可以访问和更新这个 signal 值

---

```ts
// ✅ shared.service.ts
import { Injectable, signal } from '@angular/core';

@Injectable({ providedIn: 'root' })
export class SharedService {
  // 声明一个可跨组件使用的 signal
  count = signal(0);

  increment() {
    this.count.update(v => v + 1);
  }

  reset() {
    this.count.set(0);
  }
}
```

---

```ts
// ✅ parent.component.ts
import { Component, computed, inject } from '@angular/core';
import { SharedService } from './shared.service';

@Component({
  selector: 'app-parent',
  template: `
    <h3>父组件</h3>
    <p>count: {{ count() }}</p>
    <button (click)="increment()">加一</button>

    <app-child></app-child>
  `
})
export class ParentComponent {
  private shared = inject(SharedService);

  // 引用 signal
  count = this.shared.count;

  increment() {
    this.shared.increment();
  }
}
```

---

```ts
// ✅ child.component.ts
import { Component, inject } from '@angular/core';
import { SharedService } from './shared.service';

@Component({
  selector: 'app-child',
  template: `
    <h4>子组件</h4>
    <p>count: {{ count() }}</p>
    <button (click)="reset()">重置</button>
  `
})
export class ChildComponent {
  private shared = inject(SharedService);

  count = this.shared.count;

  reset() {
    this.shared.reset();
  }
}
```

---

## ✅ 总结：

| 项目                | 是否支持跨组件     |
| ----------------- | ----------- |
| `signal()` 定义在组件内 | ❌ 不支持       |
| `signal()` 定义在服务里 | ✅ 支持，通过依赖注入 |