# cocoscreator环境

配vscode以及cocos3.7.0版本

# 编程思想差异

cocos2d与cocoscreator

director导演

scene场景切换（大地图，boss点，回大地图）

sprite精灵

节点 组件

# 节点的使用

拖拽素材，xy轴，负数向左旋转

# 锚点与坐标系

anchor锚点00设置为左下角，11设置为右上角

子节点坐标相对于父节点坐标

移动父节点相当于移动子节点的世界坐标（0，0）点

# 精灵的使用

九宫格图片拉动

图片覆盖部分显示靠下的

# 精灵图集

加载所有图消耗内存浪费性能，图少问题不大

放在一个图里只需要加载一次，性能提升内存节省

texturepacker导入图片生成导出cocos2d精灵图和plist

# 向量和标量

归一化：把向量转为单位向量的过程

# 向量的运算及意义

高中知识，懂得都懂

ab点乘等于模乘cos角

# 脚本解析

新建ts脚本，挂载到精灵上，双击ts在vscode中打开，下面是自动生成的代码

```typescript
const {ccclass, property} = cc._decorator;

@ccclass
export default class NewClass extends cc.Component {

    @property(cc.Label)
    label: cc.Label = null;

    @property
    text: string = 'hello';

    // LIFE-CYCLE CALLBACKS:

    // onLoad () {}

    start () {

    }

    // update (dt) {}
}
```

 ccclass和property两个装饰器

Newclass改为新建ts同名例如Test

export default 允许类的调用

@property 面板显示可以去修改

# 脚本生命周期

```typescript
// LIFE-CYCLE CALLBACKS:
// 初始化调用
onLoad () {
    console.debug("onload");
}

onEnable(){
    console.debug("onenable");
}// 会多次执行

// 初始化调用 
start () {
	console.debug("start");
}
// 每帧调用
update (dt) {
    
}

lateUpdate(){
    
}

onDisable(){
    console.debug("ondisable");
}
// 销毁时调用
onDestroy(){
    console.debug("ondestory");
}
```

这些函数是会自己调用的，只需要在里面写东西

# 节点常用属性方法

```typescript
const {ccclass, property} = cc._decorator;

@ccclass
export default class Test extends cc.Component {

    @property(cc.Label)
    label: cc.Label = null;

    @property
    text: string = 'hello';

    // LIFE-CYCLE CALLBACKS:

    // 初始化调用
    start () {
        // 查找子节点方式
        // this.node.children[0];
        // this.node.getChildByName("abc");
        // cc.find("Canvas/Main Camera");

        // 获得父节点方式
        // this.node.getParent();
        // this.node.setParent(ddd);

        // 移除所有子节点
        // this.node.removeAllChildren();

        // 移除某个子节点
        // this.node.removeChild(ddd);

        // 从父结点中移除
        // this.node.removeFromParent();

        // 访问位置
        // this.node.x
        // this.node.y
        // this.node.setPosition(3,4);
        // this.node.setPosition(cc.v2(3,4));

        // 旋转
        // this.node.rotation
        // 缩放
        // this.node.scale
        // 锚点
        // this.node.color = cc.Color.RED;

        // 节点开关
        // this.node.active = false;

        // 组件开关
        // this.enabled = false;

        // 获取组件
        // let sprite = this.getComponent(cc.Sprite);
        // sprite.enabled
        // this.getComponentInChildren(cc.Sprite);

        // 获取失败都是null

    }

    // 每帧调用
    update (dt) {
        
    }
}
```

# 预设体的使用

创建地面使用相同脚本 

预设体相当于一个存档

 ```typescript
start () {
    // 创建节点
    let node = new cc.Node("new");
    // 添加组件
    node.addComponent(cc.Sprite);
}
 ```

```typescript
const {ccclass, property} = cc._decorator;

@ccclass
export default class Test extends cc.Component {

    @property(cc.Label)
    label: cc.Label = null;

    @property
    text: string = 'hello';

    // 预设体
    @property(cc.Prefab)
    pre: cc.Prefab = null;

    // LIFE-CYCLE CALLBACKS:

    // 初始化调用
    start () {
        // 实例化预设体
        let node = cc.instantiate(this.pre);
        // 设置父节点
        node.setParent(this.node);
        node.setPosition(0,0,0);
    }

    // 每帧调用
    update (dt) {
        
    }
}
```

# 资源动态加载

