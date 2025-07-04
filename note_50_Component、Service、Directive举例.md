下面示例展示了三个场景，分别用 Component、Service、Directive：

```ts
// ---------- Component 示例：渲染用户列表 ----------
// 当你需要一个「视图+交互」的单元时，使用 Component
@Component({
  selector: 'app-user-list',
  // 模板里展示 users 数组，并响应用户点击等交互
  template: `
    <ul>
      <li *ngFor="let user of users">{{ user }}</li>
    </ul>
  `
})
export class UserListComponent implements OnInit {
  users: string[] = [];             // 组件状态：要渲染的数据

  constructor(private userService: UserService) {}  // 注入 Service

  ngOnInit() {
    // 组件生命周期钩子中调用服务获取数据
    this.userService.getUsers().subscribe(data => {
      this.users = data;
    });
  }
}


// ---------- Service 示例：封装数据获取逻辑 ----------
// 当你需要「与视图无关」的业务逻辑或数据操作时，使用 Service
@Injectable({ providedIn: 'root' })
export class UserService {
  constructor(private http: HttpClient) {} // 注入 HttpClient

  // 提供给组件调用的方法：从后端拉取用户列表
  getUsers(): Observable<string[]> {
    return this.http.get<string[]>('/api/users');
  }
}


// ---------- Directive 示例：高亮文本指令 ----------
// 当你需要给已有元素「复用地」添加样式或行为时，使用 Directive
@Directive({ selector: '[appHighlight]' })
export class HighlightDirective implements OnChanges {
  @Input('appHighlight') color = 'yellow';  // 输入高亮颜色

  constructor(private el: ElementRef) {}      // 元素引用

  ngOnChanges() {
    // 每次输入变化时，更新宿主元素背景色
    this.el.nativeElement.style.backgroundColor = this.color;
  }
}

/*
模板中使用：
<app-user-list></app-user-list>
<p [appHighlight]="'lightblue'">这段文字背景是浅蓝色</p>
*/
```
