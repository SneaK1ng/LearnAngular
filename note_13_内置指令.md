# 下面是一个包含所有三种 Angular 指令（组件、属性型、结构型）的**完整示例**，
- ✅ 如果你需要 带模板的组件（页面内容、按钮、结构等），用 @Component。
- ✅ 如果你只想 改变已有元素的行为或样式（不需要模板），用 @Directive。
---
| 比较项   | @Component            | @Directive                  |
| ----- | --------------------- | --------------------------- |
| 是否带模板 | ✅ 有模板（`template`）     | ❌ 没有模板                      |
| 用途    | 创建页面组件（UI 结构）         | 修改现有元素的样式或行为（增强功能）          |
| 使用方式  | `<my-comp></my-comp>` | `<div appXXX></div>`（当成属性用） |
| 适合场景  | 页面结构、视图、完整 UI         | 简单功能增强：高亮、权限控制、显示隐藏等        |

---

## @Directive 是 Angular 提供的装饰器，用来定义一个指令（Directive），也就是可以应用到 HTML 元素上的行为扩展功能
```ts
// highlight.directive.ts（属性型指令）
// 改变元素的背景色
import { Directive, HostBinding } from '@angular/core';

@Directive({
  selector: '[appHighlight]',
  standalone: true,
})
export class HighlightDirective {
  // 设置元素的背景颜色
  @HostBinding('style.backgroundColor') bgColor = 'lightgreen';
}
```

```ts
// unless.directive.ts（结构型指令）
// 类似 *ngIf，但逻辑相反：当条件为 false 时显示元素
import { Directive, Input, TemplateRef, ViewContainerRef } from '@angular/core';

@Directive({
  selector: '[appUnless]',
  standalone: true,
})
export class UnlessDirective {
  constructor(private tpl: TemplateRef<any>, private vc: ViewContainerRef) {}

  @Input() set appUnless(condition: boolean) {
    // 当条件为 false 时才显示元素
    if (!condition) {
      this.vc.createEmbeddedView(this.tpl);
    } else {
      this.vc.clear();
    }
  }
}
```

```ts
// child.component.ts（组件指令）
// 一个有输入输出的自定义组件
import { Component, Input, Output, EventEmitter } from '@angular/core';

@Component({
  selector: 'app-child',
  standalone: true,
  template: `
    <!-- 接收来自父组件的数据 -->
    <p>来自父组件的消息：{{ msg }}</p>
    <!-- 点击按钮向父组件发送消息 -->
    <button (click)="send()">回复父组件</button>
  `,
})
export class ChildComponent {
  @Input() msg = '';
  @Output() reply = new EventEmitter<string>();

  send() {
    this.reply.emit('子组件：已收到并回复！');
  }
}
```

```ts
// app.component.ts（根组件）
// 演示三种指令用法：组件、属性型、结构型
import { Component, signal } from '@angular/core';
import { HighlightDirective } from './highlight.directive';
import { UnlessDirective } from './unless.directive';
import { ChildComponent } from './child.component';

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [HighlightDirective, UnlessDirective, ChildComponent],
  template: `
    <!-- 组件指令：使用子组件 -->
    <app-child [msg]="message()" (reply)="onReply($event)" />

    <!-- 属性型指令：添加背景色 -->
    <p appHighlight>这段文字被高亮了</p>

    <!-- 属性型指令：原生 HTML 属性绑定 -->
    <button [disabled]="isDisabled()">按钮被禁用</button>

    <!-- 结构型指令：ngIf -->
    <p *ngIf="isVisible()">只有在 isVisible 为 true 时才显示</p>

    <!-- 自定义结构型指令：appUnless（当为 false 时显示） -->
    <p *appUnless="isVisible()">只有在 isVisible 为 false 时才显示</p>

    <!-- 切换按钮 -->
    <button (click)="toggle()">切换显示状态</button>

    <!-- 显示来自子组件的消息 -->
    <p>{{ received() }}</p>
  `,
})
export class AppComponent {
  isVisible = signal(true);
  isDisabled = signal(false);
  message = signal('你好，子组件！');
  received = signal('');

  toggle() {
    this.isVisible.update(v => !v);
  }

  onReply(msg: string) {
    this.received.set(msg);
  }
}
```

---

### 涵盖内容总结：

| 指令类型      | 示例内容                        |
| --------- | --------------------------- |
| **组件指令**  | `<app-child>` 子组件使用         |
| **属性型指令** | `[disabled]`、`appHighlight` |
| **结构型指令** | `*ngIf`、自定义 `*appUnless` 指令 |

这样你可以清晰地看到三种指令如何实际使用在模板中。如需我再添加一些内置属性型指令（如 `ngClass`, `ngStyle`）的例子也可以。是否需要？
