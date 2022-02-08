---
layout: post
title: "Angular学习笔记"
date: 2021-01-27 
description: "Angular is an application design framework and development platform for creating efficient and sophisticated single-page apps."
tag: Angular
---   
<!--# ANGULARSTUDY-->
<strong>Angular核心概念</strong><br>
<i>第一，模块</i><br>
- 不同于Node.js或ES6中的模块的模块，NG的模块就是一个抽象的容器，用于对组件进行分组<br>
整个应用初始时只有一个主模块：AppModule
  
<i>第二，组件</i><br>
- 是一段可以反复使用的页面片段，如页头、轮播、手风琴
- 组件(Component)=模板(Template)+脚本(Script)+样式(Style)
  
<i>第三，数据绑定</i><br>
- HTML绑定：{{ NG表达式 }}
- 属性绑定
- 指令绑定
- 事件绑定
- 双向数据绑定

<i>第四，过滤器</i><br>
- Filter：过滤器，用于在View中呈现数据时显示为另一种格式；过滤器的本质是一个函数，接收原始数据转换为新的格式进行：`function(oldVal){  ...  return newVal}`<br>
- 使用过滤器：{{e.salay | 过滤器名(也称为管道)}}

<i>第五，服务和依赖注入 --抽象和重点</i><br>
- Service:服务，Angular认为：组件是与用户交互的一种对象，其中的内容都是应该与用户操作有关系的；而与用户无关的内容都应该剥离出去，放在“服务对象”中，为组件服务，例如：日志记录、计时统计、数据服务器的访问...
<br>

##  1. <a name=''></a>创建一个自定义组件
###  1.1. <a name='Classbook.component.ts'></a>创建组件Class ，自定义一个文件如`book.component.ts`
```typescript
    //装饰器声明这是一个组件
    @Component({
       selector:'app-book', //被调用时名称
       template:'<h2>我的组件</h2>'  
       //页面设计内容如：<h2>我的组件</h2>
       //templateUrl:'./book.component.html' 将组件内容写在html文件中
       //styleUrls:['./XX.css','./XXXXX.css'] 可以引入多个样式css文件
    })

    //导出组件
    export class bookComponent{}
```
###  1.2. <a name='classbook.module.ts'></a>在某个模块中注册组件class，模块文件如：`book.module.ts`中
```typescript
    @Ngmodule({
        declarations:'bookComponent', //export class bookComponent
    })
    //声明这个组件
```
###  1.3. <a name='book.component.htmlapp-book...XX..app-bookbr'></a>使用已经注册过的组件，如：在`book.component.html`中使用`<app-book>...XX..</app-book>`<br>
    使用之后页面会出现标题二：我的组件
<br>

###  1.4. <a name='colorredAngularCLIbr'></a>$\color{red}{Angular CLI 提供快速创建组件的工具}$ <br>
   `ng generate component 组件名` or <br>
   `ng g component 组件名`

##  2. <a name='-1'></a>数据绑定
###  2.1. <a name='-1'></a>简单的绑定
####  2.1.1. <a name='-1'></a>初始化内容
```typescript
        //在`book.component.ts`中
       export class bookComponent{
           uname = Zhang;
           age = 22;
       }
        //在`book.component.html`中引用数据
       <div>{{uname}}</div>
       <div>{{age}}</div>
       //运行结果，界面内容：Zhang，22
```
####  2.1.2. <a name='colorredHTMLbr'></a>$\color{red}{HTML绑定}$<br>
- 算术运算：`{{age+2}}`<br>
- 比较运算、逻辑运算: `{{age>18 && age<30}}`<br>
- 三目运算：`{{age>=18 ? '成年':'未成年'}}`<br>
- 调用函数运算：`{{uname.length}} 、{{uname.toUpperCase()}}`<br>
    * 不支持创建对象、JSON序列化
####  2.1.3. <a name='colorredbr'></a>$\color{red}{属性绑定}$<br>
- 形式1：直接在属性上用{{}} `<p title="{{uname}}">这是一个属性</p>`<br>
- 形式2：直接使用[]做属性绑定`<p [title]="uname">这是一个属性</p>`<br>
    `<p [title]=" '当前年龄：' + age" `变量和常量结合时要用英文''和+才行，将常量变成字符串<br>
