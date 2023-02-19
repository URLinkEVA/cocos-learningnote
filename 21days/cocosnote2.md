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



# 物理碰撞

# 射线

# 射线小练习

# 动作系统

# 容器动作

# 动画系统

# 动画曲线与事件

# 小游戏练习

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

