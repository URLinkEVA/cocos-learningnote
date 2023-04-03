# 微信飞机大战练习

done

# 音频播放

通过组件形式播放AudioSource

通过代码形式播放

```typescript
start () {
    let player: cc.AudioSource = this.getComponent(cc.AudioSource);
    // 加载音频
    cc.loader.loadRes("map2",cc.AudioClip,(res,clip)=>{
        // 赋值音频
        player.clip = clip;
        // 播放
        player.play();
        // 是否正在播放
        // player.isplaying
        // 暂停
        player.pause();
        // 恢复
        player.resume();
        // 停止
        player.stop();

        // 是否循环播放
        player.loop = true;
        // 当前声音大小
        player.volume = 1;
    });
}
```

```typescript
	cc.loader.loadRes("map2",cc.AudioClip,(res,clip)=>{
        // 播放
        let audioId: number = cc.audioEngine.playMusic(clip,true);
        // 是否正在播放
        cc.audioEngine.isMusicPlaying();
        // 暂停
        cc.audioEngine.pause(audioId);
        // 暂停背景音乐
        cc.audioEngine.pauseMusic();
        // 恢复
        cc.audioEngine.resume(audioId);
        // 停止
        cc.audioEngine.stop(audioId);
        // 循环
        cc.audioEngine.setLoop(audioId,true);
        // 声音大小
        cc.audioEngine.setVolume(audioId,1);
    });
```

# 物理系统

组件rigidbody受到重力影响

在onLoad里添加代码

```typescript
cc.director.getPhysicsManager().enabled = true;
```

高速移动物体勾选bullet

Type里的dynamic受速度重力影响

# 物理碰撞

给物体一个力

物理组件中的碰撞模型

```typescript
let rbody = this.getComponent(cc.RigidBody);
// 在中心点给一个1000的力，立即生效
rbody.applyForce(cc.v2(1000,0),cc.v2(0,0),true);

rbody.applyForceToCenter(cc.v2(5000,0), true);
```

```typescript
// 速度
rbody.linearVelocity = cc.v2(50,0);
```

```typescript
// 开始碰撞
onBeginContact(contact, self, other){
     // 得到碰撞点
     let points = contact.getWorldManifold().points;
     // 法线
     let normal = contact.getWorldManifold().normal;
     console.debug("碰撞检测" + points[0]);
};

// 结束碰撞
onEndContact(contact, self, other){
     console.debug("结束碰撞");        
};
```

# 射线

sensor传感器

可以穿过的碰撞体 

```typescript
onLoad () {
    cc.director.getPhysicsManager().enabled = true;

    // 打出一条射线
    let results = cc.director.getPhysicsManager().rayCast(this.node.getPosition(), cc.v2(this.node.x, this.node.y+100), cc.RayCastType.Closest);
    for(let i = 1; i < results.length; i++){
        let res = results[i]; 
        // 射线碰到的碰撞器
        res.collider
        // 碰到的点
        res.point
        // 碰到的法线
        res.normal
    }       
}
```

# 射线小练习

```typescript
// 方向
dir: cc.Vec2 = cc.v2(0,1); // 左x右y

onLoad(){
    // 开物理检测
	cc.director.getPhysicsManager().enabled = true;
}

update(dt){
    // 更新移动
    this.node.x += this.dir.x * 100 * dt;
    this.node.y += this.dir.y * 100 * dt;
	// 射线检测
    let res = cc.director.getPhysicsManager().rayCast(this.node.position, cc.v2(this.node.x, this.node.y + this.dir.y*100), cc.RayCastType.Closest);
    if(res.length > 0) this.dir.y *= -1; 
}
```

# 动作系统

移动执行暂停

还有跳跃闪烁淡入淡出

tinTo变颜色

# 容器动作

显示隐藏翻转

序列动作，例如闪几次

```typescript
let action = ccc.show();
let action2 = cc.fadeIn(1);
let seq = cc.sequence(action, action2);
// 重复
let repeat = cc.repeat(seq, 3);
repeat = cc.repeatForever(seq);

this.node.runAction(repeat);
```

```typescript
// 闪烁完爆炸
let seq = cc.sequence(action, action2, cc.delayTime(1),cc.callFunc(()=>{
    
}));
```

# 动画系统

创建单色精灵

挂载动画组件

angle旋转物体

reverse逆向播放动画 loop循环 pingpong来回

# 动画曲线与事件

贝塞尔曲线

[从零开始学图形学：10分钟看懂贝塞尔曲线 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/344934774)

```typescript
const {ccclass, property} = cc._decorator;

@ccclass
export default class PlayerControl extends cc.Component {

    // onLoad () {}

    start () {
        let ani = this.getComponent(cc.Animation);
        // 播放动画
        ani.play("runer");
        // ani.pause();
        // ani.resume();
        // ani.stop();
    }

    // update (dt) {}
}
```

# 小游戏练习

视差效果

后置地图无限循环

