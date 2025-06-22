# 用法1：#属性名="ngModel"

在 Angular 18 中，使用 `ngModel` 时，`email.invalid` 这种状态属性实际上是属于 `NgModel` 指令实例的。要在 `AppComponent` 中使用这些状态属性，必须确保你访问的是表单控件的 `ngModel` 实例。

### 关键点：

* `email.invalid`、`email.touched` 等状态属性是通过模板引用变量 `#email="ngModel"` 访问的。**如果要在组件中使用它们，应该通过 `ViewChild` 访问模板中 `ngModel` 指令的实例。**

### 如何在 `AppComponent` 中访问这些状态？

1. 使用 `@ViewChild` 获取 `NgModel` 实例。
2. 在组件中通过 `ViewChild` 访问这些属性。

### ✅ 解决方案：通过 `@ViewChild` 访问 `ngModel` 状态

```typescript
// app.component.ts
import { Component, ViewChild, AfterViewInit } from '@angular/core';
import { NgModel } from '@angular/forms'; // 引入 NgModel 类型

@Component({
  selector: 'app-root',
  standalone: true, // 独立组件，不需要 NgModule
  imports: [FormsModule], // 直接引入 FormsModule
  template: `
    <h2>Angular 18 表单验证示例（访问 ngModel 状态）</h2>

    <input
      type="email"
      name="email"
      [(ngModel)]="emailValue"
      #email="ngModel"
      required
      email
      placeholder="请输入邮箱"
    />

    <!-- 显示表单控件状态 -->
    <p>{{ email?.pristine ? '表单控件为 pristine' : '表单控件为 dirty' }}</p>
    <p>{{ email?.invalid && email?.touched ? '请输入有效的邮箱地址' : '' }}</p>
    <p>{{ email?.valid && email?.touched ? '邮箱有效' : '' }}</p>
    <p>{{ email?.untouched ? '表单控件为 untouched' : '表单控件为 touched' }}</p>

    <!-- 提交按钮，只有输入合法时可用 -->
    <button [disabled]="email?.invalid">提交</button>

    <!-- 组件中获取表单控件状态 -->
    <p>当前控件状态：</p>
    <ul>
      <li>pristine: {{ email?.pristine }}</li>
      <li>dirty: {{ email?.dirty }}</li>
      <li>valid: {{ email?.valid }}</li>
      <li>invalid: {{ email?.invalid }}</li>
      <li>touched: {{ email?.touched }}</li>
      <li>untouched: {{ email?.untouched }}</li>
    </ul>
  `
})
export class AppComponent implements AfterViewInit {
  emailValue = '';

  // 使用 ViewChild 获取模板中的 ngModel 实例
  @ViewChild('email') emailModel!: NgModel;

  // 确保 ngModel 实例已加载后访问状态
  ngAfterViewInit() {
    console.log('ngModel 状态：', this.emailModel);
  }
}
```

---

### ✅ 解释：

1. **`@ViewChild('email') emailModel!: NgModel;`**：通过 `@ViewChild` 获取模板中 `#email="ngModel"` 绑定的 `NgModel` 实例。
2. **`ngAfterViewInit()`**：确保模板中的 `ngModel` 实例已加载并可用，在该生命周期钩子中访问表单控件的状态。
3. **通过 `emailModel` 访问状态**：在组件中，你可以通过 `this.emailModel.invalid`、`this.emailModel.touched` 等访问表单控件的状态。

---

### ✅ 小结：

* `email.invalid`、`email.valid` 等状态属性是 `ngModel` 实例的属性，**在组件中直接访问是不行的**。
* 必须通过 `@ViewChild` 获取 `ngModel` 实例后，才能在组件中访问这些状态。


# 用法2：调用子组件类中的方法

---

```ts
// ✅ 子组件 child.component.ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-child',
  template: `
    <!-- 子组件模板内容 -->
    <p>我是子组件</p>
  `
})
export class ChildComponent {
  // 提供一个方法供父组件调用
  sayHello() {
    console.log('子组件方法 sayHello 被调用！');
    alert('👋 你好，我是子组件的方法！');
  }
}
```

