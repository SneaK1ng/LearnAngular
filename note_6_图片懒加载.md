# ✅ 图片懒加载
## 1. loading="lazy" 写法
```ts
import { Component, signal } from '@angular/core';

@Component({
  selector: 'app-image-switch',
  standalone: true,
  template: `
    <!-- 图片src是动态值 -->
    <img
      [src]="currentSrc()"
      width="600"
      height="400"
      alt="图片"
      loading="lazy"
      (load)="onLoad()"
      (error)="onError()"
    />
  `
})
export class ImageSwitchComponent {
  // 占位图（默认显示）
  placeholder = 'assets/placeholder.jpg';
  // 加载失败图（备用）
  errorImage = 'assets/error.jpg';
  // 正式图片
  realImage = 'https://example.com/image.png';

  // 当前图片地址（初始为占位图）
  currentSrc = signal(this.placeholder);
  // 图片加载完成，切换为正式图
  onLoad() {
    if (this.currentSrc() === this.placeholder) {
      this.currentSrc.set(this.realImage);
    }
  }

  // 加载失败，切换为错误图
  onError() {
    this.currentSrc.set(this.errorImage);
  }
}
```
## 2. IntersectionObserver写法
```ts
import { Component, ElementRef, OnInit, ViewChild } from '@angular/core';

@Component({
  selector: 'app-image-lazy-load',
  template: `
    <div class="image-container">
      <img
        #lazyImage
        [src]="imageSrc"
        [alt]="altText"
        width="600"
        height="400"
        loading="lazy"
        class="lazy-load"
      />
    </div>
  `,
  styles: [
    `
      .lazy-load {
        opacity: 0;
        transition: opacity 1s;
      }
      .lazy-load.loaded {
        opacity: 1;
      }
    `,
  ],
})
export class ImageLazyLoadComponent implements OnInit {
  @ViewChild('lazyImage') lazyImage: ElementRef | undefined;

  imageSrc: string = 'https://example.com/image.png'; // 正式图片路径
  altText: string = '懒加载图片'; // 图片的alt文本

  private observer: IntersectionObserver | undefined;

  ngOnInit() {
    // 创建 IntersectionObserver 实例
    this.observer = new IntersectionObserver(
      (entries) => {
        entries.forEach((entry) => {
          if (entry.isIntersecting) {
            // 进入视口时，添加 `loaded` 类，确保图片显示
            this.lazyImage?.nativeElement.classList.add('loaded');
            this.observer?.disconnect(); // 停止观察
          }
        });
      },
      {
        rootMargin: '0px',
        threshold: 0.1, // 10%的图片进入视口时触发
      }
    );
  }

  ngAfterViewInit() {
    if (this.lazyImage) {
      // 开始观察图片
      this.observer?.observe(this.lazyImage.nativeElement);
    }
  }
}
```