####  2.1.4. <a name='colorredbr-1'></a>$\color{red}{事件绑定}$<br> 
```typescript
    //在html文件中添加button按钮实现事件
       <button (click)="printUname()">输出用户名</button>
    //在component.ts文件class中添加事件函数
        export class bookComponent{
            //class内部成员属性
            uname = 'Zhang';
            age = 22;

            //class内部成员方法,不需要返回类型
            printUname(){
                console.log(this.uname)
            }
        }
```
###  2.2. <a name='Angular'></a>Angular中的指令系统
####  2.2.1. <a name='Angular:'></a>Angular中的指令分为三类:
- 组件指令：NG中Component继承自Directive
- 结构型指令：会影响DOM树结构，必须使用 * 开头，如*ngFOr、*ngIf
- 属性型指令：不会影响DOM树结构，只会影响元素外观或行为，必须用 [] 括起来，如[ngClass]、[ngStyle]
####  2.2.2. <a name='ngForofbr'></a>循环绑定：*ngForof<br>
   `<ANY *ngFor="let 临时变量 of 数据></ANY>`
```typescript
    //1.在.module.ts文件中添加一个数据组如
       empList = ["xx","yy","zz"];

    //2.在.html文件中添加引用
    //方法一：
       <ul>
           <li *ngFor="let e of empList">{{e}}</li>
       </ul>
    //方法二：
       <ul>
           <li *ngFor="let e of empList;let i=index">{{i}}-{{e}}</li>
       </ul>
    //方法三:
       <ul>
           <li *ngFor="let e of empList;index as i">{{i}}-{{e}}</li>
       </ul>
```
####  2.2.3. <a name='ngIfbr'></a>选择绑定：*ngIf<br>
   `<ANY *ngIf="布尔表达式"></ANY>`<br>
   or`<ANY *ngIf="condition;then thenBlock else elseBlock"></ANY>`
```typescript
      isPayingUser=false;

      //简单的选择
      <div *ngIf="isPayingUser">
          此区域内容仅付费用户可见
      </div>

      //带有then和else的选择
      <div *ngIf="isPayingUser; then theBlock else elseBlock"></div>
      <ng-template #thenBlock>已付款成功,成为会员！！！</ng-template>
      <ng-template #elseBlock>请先成为会员！！！</ng-template>
```
####  2.2.4. <a name='ngStylebr'></a>样式绑定：[ngStyle]<br>
<b>说明ngStyle绑定的值必须是一个对象，对象属性就是CSS样式</b><br>
`<ANY [ngStyle]="StyleName"></ANY>`

```typescript
    //在.html文件中添加引用
    <button [ngStyle]="myStyleObj" (click)="loadMore()">加载更多内容</button>

    //在.module.ts文件中添加样式对象
    myStyleObj={
        backgroundColor:'green',
        color:'#fff',
        borderColor:'#252'
    }
    loadMore(){ //点击后更改按钮颜色
        this.myStyleObj.backgroundColor="red"
        this.myStyleObj.color="black"
    }
```
####  2.2.5. <a name='ngClassbr'></a>样式绑定：[ngClass]<br>
   <b>说明：ngClass绑定的值必须是一个对象，对象属性就是CSS Class名，属性值为true/false，true时class出现，否则class不出现</b><br>
   `<ANY [ngStyle]="CSSClassName"></ANY>`

```typescript
   //在.html文件中添加引用
    <button [ngStyle]="myClassObj" (click)="loadMore2()">加载更多内容</button>

    //在.module.ts文件中添加样式对象
    myClassObj={
        btn: true,
        'btn-danger': false, //烤串法则
        'btn-success':ture  
    }
    loadMore2(){ //点击更改按钮颜色
        this.myClassObj["btn-danger"]=true
        this.myClassObj["btn-success"]=true
    }
```
####  2.2.6. <a name=':ngSwitchbr'></a>了解：特殊选择绑定:[ngSwitch]<br>
```typescript
    <ANY [ngSwitch]="表达式">
        <ANY *ngSwitchCase="值1"></ANY>
        <ANY *ngSwitchCase="值2"></ANY>
        ....
        <ANY *ngSwitchDefault></ANY>
    </ANY>
```
####  2.2.7. <a name='colorredngModulebr'></a>$\color{red}{双向数据绑定指令：[(ngModule)]  — 重点}$ <br>
方向1：Module => View，模型变则视图变<br>
方向2：View => Moudule，视图(表单元素)变则模型变<br>
`<input/select/textarea [(ngModel)]="uname">`<br>
    - 注意：ngModule指令不在CommonModule模块中，而在FormsModule中，使用之前必须在主模块中导入该模块;(ngModelChange)实时监听绑定内容是否改变