```typescript
    start () {
        let self = this;
        // http://website.com/11.png
        cc.loader.loadRes("Test/11", cc.SpriteFrame ,function(err, sp){
            self.getComponent(cc.Sprite).spriteFrame = sp;
        });
        // ```
    }
```

```typescript
cc.loader.loadResDir("Test/lumine1", cc.SpriteAtlas, function(err, atlas: cc.SpriteAtlas){
    self.getComponent(cc.Sprite).spriteFrame = atlas.getSpriteFrame("head");
});
```

动态加载精灵图中的某一部分

# 场景管理

新建场景并保存

第一个场景挂了个Test脚本

进度条就是等加载场景

```typescript
start () {
   // 加载第二个场景
   // cc.director.loadScene("game2", function(){
   //     // 当前已加载到新的场景里了
// });

   // 预加载
   cc.director.preloadScene("game2", function(){
       // 场景加载到内存了，还没有切换
       cc.director.loadScene("game2");
   });
}
```

```typescript
cc.game.addPersistRootNode(this.node); // 常驻节点
cc.game.removePersistRootNode(this.node);
```

# 键鼠事件

```typescript
start () {
    this.node.on(cc.Node.EventType.MOUSE_DOWN, function(event){
        console.debug("鼠标已按下" + event.getLocation());
    });
}
```

鼠标事件监听，getLocation方法是获取点击坐标

mouse_down,enter,move,leave,up,wheel

```typescript
start () {
    this.node.on(cc.Node.EventType.MOUSE_DOWN, function(event){
        if(event.getButton() == cc.Event.EventMouse.BUTTON_LEFT){
            console.debug("左键" + event.getLocation());
        }
        if(event.getButton() == cc.Event.EventMouse.BUTTON_RIGHT){
            console.debug("右键" + event.getLocation());
        }
    });
}
```

监听鼠标左右键

```typescript
this.node.off(cc.Node.EventType.MOUSE_DOWN); // 停止监听
```

一般写在onDestroy()中

键盘监听

```typescript
cc.systemEvent.on(cc.SystemEvent.EventType.KEY_DOWN, function(event){
      if(event.keyCode == cc.macro.KEY.w)
            console.debug("按下w键");
});
```

如果键多的话用switch

# 触摸与自定义事件

```typescript
start () {
    cc.systemEvent.on(cc.SystemEvent.EventType.KEY_DOWN, function(event){
        if(event.keyCode == cc.macro.KEY.w)
            console.debug("按下w键");
        if(event.keyCode == cc.macro.KEY.a)
            console.debug("按下a键");
        if(event.keyCode == cc.macro.KEY.s)
            console.debug("按下s键");
        if(event.keyCode == cc.macro.KEY.d)
            console.debug("按下d键");
        if(event.keyCode == cc.macro.KEY.j)
            console.debug("按下j键");
        if(event.keyCode == cc.macro.KEY.k)
            console.debug("按下k键");    
    });

    // 触摸事件
    this.node.on(cc.Node.EventType.TOUCH_START, function(event){
        console.debug("已触摸" + event.getLocation());
    });
}
```

```typescript
    // 触摸事件
    let self = this;
    this.node.on(cc.Node.EventType.TOUCH_START, function(event){
        // console.debug("已触摸" + event.getLocation());
        // self.node.emit("myevent1");
        self.node.dispatchEvent(new cc.Event.EventCustom("myevent1", true));
    });

    // 监听自定义事件
    this.node.on("myevent1", function(){
        console.debug("这是自定义事件1");
```

冒泡传递，事件一个向上传一个，传递完才会开始逐级处理

大游戏建议自己写一套消息框架，那两个都不是很好用

# 碰撞检测

添加碰撞组件，offset偏移，size大小都不怎么改

other是别人，self是自己

记得开碰撞检测

```typescript
const {ccclass, property} = cc._decorator;

@ccclass
export default class NewClass extends cc.Component {

    @property(cc.Label)
    label: cc.Label = null;

    @property
    text: string = 'hello';

    // LIFE-CYCLE CALLBACKS:pen

    // onLoad () {}

    start () {
        // 碰撞检测
        cc.director.getCollisionManager().enabled = true;
    }

    // 产生碰撞会调用一次
    onCollisionEnter(other){
        console.debug("碰撞产生" + other.tag);
    }

    onCollisionStay(other){
        console.debug("碰撞持续");
    }

    onCollisionExit(other){
        console.debug("碰撞结束");
    }

    // update (dt) {}
}
```
