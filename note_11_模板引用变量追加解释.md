## 🎯 什么是模板引用变量（Template Reference Variable）？

**模板引用变量**是你在 HTML 模板中，给某个元素、组件或指令起的“临时名字”，可以在模板中引用它的属性或方法。

```html
<input #myInput />
<button (click)="doSomething(myInput.value)">Click</button>
```

🧠 它**不会出现在 TypeScript 文件中**，仅限在当前模板中使用。

---

## ✍️ 定义语法

```html
<tag #变量名></tag>
```

---

## ✅ 能引用哪些东西？

| 可引用对象          | 示例                           |
| -------------- | ---------------------------- |
| 原生 DOM 元素      | `<input #emailInput>`        |
| 子组件实例          | `<app-child #childRef>`      |
| 指令实例（如 ngForm） | `<form #loginForm="ngForm">` |

---

## 📘 示例 1：获取输入框的值

```html
<input #username />
<button (click)="greet(username.value)">Greet</button>
```

```ts
greet(name: string) {
  console.log(`Hello, ${name}`);
}
```

---

## 📘 示例 2：调用子组件方法

### 子组件 `<app-counter>`

```ts
@Component({
  selector: 'app-counter',
  template: `<p>{{ count }}</p>`
})
export class CounterComponent {
  count = 0;
  reset() {
    this.count = 0;
  }
}
```

### 父组件模板

```html
<app-counter #counterComp></app-counter>
<button (click)="counterComp.reset()">重置计数器</button>
```

---

## 📘 示例 3：结合 Angular 指令（如 ngForm）

```html
<form #userForm="ngForm" (ngSubmit)="submit(userForm)">
  <input name="email" ngModel required />
  <button type="submit">提交</button>
</form>
```

```ts
submit(form: NgForm) {
  console.log(form.value);
}
```

* `#userForm="ngForm"` 是绑定到 `ngForm` 指令的导出名

---

## 📘 示例 4：获取 DOM 元素对象（例如改变焦点）

```html
<input #nameInput />
<button (click)="nameInput.focus()">Focus Input</button>
```

---

## 💡 作用范围

* **模板内有效**：不能在组件的 TypeScript 中直接访问
* **无需声明变量或查询 DOM**
* **不影响逻辑层封装**

---

## ✅ 优点总结

✔️ 简单易用
✔️ 无需在组件中写 `@ViewChild`
✔️ 可引用 DOM、子组件、指令
✔️ 支持与事件绑定搭配使用

---

## 📌 补充小技巧

你可以给一个元素多个绑定，例如：

```html
<input #inputRef (keyup.enter)="log(inputRef.value)" />
```

也可以传给管道、结构指令：

```html
<div *ngIf="inputRef.value.length > 0">You typed something</div>
```

---

## 🧠 记忆口诀

> “模板里给个名，#号前缀来命名；只在 HTML 中用，用来引用真方便！”

---

如果你需要，我可以给你做一个 **完整可运行的小例子**，展示多种引用方式一起用。要来一个吗？💻✨