```typescript
    //app.module.ts导入FormsModule
    import { FormsModule } from '@angular/forms';

    @NgModule({
        ...others content...

        imports:[BrowserModule,
                FormsModule]
    })

    //.html引用
    <input [(ngModel)]="uname" placeholder="请输入用户名" (ngModelChange)="unameChange()">
    <p>当前用户名：{{uname}}</p>

    //.component.ts文件中添加函数
    unameChange(){//实时监听uname是否改变
        console.log(this.uname);
    }
```
####  2.2.8. <a name='br'></a>扩展知识：如何自定义指令<br>
创建指令的简单工具：`ng g directive 指令名`；自定义指令都是作为元素属性来使用的，selector应该是:[指令名]<br>
    `<ANY 指令名>...</ANY>`
```typescript
    //.Directive.ts文件中
    export class DirectDirective {
        //构造方法
        constructor(e:elementRef) {
           // console.log("ewr")
           e.nativeElement.style.color='#833'
           e.nativeElement.style.background='#123'
         }
    }
    
    //.html文件中引用
    <p appDirect>...</p>
```
##  3. <a name='-1'></a>自定义管道（过滤器）
- 创建管道class，实现转换功能
- 在模块中注册管道
- 在模板视图中使用管道
- 可以使用CLI命令行`ng g pipe 管道名`<br>
<div align="center" >
<img src="/images/Common_pipe.png" width="350" height="350"/>
</div>

```typescript
    //1.创建一个sex.ts文件，文件内容如下：
    import { Pipe } from "@angular/core";
    @Pipe({
        name:'sex' //过滤器名\管道名
    })
    export class SexPipe{
        //管道到执行过滤任务的是一个固定的函数
        transform(val:Number,lang:string){
            if(lang=='zh'){ //可以设置其他内容如中英文显示
                if(val==1){
                    return '男'
                }else if(val=1){
                    return '女'
                }else{
                    return '未知'
                }
            } 
        }
    }

    //2.模块中注册
    //3.在.html文件中两种引用
    <p> {{e.sex | sex }} </p>
    <p [title]="empSex | sex"></p>
```
###  3.1. <a name='br-1'></a>练习：<br>
####  3.1.1. <a name='-1'></a>实现添加、删除事件
```typescript
    //在.component.ts文件中
    todolist=['Study','Work','Play']
    accidence=''

    doDelete(index: number){
        console.log(index)
        //删除指定数组下标元素
        this.todolist.splice(index,1)
    }
    addAccidence(){
        //console.log(this.accidence)
        this.todolist.push(this.accidence)
        this.accidence=''
    }

    //在HTML文件中
    <p *ngIf="todolist.length==0">This is no accidence</p> 
    <input [(ngModel)]="accidence" placeholder="please add a accidence">
    <button (click)="addAccidence()"> Add </button>
    <ul>
        <li *ngFor="let items of todolist;let i=index">
            {{i+1}}-{{items}}
            <button (click)="doDelete(i)">
                Delete
            </button>
        </li>
    </ul> 
```
####  3.1.2. <a name='br-1'></a>创建员工列表数组<br>
```typescript
    //在.component.ts文件添加内容和函数
    emplist=  [{eid:101,ename:"Zhang",sex:1,salay:5000},
                {eid:102,ename:"zheng",sex:0,salay:5100},
                {eid:103,ename:"Wang",sex:1,salay:5050}]

    doDelete(index: number){
        this.emplist.splice(index,1)
    }

    //在.html实现内容,其中sex为自定义管道
    <table border="1" [width]="100">
    <tbody>
        <tr *ngIf="emplist.length==0" >
            <td>
                no employee!!
            </td>
            <td>
                <button> add </button>
            </td>
        </tr>
        <tr *ngFor="let e of emplist;index as i">
            <td>{{e.eid}}</td>
            <td>{{e.ename}}</td>
            <td>{{e.sex}}</td>
            <td>{{e.sex| sex : 'en'}}  </td><!--使用冒号给管道传参-->
            <td>{{e.salay}}</td>
            <td>
                <button (click)="doDelete(i)">Delete</button>
            </td>
        </tr>
    </tbody>
</table>
```
##  4. <a name='br-1'></a>创建对象的两种方式<br>
- 方式1：手工创建式:自己创建 `let car = new Car()`
- 方式2：依赖注入式：无需自己new，只需要声明依赖`ng g service 服务名`
- 创建服务对象的步骤：
    - 创建服务对象并指定服务提供者
    - 在组件中声明依赖，服务提供者就会自动注入，组件直接使用服务对象即可
