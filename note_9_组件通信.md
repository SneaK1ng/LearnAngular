# 1. 组件通信

---

```ts
// ✅ 子组件 child.component.ts
import { Component, Input, Output, EventEmitter } from '@angular/core';

@Component({
  selector: 'app-child',
  template: `
    <!-- 显示从父组件传来的用户名 -->
    <p>子组件：接收到的用户名是：{{ username }}</p>

    <!-- 点击按钮时，向父组件发送一个事件 -->
    <button (click)="sendToParent()">通知父组件</button>
  `
})
export class ChildComponent {
  // 使用 @Input() 接收父组件传来的用户名
  @Input() username: string = '';

  // 使用 @Output() 定义一个事件，父组件可以监听它
  @Output() notify = new EventEmitter<string>();

  // 点击按钮后触发该函数，向父组件发消息
  sendToParent() {
    // 发出事件，传递字符串数据
    this.notify.emit('你好，父组件！来自子组件的问候');
  }
}
```

---

```ts
// ✅ 父组件 parent.component.ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-parent',
  template: `
    <h2>父组件</h2>

    <!-- 输入框可修改父组件中的 username 变量 -->
    <input [(ngModel)]="username" placeholder="输入用户名">

    <!-- 向子组件传值 username，同时监听 notify 事件 -->
    <app-child
      [username]="username"
      (notify)="handleNotify($event)">
    </app-child>

    <!-- 显示从子组件接收到的消息 -->
    <p>来自子组件的消息：{{ childMessage }}</p>
  `
})
export class ParentComponent {
  // 要传给子组件的变量
  username = '默认用户';

  // 接收子组件传来的消息
  childMessage = '';

  // 当子组件发出 notify 事件时，执行该方法
  handleNotify(msg: string) {
    // 把收到的消息保存到 childMessage 变量中，供模板显示
    this.childMessage = msg;
  }
}
```

---

```ts
// ✅ 模块配置 app.module.ts（确保一切可用）
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { FormsModule } from '@angular/forms'; // 让 [(ngModel)] 生效

import { AppComponent } from './app.component';
import { ParentComponent } from './parent.component';
import { ChildComponent } from './child.component';

@NgModule({
  declarations: [
    AppComponent,
    ParentComponent,
    ChildComponent
  ],
  imports: [
    BrowserModule,
    FormsModule  // 用于支持双向绑定
  ],
  bootstrap: [AppComponent]
})
export class AppModule {}
```

---

```ts
// ✅ 根组件 app.component.ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  template: `<app-parent></app-parent>`
})
export class AppComponent {}
```
---

# 总结

- 父传子用 `[username]="username"`
- 子传父用 `(notify)="handleNotify($event)"`

---

| 写法                                | 含义             | 方向    |
| --------------------------------- | -------------- | ----- |
| `[username]="parentUsername"`     | 绑定属性，把父 -> 子   | 父 ➜ 子 |
| `(notify)="handleNotify($event)"` | 监听事件，子发出 ➜ 父执行 | 子 ➜ 父 |

---

# 1.1 @Input + get set

---

## ✅ @Input + setter（set）组合用法：

```ts
// ✅ 子组件（ChildComponent）
import { Component, Input } from '@angular/core';

@Component({
  selector: 'app-child',
  template: `
    <p>接收到的名字是：{{ displayName }}</p>
  `
})
export class ChildComponent {
  // 私有变量存储传入的值
  private _name = '';

  // 使用 @Input 配合 set，当父组件传值时自动触发 set 方法
  @Input()
  set name(value: string) {
    this._name = value.trim(); // 可以做格式处理、验证等
  }

  get name() {
    return this._name;
  }

  get displayName() {
    return this._name.toUpperCase(); // 举例：模板中展示大写的
  }
}
```

---

## ✅ 父组件传值模板写法：

```html
<app-child [name]="parentName"></app-child>
```

```ts
// 父组件中
export class ParentComponent {
  parentName = ' 张三 ';
}
```

---

## ✅ 模板中仍然直接用属性访问

* **你不需要改写成 `name()`**，即使 `name` 是 setter 和 getter。
* 在子组件内部的模板里，像平常一样写 `{{ name }}` 或 `{{ displayName }}` 就可以了。

---

## ✅ 总结一句话：

> 当你用 `@Input() set xxx(value)` 处理父组件传来的值，**模板里还是直接用 `xxx` 属性名访问即可，不需要加括号 `()`**。