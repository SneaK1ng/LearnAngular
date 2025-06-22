# ✅ 插值语法
```ts
// 引入 Angular 核心功能，包括组件和 signal
import { Component, signal } from '@angular/core';

// 使用 @Component 装饰器定义组件
@Component({
  // 组件的选择器（HTML 中的标签名）
  selector: 'app-root',

  // 模板部分，使用各种插值方式展示数据
  template: `
    <!-- 显示字符串变量 -->
    <h1>{{ title }}</h1>

    <!-- 显示嵌套对象的属性 -->
    <p>用户名：{{ user.name }}</p>
    <p>年龄：{{ user.age }}</p>

    <!-- 显示 signal 对象属性（调用函数） -->
    <p>signal 中的状态：{{ status().state }}</p>

    <!-- 表达式插值：数字加法 -->
    <p>年龄+5年后：{{ user.age + 5 }}</p>

    <!-- 表达式插值：三元表达式 -->
    <p>是否成年：{{ user.age >= 18 ? '是' : '否' }}</p>

    <!-- 调用组件中的方法 -->
    <p>问候语：{{ greetUser() }}</p>
  `
})
// 定义组件类
export class AppComponent {
  // 字符串变量
  title = '用户信息展示';

  // 普通对象：插值中可以访问其属性
  user = {
    name: '张三',
    age: 20
  };

  // signal 响应式变量，返回对象，需要加 ()
  status = signal({ state: '活跃中' });

  // 方法：用于返回一个字符串，用在插值中
  greetUser(): string {
    return `你好，${this.user.name}！`;
  }
}
```