- 使用Angular官方提供的服务对象——HttpClient Service
    - HttpClient服务对象用于向指定的URL发起异步请求，使用步骤：
        - 在主模块中导入HttpClient服务所在的模块,修改`app.module.ts`
        - 在需要使用异步请求的组件中声明依赖于HttpClient服务对象，就可以使用该对象发起异步请求了
- 在根模块中提供服务对象，即声明方法一、二，则创建的服务对象是‘单例的’——不论多少个组件使用该服务，只创建一个对象
- 在组件中提供服务对象，即声明方法三，则该对象为该组件所拥有
```typescript
    //1.创建一个log.Service.ts文件
    import { time } from "console"
    //所有的服务对象都是可被注入的

    //声明方法一：
    @Injectable({
        providedIn:'root' //指定当前服务对象在“根模块”中提供-AppModule
    })
    //声明方法二:app.module.ts文件中
    //providers:[logService]

    //声明方法三:@injectable()
    //@Component({... providers:[logService]})    组件级服务对象

    export class logService{
        doLog(action:string){//执行日志记录功能
            let uname="admin"
            let data=new Date().getTime()
            console.log(`管理员:${uname} 时间:${data} 动作:${action}`);
        }
    }

    //2.在组件中注入
    export class bookcomponent {
        log:logService
        constructor(_log:logService){
            this.log=_log
        }
        doDelete(){
            this.log.doLog("Delete")
        }
    }
```
前端体系有哪些发起异步请求的工具？各自的利弊？

|工具名   | 本质  | 优劣势 |
|:---:|:---:|:---:|
|原生XHR| let xhr = new XMLHttpResquest()| 浏览器支持的原生技术：基于回调方式处理响应；|
|JQuery.ajax()|也是XHR，只是进一步的封装而已|比原生要简单；基于回调方式处理响应|
|Axios|也是XHR，只是进一步的封装而已|比原生简单；基于Promise处理响应；可以排队、并发|
|NGHttpClient|也是XHR，只是进一步的封装而已|比原生要简单；基于“观察者模式（23种设计模式之一）”处理响应；可以排队、并发、撤销|
|Fetch|不再是XHR，是W3C提出的新技术，有望取代XHR|比XHR从根本上更加先进；天然基于Promise|