---

```ts
// ✅ 父组件 parent.component.ts
import { Component, ViewChild } from '@angular/core';
import { ChildComponent } from './child.component';

@Component({
  selector: 'app-parent',
  template: `
    <h2>父组件</h2>

    <!-- 使用模板引用变量 #childRef 来引用子组件实例 -->
    <app-child #childRef></app-child>

    <!-- 点击按钮时，调用子组件的方法 -->
    <button (click)="childRef.sayHello()">调用子组件方法</button>
  `
})
export class ParentComponent {}
```

---

### ✅ 说明：

| 用法                           | 解释                               |
| ---------------------------- | -------------------------------- |
| `#childRef`                  | 是模板引用变量，指向 `<app-child>` 这个组件的实例 |
| `childRef.sayHello()`        | 直接调用子组件类中的方法                     |
| 不需要 `@Input()` / `@Output()` | 因为是直接拿子组件实例调用                    |

---

### ✅ 优缺点对比：

| 模板引用变量      | @ViewChild  | @Input/@Output |
| ----------- | ----------- | -------------- |
| 简单直接，用于模板内部 | 用于类中访问组件实例  | 推荐用于标准父子通信     |
| 模板中能立即调用方法  | 需要类中声明和生命周期 | 更解耦、清晰         |

---
# 用法3：访问表单的状态

`#myform="ngForm"` 是 Angular 中的一种模板引用变量的用法，通常用于访问 Angular 表单的状态，特别是在 **模板驱动表单** 中。它用于获取整个表单的 `NgForm` 实例，从而可以在模板中访问表单的验证状态、提交状态等。

### 解释：

* `#myform` 是模板引用变量的名称，它让你可以在模板中引用表单控件（`<form>` 标签）的实例。
* `ngForm` 是指令的名称，它是 Angular 的表单模块中提供的一个指令，用于管理表单的验证、提交状态等。`ngForm` 指令会自动为表单元素添加相关的状态信息。

通过 `#myform="ngForm"`，你可以在模板中访问 `NgForm` 实例的各种属性，例如表单是否有效、是否被提交过等。

### 示例代码：

```html
<form #myform="ngForm" (ngSubmit)="onSubmit(myform)">
  <label for="email">Email:</label>
  <input type="email" id="email" name="email" ngModel required />

  <button type="submit" [disabled]="myform.invalid">提交</button>
</form>

<p *ngIf="myform.submitted">表单已经提交！</p>
```

### 解释：

1. **`#myform="ngForm"`**：这是模板引用变量，它将表单绑定到 `myform` 变量，并使其具有 `ngForm` 指令的功能和属性。
2. **`myform.invalid`**：可以通过 `myform` 来访问表单的 `invalid` 属性，表示表单是否无效。如果表单中的控件不满足验证规则，`invalid` 为 `true`。
3. **`myform.submitted`**：这是表单的提交状态，当表单被提交时，`submitted` 属性会变为 `true`。
4. **`(ngSubmit)="onSubmit(myform)"`**：当表单提交时，会调用 `onSubmit` 方法，并传入 `myform`（表单实例），可以在方法中进一步处理表单数据或状态。

### 在组件中如何处理：

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  template: `
    <form #myform="ngForm" (ngSubmit)="onSubmit(myform)">
      <label for="email">Email:</label>
      <input type="email" id="email" name="email" ngModel required />

      <button type="submit" [disabled]="myform.invalid">提交</button>
    </form>

    <p *ngIf="myform.submitted">表单已经提交！</p>
  `
})
export class AppComponent {
  onSubmit(form: any) {
    console.log('提交的表单:', form);
    console.log('表单数据:', form.value);
  }
}
```

### 小结：

* `#myform="ngForm"` 让你在模板中通过 `myform` 引用整个表单的状态和验证信息。
* `ngForm` 是一个指令，它会自动添加到表单元素上，管理表单的验证状态、提交状态等。