```typescript
// 移动
for(let bg of this.node.children){
    bg.x -= this.speed * dt;
    // 背景超出屏幕
    if(bg.x <= -this.width) bg.x += this.width * 2;
}
```

跨脚本调用

```typescript
@property(BirdControl)
bird: BirdControl = null;
```

```typescript
cc.systemEvent.on(cc.SystemEvent.EventType.KEY_DOWN,function(e){ //按下空格监听
     switch(e.keyCode){
          case cc.macro.KEY.space:
               this.player_atk();
               break;
     }
}.bind(this), this);

cc.systemEvent.on(cc.SystemEvent.EventType.KEY_DOWN, (e)=>{
    if(e.keyCode == cc.macro.KEY.space) 
    {
        console.log("按下空格");
        this.bird.fly();
    }
});
```

## 问题

鼠标触发改成键盘空格控制无反应（已解决）

- 没点进界面里，直接开了f12调试

通过障碍中传感器输出没反应（已解决）

- 在设置好碰撞分组和物理引擎后，需要在物理刚体组件中的`Enabled Contact Listener` 属性勾选上



# 屏幕Canvas

设计分辨率

Fit Height、Fit Width填充显示

label string属性

overflow文字排版，resize自动更新节点height属性

# 图文混排

富文本

```typescript
这是<color=#ff0000>红色</color>
```

```typescript
<on click="test">这是</on><color=#ff0000>地图</color>
    
test(){
    console.debug("点击了");
}
```

点击调用test方法

图文混合

```typescript
这是枣椰树<img src = "datepalm" click = "test"/>
```

# 屏幕适配与遮罩

渲染组件mask

以屏幕作为父节点，所有都放在canvas下

屏幕适配添加ui widget组件

中心位置以及align mode

# 按钮与布局

创建ui节点button

interactable可交互

normal,pressed,hover,disabled四种状态图

click events挂载button，放置脚本调用方法

- button target 挂载的是background



layout布局，挂载三个button

人物属性对齐作用

grid格子布局，做物品栏

# 滑动进度控件

滚动视图，view做了遮罩

inertia惯性 减速0-1

pageview滑动视图

进度条ProgressBar

# 输入框

editbox包含输入文本和占位符

一些常见属性

# 补充控件

slider进度条

toggle选择框

toggleContainer三选一

videoplayer视频播放，需要预加载

webview套壳链接界面

# 对话框练习

done

# 数据存储

数据存本地，二次打开可以看到，保持进度

使用键值对，类似字典方式

```typescript
start () {
    // 储存数据
    cc.sys.localStorage.setItem("name","ayaka");
    // 获取数据
    let name = cc.sys.localStorage.getItem("name");
    console.log(name);
}
```

储存进去注释掉再获取也能找到

## 删除数据

```typescript
// 移除一个数据
cc.sys.localStorage.removeItem("name");
// 清除所有数据
cc.sys.localStorage.clear();
```

# Json数据

数据格式：json/xml/csv

客户端-客户端

游戏存档->地图、坐标、等级、攻击、防御、物品...

对象转字符串

json：{}对象，[]数组

```typescript
const {ccclass, property} = cc._decorator;

class Person{
    id: number;
    name: string;
    skill: string[];
}

@ccclass
export default class NewClass extends cc.Component {

    // onLoad () {}

    start () {
        let person:Person = new Person();
        person.id = 101;
        person.name = "ayaka";
        person.skill = ["起舞吧", "拿下"];

        // 对象->json 序列化
        let json1 = JSON.stringify(person);
        // console.log(json1);

        // 储存
        cc.sys.localStorage.setItem("save1", json1);
        
        // json->对象
        let person2:Person = Object.assign(new Person(), JSON.parse(json1));
        console.log(person2.name);
    }

    // update (dt) {}
}
```

# 数据格式

person[] -> 字符串

对一个JSON文件作完整的描述，需要另外写一个和字段结构一样的描述结构，这样更加清晰

## json

```json
{
	"code": 0,
	"message": "ok",
	"data": {
		"id": "101",
		"type": 0,
		"name": "ayaka"，
        "skill": ["起舞吧", "祭典重现"]，
		"createTime": "2023-04-03"
	},
	{
        "id": "102",
		"type": 0,
		"name": "yoimiya"，
        "skill": ["点燃引芯", "拿下"]，
		"createTime": "2023-04-03"
	}
	"#data": {
		"#id": "用户ID",
		"#type": "0=正常; 1=异常",
		"#name": "角色名称",
        "#skill": ["小技能", "大技能"],
		"#createTime": "创建时间(yyyy-MM-dd)"
	}
}
```

## xml

```xml
<root>
    <person id = "101">
        <name>ayaka</name>
        <skill>起舞吧,拿下</skill>
    </person>
    <person id = "102">
        <name>yoimiya</name>
        <skill>点燃引芯,祭典重现</skill>
    </person>
</root>
```

## csv

```
101,ayaka,起舞吧/拿下
102,yoimiya,点燃引芯/祭典重现
```