### Angular官方提供的服务对象——HttpClient
- HttpClient服务：是Angular提供的用于发起异步XHR请求的对象
- 使用步骤：
    - 1，在主模块中导入HttpClientModule模块
    - 2，在组件中声明依赖于HttpClient服务对象，就会被自动注入
    - 3，调用HttpClient实例实现异步请求`this.http.get(url).subscribe((res) => {} )`

    ```typescript
    //component.ts页面内容
    export class bookcomponent { 
    productList=[]
    private pno:number=0
    constructor(private http:HttpClient){}
    public loadMore():void{
        console.log("sdfs")
        this.pno++;
        let url:string="https://www.taobao.com/"+this.pno
        this.http.get(url).subscribe((res:any)=>{
            //console.log('get data')
            //console.log(res.data)
            this.productList=res.data
        })
    }
    //HTML页面内容
    <table border="1" [width]="500">
        <tbody>
            <tr *ngIf="productList.length==0">
                <td colspan="5">暂时没有数据</td>
            </tr>
            <tr *ngFor="let p of productList">
                <td align="center">p.id</td>
                <td align="center">p.name</td>
                <td align="center">p.img</td>
                <td align="center">p.price</td>
                <td align="center">
                    <button>删除</button>
                </td>
            </tr>
        </tbody>
    </table>
    <button (click)="loadMore()">加载下一页数据</button>
    ```

    - 4，组件的生命周期钩子函数
        - Angular中的组件的声明周期钩子函数调用顺序：
            - constructor()：组件对象被创建先调用构造函数
            - ngOnChanges()：组件绑定的属性值发生改变
            - <b>ngOnInit()</b>：组件初始化完毕-等同于Vue.js的mounted
            - ngDoCheck()：开发者自定义变更检测
            - ngAfterContentInit()：在组件内容初始化后调用
            - ngAfterContentChecked()：在组件内容每次检查后调用
            - ngAfterViewInit()：在组件视图初始化后调用。
            - ngAfterViewChecked()：在组件视图每次检查后调用
            - ngOnDestroy()：在指令销毁前调用,组件即将被从DOM树上卸载，适合执行一些资源释放性语句，例如：定时器销毁...

    - 5，父子组件传递--重点&难点
    
    ```typescript
        //创建父组件
        export class parentComponent {
            userName:string='user'
            doCry(e:string){//接收子组件传递的数据
                //e为子组件传递过来的数据
                this.userName=e
            }

            //输出父组件内部的识别符的子组件,c0为html中的标识符
            @ViewChild('c0',{static:ture})
            private child;
            print(){
                console.log(this.child)
            }

        }

        //创建子组件（更改内容组件）
        export class childModifyComponent{
            //子=>父，子组件通过触发事件（其中携带者数据），把数据传递给父组件提供事件处理方法
            userInput:string = null

            //事件发射器
            @Output() //输出型属性，可以向父组件传递数据
            cryEvent = new EventEmitter()

            doModify(){//button (click)绑定事件
                //子组将要将数据传递给父组件
                //console.log(this.userInput)
                this.cryEvent.emit(this.userInput)
            }
        }
        
        //创建子组件（图片组件）
        export class childPhotoComponent {
            //普通属性不能被父组件传值
            //private childName:string = null

            @Input()//父=>子
            //输入型属性父组件可以利用这种属性传值，一个装饰器只能装饰一个，多个参数需要多个装饰器
            childName:string = null
        }

    ```
    ```typescript
        //parent.html内容中引用事件和参数
        <div style="background-color: brown;width: 1000px;height: 1000px;"  #c0>
        //#XX标识符
            <h2>{{userName}}的博客</h2>

            //监听子组件的事件
            <app-child-modify (cryEvent) = "doCry($event)" #c1></app-child-modify>
            <app-child-photo [childName]="userName"></app-child-photo>
        </div>
    ```

