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



# 屏幕Canvas

# 图文混排

# 屏幕适配与遮罩

# 按钮与布局

# 滑动进度控件

# 输入框

# 补充控件

# 对话框练习

# 数据存储

# Json数据

# 数据格式

