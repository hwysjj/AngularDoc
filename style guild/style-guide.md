# Style Guide

**代码风格需要严格保持一致，不要轻易打破代码风格指南，除非你完全理解这套指南并且有足够充分的理由。**

## 1. 单一简单原则
### 规则1
- 一个文件只做一件事情，比如service, component
- 每个文件不要超过400行代码

### 规则2
- 定义小函数
- 一个函数不要超过75行

## 2. 命名规范
### 规则1
- 命名规规范需要一致
- 文件名格式需要一致，先功能，然后是类型，格式是feature.type.ts。 e.g. hero.component.ts
- 文件或者文件名应该简单明了，能够直观的传达文件或者文件夹的用途

### 规则2
- 描述性的名字用“-”隔开
- 描述性名字和类型之间用“.”隔开
- 用正常的类型名字, 不要创建太多，通常的有 .service, .component, .pipe, .module 和 .directive

### 3. 符号名和文件名
- 符号名
    ```javascript
    @Component({ ... })
    export class AppComponent { }
    ```
- 文件名 `app.component.ts`
- 用大驼峰规则来定义类。
- 符号名后边通常加对应的类型（比如Component, Directive, Module, Pipe或者Service）

### 4. Service命名
- 有些词汇一看就知道是Service的，比如以“er”结尾的，***Logger*** 可能比 ***LoggerService*** 好一点。
    ```javascript
    @Injectable()
    export class HeroDataService { }
    ```
    ```javascript
    @Injectable()
    export class Logger { }
    ```

### 5. Bootstrapping
- bootstrapping和platform的逻辑放到main.ts
- 在bootstrapping的逻辑里边要加出错处理
    ```javascript
    platformBrowserDynamic().bootstrapModule(AppModule)
        .then(success => console.log(`Bootstrap success`))
        .catch(err => console.error(err));
    ```

### 6. Component选择器
- 用中线命名法或者是烤串命名法来命名selector的，小写命名
    ```javascript
    @Component({
        selector: 'toh-hero-button',
        templateUrl: './hero-button.component.html'
    })
    export class HeroButtonComponent {}
    ```
- selector的命名需要加上前缀，避免与html标签或者引用该组件的其他app冲突

### 7. Directive选择器
- 用小驼峰命名法
- 加上前缀避免命名冲突
    ```javascript
    @Directive({
        selector: '[tohValidate]'
    })
    export class ValidateDirective {}
    ```

### 8. 单元测试
- 文件名需要跟测试的文件名保持一致，后缀.spec，例如 `heroes.component.spec.ts`

### 9. E2E测试
- 需要与功能名保持一致，后缀.e2e-spec, 例如 `heroes.e2e-spec.ts`

### 10. NgModule
- 符号名为功能名，后缀Module
- RoutingModule的后缀-routing.module.ts

### 11. 常量
- 坚持用const声明变量，除非他们的值在应用的生命周期内会发生变化
- 小驼峰命名， 例如heroRoutes，而不是大写蛇形命名法HERO_ROUTES
- 不需要修改现存的使用大写蛇形命名法的常量

### 12. 接口
- 坚持使用大驼峰
- 考虑不要使用I作为接口的前缀
- 考虑在服务和可声明对象（组件、指令和管道）中用类代替接口

### 13. 属性和方法
- 坚持使用小驼峰命名法
- 不要给private属性或者方法名加前缀_

### 14. 导入空行
- 考虑在三方import和应用import之间加空行
- 考虑按照模块名字字母顺序import
- 考虑在解构表达式{}中按字母顺序排列import
```javascript
import { HttpClient } from '@angular/common/http';
import { Injectable } from '@angular/core';

import { ExceptionService, SpinnerService, ToastService } from '../../core';
import { Hero } from './hero.model';
```

### 15. 应用程序结构和NgModules
- 所有的源代码放到src目录下，所有的feature放到自己单独的文件夹下并带上自己的NgModule
- 三方的程序库不要放在src下，因为不需要改它。

### 16. 扁平化
- 如果一个文件夹下有七个文件以上，创建子文件夹
- 考虑配置IDE, 隐藏无关紧要的文件，例如生成出来的.js和.js.map文件

### 17. App root module
- 坚持在app根目录下创建一个NgModule, 例如/src/app
- 考虑命名root module 为 app.module.ts

### 18. Feature modules
- 坚持为所有不同的feature创建一个NgModule, 这样可以控制是否暴露给其他模块，便于澄清开发职责，易于隔离测试
- 考虑在服务整个应用的shared文件夹下定义SharedModule
- 不要在SharedModule中提供服务，因为服务是单例的。
- 在SharedModule中declare, export, import所有的东西

### 内联输入和输出属性装饰器
- 坚持 使用 @Input() 和 @Output()，而非 @Directive 和 @Component 装饰器的 inputs 和 outputs 属性
- 考虑把 @Input() 或者 @Output() 放到所装饰的属性的同一行。
- 避免除非有重要目的，否则不要为输入和输出指定别名。
- 不要给输出属性加前缀
- 坚持命名事件时，不要带前缀 on。
- 坚持把事件处理器方法命名为 on 前缀之后紧跟着事件名。
```javascript
@Component({
    selector: 'toh-hero-button',
    template: `<button>{{label}}</button>`
})
export class HeroButtonComponent {
    @Output() change = new EventEmitter<any>();
    @Input() label: string;
}
```
```javascript
export class HeroComponent {
  @Output() savedTheDay = new EventEmitter<boolean>();
}

<toh-hero (savedTheDay)="onSavedTheDay($event)"></toh-hero>
```

### 成员顺序
- 坚持把属性成员放在前面， 方法成员放在后边
- 先放公共成员，再放私有成员，并按照字母顺序排列

### 逻辑放到服务里
- Component里只放与视图相关的逻辑。其他所有逻辑放到服务里

### 把表现层逻辑放到组件类里
- 坚持把表现层逻辑放进组件类中，而不要放在模板里。

### HostListener 和 HostBinding 装饰器 vs. 组件元数据 host
- 考虑优先使用 @HostListener 和 @HostBinding，而不是 @Directive 和 @Component 装饰器的 host 属性。

### 使用 @Injectable() 类装饰器
- 坚持当使用类型作为令牌来注入服务的依赖时，使用 @Injectable() 类装饰器，而非 @Inject() 参数装饰器。
```javascript
@Injectable()
export class HeroArena {
  constructor(
    private heroService: HeroService,
    private http: Http) {}
}
```

### 数据服务
- 坚持把数据操作和与数据交互的逻辑重构到服务里。
- 坚持让数据服务来负责 XHR 调用、本地储存、内存储存或者其它数据操作。
