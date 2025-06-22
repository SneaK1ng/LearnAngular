在 JavaScript 和 Angular 中，可以使用 `event.preventDefault()` 来阻止默认行为，使用 `event.stopPropagation()` 来阻止事件冒泡。

### **1. 阻止默认行为**

默认行为是指浏览器为特定事件自动执行的操作。例如，点击一个链接时，浏览器会导航到链接指向的 URL，提交表单时，浏览器会刷新页面等。

#### **`event.preventDefault()`** 用法：

* 阻止元素执行默认的行为（如提交表单、点击链接等）。

#### **例子：阻止表单提交的默认行为**

```html
<form (submit)="onSubmit($event)">
  <button type="submit">提交</button>
</form>
```

```typescript
export class AppComponent {
  onSubmit(event: Event) {
    event.preventDefault();  // 阻止表单的默认提交行为
    console.log('表单提交被阻止');
  }
}
```

* 当点击“提交”按钮时，`event.preventDefault()` 会阻止表单的默认提交行为，页面不会重新加载。

---

### **2. 阻止事件冒泡**

事件冒泡是指事件从目标元素触发后，沿着 DOM 树向上传播的过程。例如，点击一个嵌套的按钮，事件会先触发按钮的 `click` 事件，然后再触发它的父元素的 `click` 事件。

#### **`event.stopPropagation()`** 用法：

* 阻止事件向上传播，避免父元素的事件处理程序被触发。

#### **例子：阻止事件冒泡**

```html
<div (click)="onDivClick()">
  <button (click)="onButtonClick($event)">点击我</button>
</div>
```

```typescript
export class AppComponent {
  onDivClick() {
    console.log('Div 被点击');
  }

  onButtonClick(event: Event) {
    event.stopPropagation();  // 阻止事件冒泡
    console.log('Button 被点击');
  }
}
```

* 当点击按钮时，`event.stopPropagation()` 会阻止事件冒泡到父元素 `div`，因此 `Div 被点击` 的日志不会出现，只有 `Button 被点击` 会被打印。

---

### **3. 同时阻止默认行为和事件冒泡**

你可以同时使用 `event.preventDefault()` 和 `event.stopPropagation()` 来阻止默认行为和事件冒泡。

#### **例子：阻止默认行为和冒泡**

```html
<button (click)="onButtonClick($event)">点击我</button>
```

```typescript
export class AppComponent {
  onButtonClick(event: Event) {
    event.preventDefault();      // 阻止默认行为
    event.stopPropagation();     // 阻止事件冒泡
    console.log('按钮点击事件被处理');
  }
}
```

* 在这个例子中，点击按钮后，默认行为（如表单提交、链接跳转等）和事件冒泡都会被阻止。

---

### **总结**

* **`event.preventDefault()`**：阻止事件的默认行为（如表单提交、链接跳转等）。
* **`event.stopPropagation()`**：阻止事件冒泡，防止父元素的事件处理函数被触发。
