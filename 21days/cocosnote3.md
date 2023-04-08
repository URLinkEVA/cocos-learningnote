# 网络请求

排行榜需求

客户端服务端

## 响应

Type length响应头（识别出格式）

响应体

## 请求

GET请求

域名->ip 访问服务器加参数

一些计网知识

# 网络请求2

json插件[JSON-Handle 官网 - 打开json格式文件的浏览编辑器 (jsonhandle.sinaapp.com)](http://jsonhandle.sinaapp.com/)

网址：html

接口api json/xml

跨域调用，一个服务器不能直接调用另一个服务器

```typescript
const {ccclass, property} = cc._decorator;

@ccclass
export default class NewClass extends cc.Component {

    // onLoad () {}

    start () {
        // url
        let url = "http://api.douban.com/v2/movie/top256?staet=25&count=25";
        // 请求
        let request = cc.loader.getXMLHttpRequest();
        request.open("GET",url,true); // 异步
        request.onreadystatechange = ()=>{
            // 请求状态改变
            // 请求结束后，获取信息
            if(request.readyState == 4 && request.status == 200){
                console.log("请求完成");
                console.log(request.responseText);
            }
        };
        request.send();
    }

    // update (dt) {}
}
```

# 自定义Animation

```typescript
const {ccclass, property} = cc._decorator;

@ccclass
export default class MyAnimation extends cc.Component {
    // 每秒播放速度
    @property
    speed: number = 0.1;
    // 播放帧数组
    @property([cc.SpriteFrame])
    sprites: cc.SpriteFrame[] = [];
    // 是否播放动画
    @property
    isPlay: boolean = false;

    // 当前播放帧
    index: number = 0;
    // 计时器
    timer: number = 0;

    // onLoad () {}

    start () {

    }

    play(){
        this.isPlay = true;
    }

    stop(){
        this.isPlay = false;
    }

    update (dt) {
        if(this.isPlay){
            // 播放动画
            // 计时器增加
            this.timer += dt;
            if(this.timer > this.speed){
                this.timer = 0;
                // 切换帧 0 1 2 0 1 2
                this.index++;
                if(this.index >= this.sprites.length){
                    this.index = 0;
                }
                // 更换帧图片
                this.getComponent(cc.Sprite).spriteFrame = this.sprites(this.index);

            }

        }
    }
}
```

# Socket

套接字 封装TCP/IP

端口0-65535

# node.js服务器



# Socket.io客户端

# tieldmap

# cocos加载tieldmap

# 控制封装

