# ✅ 1. 使用 @NgModule 的传统写法（Angular 旧方式）
```ts
// 引入组件装饰器，用来定义 Angular 组件
import { Component, NgModule } from '@angular/core';
// 引入浏览器模块，是 Web 应用启动的核心模块
import { BrowserModule } from '@angular/platform-browser';
// 引入 CommonModule，提供常用指令（如 *ngIf）
import { CommonModule } from '@angular/common';
// 引入启动函数，用于启动模块
import { platformBrowserDynamic } from '@angular/platform-browser-dynamic';

// 用 @Component 装饰器定义一个组件
@Component({
  // 组件选择器，在 HTML 中通过 <hello-comp> 使用
  selector: 'hello-comp',
  // 组件模板，显示一段文字
  template: `<p>Hello from NgModule</p>`
})
// 导出组件类
export class HelloComponent {}

// 用 @NgModule 装饰器定义一个模块
@NgModule({
  // 声明当前模块包含的组件
  declarations: [HelloComponent],
  // 导入其他 Angular 功能模块
  imports: [BrowserModule, CommonModule],
  // 指定启动时加载的组件
  bootstrap: [HelloComponent]
})
// 导出模块类
export class AppModule {}

// 启动 Angular 应用，传入模块 AppModule
platformBrowserDynamic().bootstrapModule(AppModule);

```
# ✅ 2. 使用 standalone 的现代写法（Angular 18 推荐）
```ts
// 引入组件装饰器，用于定义 Angular 独立组件
import { Component } from '@angular/core';
// 引入 CommonModule，提供 *ngIf、ngFor 等常用指令
import { CommonModule } from '@angular/common';
// 引入独立组件启动函数
import { bootstrapApplication } from '@angular/platform-browser';

// 用 @Component 装饰器定义一个 standalone 独立组件
@Component({
  // 组件选择器，在 HTML 中通过 <hello-comp> 使用
  selector: 'hello-comp',
  // 标记为独立组件，不需要声明到 NgModule 中
  standalone: true,
  // 组件内部需要用到的模块，自己声明
  imports: [CommonModule],
  // 组件模板，显示一段文字
  template: `<p>Hello from Standalone</p>`
})
// 导出组件类
export class HelloComponent {}

// 启动 Angular 应用，直接使用独立组件启动
bootstrapApplication(HelloComponent);

```

# ✅ 对比总结
| 项目               | `@NgModule` 写法               | `standalone` 写法                       |
| ---------------- | ---------------------------- | ------------------------------------- |
| 是否需要 NgModule    | ✅ 必须                         | ❌ 不需要                                 |
| 依赖组件怎么导入         | 在 NgModule 的 `imports` 中统一导入 | 在组件自身的 `imports` 中导入                  |
| 组件是否要在模块中声明      | ✅ 要在 `declarations` 中注册      | ❌ 不需要，自己就是独立的                         |
| 启动方式             | `bootstrapModule(AppModule)` | `bootstrapApplication(YourComponent)` |
| 推荐程度（Angular 18） | ✅ 旧项目兼容用                     | ✅ 官方推荐新项目使用                           |