### 路由和导航
- 多页面应用：一个项目中有多个完整HTML文件，使用超链接跳转--销毁一棵DOM树，同步请求另一棵，得到之后再重建新的DOM树；不足：DOM树妖反复重建，间隔中客户端一片空白
- 单页面应用：称为SPA（Single Page Application），整个项目中有且只有一个“完整的”HTML文件，其他的“页面”都只是DIV片段；需要哪个“页面”就将其异步请求下来，“插入”到“完整的”HTML文件中。单页应用的优势：整个项目中客户端只需要下载一个HTML页面，创建一个完整的DOM树；页面跳转都是一个DIV替换另一个DIV而已--能够实现过程动画。缺点：不利于SEO访问优化
- Angular中使用“单页应用”的步骤
    - 定义“路由词典”——[{URL-组件},{URL-组件},...]
        - 为每个路由组件分配一个路由地址
    - 注册“路由词典”
    - 创建路由组件挂载点——称为“路由出口”
    - 访问测试 
        - 注意：
            - 路由词典中的路由地址不能以/开头或结尾，但中间可以包含/
            - 路由词典中可以指定一个默认首页地址：`{path:'',component:...}`
            - 路由词典中的每个路由要么指定component（由哪个组件提供内容），要么指定redirect（重定向到另一个路由地址）
            - 路由词典中可以指定一个匹配任意地址‘**’，注意该地址用于整个路由词典的最后一个
    - 路由跳转/导航：从一个路由地址跳转到另一个；实现方案：
        - 方法一：使用模板跳转方法<br>
            `<any routerLink="/ucenter">`;注意：用于任意标签上，跳转地址应该以/开头，防止以相对方式跳转
    ```typescript
        //在app.module.ts文件中声明路由词典--路由地址和路由组件的对应集合
        let routes=[
            {path:'index',component:IndexComponent},
            {path:'plist',component:ProductListComponent},
            {path:'pdetail',component:ProductDetailComponent},
            {path:'ucenter',component:UserCenterComponent},
            //重定向需要指定“路由地址匹配方式”为“full”或“profix”
            {path:'',redirectTo:'pdetail',pathMatch:'full'}
            //任意地址，如404网页;**地址匹配任意地址，任意地址不能放在最上面，有顺序的查找
            {path:'**',vomponent:NotFoundComponent}
        ]
        
         @NgModule({
             imports:[RouterModule.forRoot(routes)] //导入路由模块，并注册路由词典，用于根模块中
         })

         //在.html文件中创建路由挂载点
         //访问测试通过浏览器打开，如 http://127.0.0.1:4200/index | http://127.0.0.1:4200/ucenter
         <router-outlet> </router-outlet>

        //传统的超链接可以跳转，但属于DOM树完全重建
        <a href="ucenter">进入用户中心</a>

        //routerLink可以用在任何标签上的，如按钮、属性等
         <a routerLink="/ucenter">进入用户中心</a>

         //路由嵌套修改路由词典：
         //注意：“用户中心”下的二级路由组件挂载点/路由出口应该放在UserCenter.component.html中
            {path:'user/center',
            component:IndexComponent,
            children:[//嵌套二级路由
                {path:'info',component:XXXComponent},
                {path:'security',component:XXXComponent},
            ]},

    ```
    - 方式二：使用脚本方法
        - 注意：Router类是RouterModule提供的一个服务类，声明依赖即可使用
    ```typescript
        //在.html文件中
        <button (click)="jump()">跳转页面</button>

        //在class中依赖注入
        export class ParentModule(){
            constructor(private router:Router){}

            jump(){//跳转到用户中心--需要“路由器”服务
                this.router.navigateByUrl('/ucenter')
            }
        }
    ```
    - Vue.js中的路由跳转的机制有哪些？
    1.hash法：只需要修改url中的hash部分：http://127.0.0.1/index.html#/ucenter<br>
    2.history法：需要修改window.history对象，从而支持浏览器自带的后退按钮:http://127.0.0.1/ucenter;(Angular只有这种方法)
    - 路由参数：
        - 在路由词典定义路由地址时，其中可以包含可变的参数:
        - 在.html文件中,为路由参数提供具体的参数值
        - 到目标路由组件，可以读取“当前路由地址”中的参数

```typescript
    //app.module.ts文件
    let routers = [ //':'lid表示参数名
        {path='producdetail/:lid',component:ProductDetailComponent},
    ]
        
    //在.html文件中,为路由参数提供具体的参数值
    <a routerLink="/producdetail/5">进入5</a>
    <a routerLink="/producdetail/13">进入13</a>
    <a routerLink="/producdetail/21">进入21</a>

    //在producu.component.ts文件中
    export class ProductDetailComponent implements Oninit{
        private productId:number
        
        //声明依赖：读取参数需要“当前的路由”服务对象
        constructor(private route:ActivatedRoute){}

        ngOnInit(){
            //组件初始化完成，读取路由参数，进而根据此参数查询
            this.route.params.subscribe((data)=>{
                console.log('得到订阅的路由参数：')
                console.log(data)
                this.productId = data.lid
            })
        }
    }
```

- 路由守卫：
    - Angular提供了“路由守卫（Guard）”来实现访问路由组件前的检查功能：如果检查通过（return true）就放行，如果不通过（return false）不放行 
    - `ng g guard GuardName`
```typescript
    //创建Login.guard.ts文件
    @Injectable({ //路由守卫是“可注入”服务对象
        providedIn:'root'
    })
    export class LoginGuard implements CanActivate {
        //依赖注入路由
        constructor(private router:Router){}
        
        private isLogin = false //此处应该根据用户是否登入而改变
        canActivate(){
            if(this.isLogin){//可以激活后续的组件
                return true
            }else{//阻止激活后续的组件
                this.router.navigateByUrl('/index')
                return false
            }
        }
    }

    //在app.module.ts文件中
        {path:'user/center',
        component:IndexComponent,
        //路由守护
        canActivate:[LoginGuard],
        children:[//嵌套二级路由
            {path:'info',component:XXXComponent},
            {path:'security',component:XXXComponent},
    ]},
```