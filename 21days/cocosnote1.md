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



# 场景管理

# 键鼠事件

# 触摸与自定义事件

# 碰撞检测

