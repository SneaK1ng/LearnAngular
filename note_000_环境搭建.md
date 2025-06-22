# 环境搭建

## 1. 创建项目:
```bash
npm install -g @angular/cli
ng new 项目名称 --skip-tests
cd 项目名称
ng serve --open
node.js
```
## 2. 升级：重新安装
```bash
node -v
npm cache clean -f（清除缓存）
```
## 3. 启动项目
```bash
git clone url
# npm install
npm i
# npm run 是 npm（Node.js 的包管理工具）中的一个命令，
# 用于执行在 package.json 文件中定义的 scripts 部分的命令
# 运行 ng serve 命令来启动 Angular 开发服务器
npm run start
# 当你准备好将项目发布到生产环境时，运行此命令进行生产构建
# 会生成优化过的、压缩的文件，以便部署到生产环境。默认会将构建文件输出到 `dist/` 目录
npm run build
# 以开发模式持续监视项目文件变化并自动构建，默认是以开发模式构建（不进行优化、压缩）
npm run watch
# 运行单元测试
```
## 4. 项目配置
```json
// package.json
{
  "name": "entitlement-system",
  "version": "0.0.0",
  "scripts": {
    // 启动 Angular 开发服务器
    // port 3000：指定开发服务器运行在 3000 端口。
    // host 0.0.0.0：允许局域网中的设备访问你的开发服务器。
    // open：在启动开发服务器时自动打开浏览器。
    "start": "ng serve --port 3000 --host 0.0.0.0 --open",
    // 构建项目
    "build": "ng build",
    // 监视文件变化并以开发模式构建
    "watch": "ng build --watch --configuration development",
    // 运行单元测试
    "test": "ng test"
  },
  // 不允许发布到 npm，保护私有项目
  "private": true,
  // 项目运行时需要的依赖
  "dependencies": {
    // Angular 动画支持
    "@angular/animations": "^18.2.0",
    // 公共功能模块（如常用指令、管道等）
    "@angular/common": "^18.2.0",
    // 模板编译器
    "@angular/compiler": "^18.2.0",
    // Angular 核心库
    "@angular/core": "^18.2.0",
    // 表单支持（响应式和模板驱动）
    "@angular/forms": "^18.2.0",
    // 浏览器平台支持
    "@angular/platform-browser": "^18.2.0",
    // 动态编译支持
    "@angular/platform-browser-dynamic": "^18.2.0",
    // 路由功能
    "@angular/router": "^18.2.0",
    // Ant Design 图标（Angular 版本）
    "@ant-design/icons-angular": "^18.0.0",
    // UI 组件库 ng-zorro（Ant Design Angular 实现）
    "ng-zorro-antd": "^18.1.1",
    // 响应式编程库（Observable 等）
    "rxjs": "~7.8.0",
    // TypeScript 的运行时库，减小打包体积
    "tslib": "^2.3.0",
    // Angular 必需库，用于异步上下文跟踪
    "zone.js": "~0.14.10"
  },
  // 开发和测试阶段使用的依赖
  "devDependencies": {
    // Angular 构建工具
    "@angular-devkit/build-angular": "^18.2.8",
    // Angular CLI 命令工具
    "@angular/cli": "^18.2.8",
    // 用于 AOT 编译的 Angular 编译器
    "@angular/compiler-cli": "^18.2.0",
    // jasmine 的类型定义
    "@types/jasmine": "~5.1.0",
    // jasmine 核心库（测试框架）
    "jasmine-core": "~5.2.0",
    // karma 测试运行器
    "karma": "~6.4.0",
    // 启动 Chrome 浏览器进行测试
    "karma-chrome-launcher": "~3.2.0",
    // 生成测试覆盖率报告
    "karma-coverage": "~2.2.0",
    // karma 的 jasmine 插件
    "karma-jasmine": "~5.1.0",
    // 在浏览器中展示测试结果
    "karma-jasmine-html-reporter": "~2.1.0",
    // TypeScript 编译器
    "typescript": "~5.5.2"
  }
}
```
##