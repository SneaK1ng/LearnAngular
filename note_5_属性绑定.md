# ✅ 综合示例：属性绑定 + signal + null/undefined 行为
```ts
// 引入 Angular 的核心模块和 signal 响应式变量
import { Component, signal } from '@angular/core';

// 定义一个独立的组件（使用 Angular 17+ 的 standalone 模式）
@Component({
  selector: 'app-root',
  standalone: true,

  // 模板部分：展示属性绑定的各种情况
  template: `
    <!-- 属性绑定 src，值为 null，img 标签不会显示图片 -->
    <!-- 浏览器不会报错，但不会加载任何图片 -->
    <img [src]="nullImage" alt="null 图片" />

    <!-- 属性绑定 src，值为 undefined，同样不会显示图片 -->
    <img [src]="undefinedImage" alt="undefined 图片" />

    <!-- 属性绑定 signal 变量，使用 name() 取值 -->
    <input [value]="name()" />

    <!-- 属性绑定 signal 变量，动态控制 disabled -->
    <button [disabled]="isDisabled()">点击我</button>
  `
})
export class AppComponent {
  // null 值绑定给 src，img 不会加载图片
  nullImage = null;

  // undefined 值绑定给 src，img 不会加载图片
  undefinedImage = undefined;

  // signal 响应式变量，必须调用 name()
  name = signal('张三');

  // signal 控制按钮是否禁用
  isDisabled = signal(true);
}
```