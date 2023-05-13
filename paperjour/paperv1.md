# 基于2D地图探索的轻量级游戏渲染的设计与实现

# 摘要

近年来，互联网游戏行业发展迅速，todo，当前游戏h5活动短周期大需求，根据一个简单易上手的活动快速了解针对版本内容前瞻，仅供娱乐

本文在分析国内外研究的基础上，对2D地图探索轻量级游戏的设计意义、设计重点、设计状况进行了介绍。通过Typescript语言开发学习游戏引擎cocos相关知识，并结合Tiledmap制作轻量级游戏渲染地图。游戏设计还引入了语音合成技术，使用了飞桨（PaddlePaddle）深度学习框架下语音方向的开源模型库paddlespeech，进行了声音分类、语音唤醒、声纹识别、语音识别、语音翻译、语音合成、声音克隆的技术实现，并对不同网络模型采取搭建和训练。代码开源在github.com/URLinkEVA/paperjour。

关键字： 2d游戏   Cocos2d  Typescript  语音合成  深度学习

# Abstract

In recent years, the Internet has shown a rapid development trend and the development of the Internet has improved the development of various fields and boosted the social progress. The Internet has penetrated 
Based on the analysis of the research at home and abroad, this paper introduces the design significance, design emphasis and design status of 2D map exploring lightweight games. Through the development of Typescript language learning game engine cocos-related knowledge, and combined with Tiledmap to make lightweight game rendering map. The game is also designed to incorporate speech synthesis technology, using paddlespeech, an open source model library for speech directions under the paddle deep learning framework, this paper introduces the implementation of voice classification, Voice wake-up, voiceprint recognition, voice recognition, voice translation, voice synthesis and voice cloning. It works well with the finetune training, which uses the voice data set of a particular character. Code is available at github.com/URLinkEVA/paperjour.

# 第一章 绪论

## 1.1 课题研究背景与选题意义

当前游戏h5活动短周期大需求，根据一个简单易上手的活动快速了解针对版本内容前瞻，仅供娱乐

当前游戏角色声音存在不稳定性，无法满足游戏行业需求

## 1.2 课题研究现状

版本宣发，语音合成技术

### 1.2.1 国外研究现状

深度学习、sovit4

### 1.2.2 国内研究现状

paddlespeech

## 1.3论文主要内容和结构安排

### 1.3.1 论文主要内容

本文根据目前2d游戏前景以及语音合成技术及其发展形势，研究了依靠特定的配音员声音进行新的语音合成方法。主要研究内容如下：

（1）研究了基于深度学习的中文语音合成方法。该方法使用了非自回归的序列到序列模型，并行化编码及解码步骤。针对中文语言独有特点，本文增加了前端处理流程，完成了多音字消歧、文本规范化和韵律预测等处理功能。此外，在模型中添加了更多的语音特征如音高、能量和时长等，增加了音频特征。最后使用了梅尔-生成对抗网络声码器，重构语音波形。

（2）研究了基于迁移学习的四川话语音合成方法。针对四川话语言特点，研究了四川方言特殊发音、语义以及特殊词汇，并构建了四川方言数据集，添加了四川方言的文本处理前端，对输入的文本进行方言化处理，将普通话中的文字与四川方言进行映射。为得到干净数据集，研究了原始音频的维纳滤波降噪算法。在中文语音合成模型的基础上使用迁移学习（Transfer-learning）的方式对原模型进行微调（Fine-tune），实现利用较少数据集合成质量较高的四川话语音。实验比较了在不同学习率和降噪情况下，模型合成音频的效果。

（3）研究了基于深度学习的中英文多说话人语音合成方法。针对中英文中语种不同的问题，利用卡梅隆大学音素词典将中英文音素采用统一的形式表示，构建语种编码器。利用语音特征提取算法，提取说话人音频特征，并通过自注意力机制的池化层将特征转换为固定维度，输出说话人嵌入向量，构建多说话人编码器。采用中文和英文的多说话人数据集混合训练，最后评估了中英文合成音频的自然度和对应说话人的相似度。



### 1.3.2 论文结构安排

本课题主要分为六章节，课题设计的具体章节信息如下：

第一章，绪论。分析了当前语音合成技术研究的背景及意义和当前国内外研究现状，对比了传统语音合成以及深度学习语音合成方法，并介绍了本文主要研究内容以及论文结构。

第二章，理论基础及背景知识。介绍语音合成中相关的语音学基础，拼音、音素、国标等相关知识，对深度学习中基础知识也进行简单介绍，如循环神经网络、注意力机制等，并简单描述了深度学习语音合成所使用的各大模块。

第三章，地图的设计与实现。基于tiledmap开发地图。

第四章，游戏整的设计与实现。首先介绍 Cocos 的相关知识，接着按照模块化的方法对整个游戏的软件功能需求进行细致分析，并详细介绍其每个模块的具体实现过程。

第五章，深度学习语音

第六章，总结与展望。对本文游戏和语音两部分内容进行了总结，分析了研究内容的可取与不足之处，并对未来可拓展的研究重点进行了展望。

# 第二章 理论基础及相关技术

自己话写相关理论基础  功能点

## 2.1 canvas渲染技术

基于HTML 的图形渲染主要有 SVG 和 Canvas 两种主流解决方案，SVG 特点是操作简单且不依赖分辨率，适合区域大面积渲染例如百度地图这种应用程序，而 Canvas 潜力更大，能以图片形式保存场景适合图像密集型的游戏。在简单的统计图表性能方面上两者差别不明显，但在关系分析这种场景下，SVG 基本上是千这个规模，而 Canvas 则能提高1至2个数量级，例如在 1W-10W 下同样可以做到流畅交互。本文该部分主要关注如何使用 Canvas 绘制出更多的图形，提供更加流畅的交互。本文从渲染机制、性能瓶颈、绘制更多的图形、让交互更流畅、webGL 实现 2D 渲染逐步展开讲解。

SVG 是一种使用 XML 描述 2D 图形的语言，SVG DOM 中的每个元素都是可用的。可以为某个元素附加 JavaScript 事件处理器。在 SVG 中每个被绘制的图形均被视为对象，如果 SVG 对象的属性发生变化，那么浏览器能够自动重现图形。而 Canvas 通过 JavaScript 来绘制 2D 图形，逐像素进行渲染。在 Canvas 中，一旦图形被绘制完成，它就不会继续得到浏览器的关注。如果其位置发生变化，那么整个场景也需要重新绘制，包括任何被图形覆盖的对象。在渲染机制上，SVG 同其他的 HTML 标签一样，每个图形对应一个标签，图形的绘制同 HTML 标签一致。而 Canvas 本质上是一个图片，无论有多少个图形只有一个标签，需要使用 Javascript 来绘制图形。

以画一个简单的圆为示例，来对比 SVG 和 Canvas 的渲染

```
<svg>
<circle cx="100" cy="50" r="40" stroke="black" stroke-width="2" fill="red"/>
</svg>
```

```
<canvas></canvas>
<script>
  var ctx=c.getContext("2d");
  ctx.strokeStyle = 'black';
  ctx.fillStyle = 'red'
  ctx.lineWidth = 2;
  ctx.beginPath();
  ctx.arc(100,50,40,0,2 * Math.PI);
  ctx.stroke();
</script>
```

SVG 因为图形由多个不同的 HTML 标签组合而成， SVG 的图形自然而然的支持浏览器所有事件。而 Canvas 是一张画布，通过一支笔将图形绘制到画布上后，这些图形就嵌入画布组成一体，每个图形都无法独立的对浏览器的事件进行相应。拾取图形需要判断指定点所在的图形，可以通过浏览器提供的 isPointInPath 和 isPointInStroke 两个方法判定点是否在图形内，点是否在图形的边上。也可以缓存所有图形的属性，使用数学方法来判断指定点所在的图形。

浏览器方法需要重新绘制一遍图形

```
ctx.beginPath();
ctx.arc(100,50,40,0,2 * Math.PI);
const inPath = ctx.isPointInPath(100, 100);
const inStroke = ctx.isPointInStroke(100, 100);
```

这是一种使用简单，同时所有图形都适用的方式，但是成本巨大，例如：鼠标每在画布上移动一次，都会导致所有的图形绘制一遍。建议图形个数小于 500 个时使用这个方案。

数学拾取需要对每种图形提供判断是否在图形内部和图形边上

```
function isInCircle(point, x, y, r) {
  return distance(point.x, point.y, x, y) <= r;
}

function isInCircleStroke(point, x, y, r, lineWidth) {
  const d = distance(point.x, point.y, x, y);
  return d <= r + lineWidth / 2 && d >= r - lineWidth / 2;
}
const point = {x: 100, y: 100};
const inPath = isInCircle(point, 100, 50, 40);
const inStroke = isInCircle(point, 100, 50, 40, 2);
```

从性能上来说数学拾取的性能比使用浏览器的方法要快 20 倍左右，从实现上来说需要实现所有几何图形的数学计算，更多的数学计算参考 [2D 图形计算](https://link.zhihu.com/?target=https%3A//www.yuque.com/antv/ou292n/vfu79v)

两者的性能测试对比

|        |     浏览器API      |      数学计算      |
| :----: | :----------------: | :----------------: |
|   1    | 111.02999997092411 | 4.779999959282577  |
|   2    | 110.53000000538304 | 5.694999999832362  |
|   3    | 117.55500000435859 | 7.979999994859099  |
|   4    | 126.25999999899976 | 5.354999972041696  |
|   5    | 110.8949999907054  | 4.725000006146729  |
|   6    | 121.65499996622675 | 6.2049999833106995 |
|   7    | 121.18500005453825 | 4.529999976512045  |
|   8    | 116.78500002017245 | 8.094999997410923  |
|   9    | 124.06000000191852 | 8.925000031013042  |
|   10   | 124.42499998724088 | 4.849999968428165  |
| 平均值 | 118.43799999915063 | 6.113999988883734  |

在图形更新上，对于 SVG 图形直接修改对应标签的属性即可，有浏览器控制刷新图形。但对于 Canvas 则需要清除整个画布，重新绘制所有图形，也就是说 Canvas 画布上有 10W 个图形，仅仅更新一个图形时，其他剩余的 99999 个图形也需要重新绘制。

```
function drawAll() { 
  // 绘制所有图形
}
function repaint() {
  ctx.clearRect(0, 0, width, height);
  drawAll();
}
```



从上面的渲染机制我们可以自然推导出 Canvas 的图形渲染的性能瓶颈主要有以下三方面：同一时间绘制过多的图形，会阻塞浏览器的进程，导致页面不能响应。鼠标在画布上移动时，如果不能及时捕捉鼠标，会导致卡顿。图形更新时，重绘的时间过长，则帧率非常低。



当一次渲染的图形过多时，将一次渲染分成多次渲染，每次渲染时间增加几毫秒的间隔，能够有效避免部分卡顿情况。这种方案虽然会增加总的渲染时长，但是可以降低页面的卡顿感，对所有图形进行整体更新时也可以使用这个方案，但是进行交互时这种方案会带来一定的延迟。

现在的屏幕都有固定的刷新率，电脑端一般正常以 60Hz 为主，浏览器在两次硬件刷新之间进行两次重绘没有意义且消耗性能。 浏览器可以利用这个间隔 16ms（1000ms/60hz）适当地对绘制进行节流，保证 60 帧的重绘频率即可，因此重绘的间隔不能小于 16ms，我们可以将持续渲染的同步机制，改成每 16 ms 渲染的异步延迟渲染机制，这样可以大大降低重绘的频率。

![image-20230513154122165](D:\term4\毕业设计\论文写稿\image-20230513154122165.png)

延迟渲染需要保证在16ms内至少渲染一次，目前通用的有两种解决方案来实现，第一种是在一段时间 16ms 内调用 n 次渲染，保证渲染两次，第一次和最后一次，第二种是在一段时间 16ms 内调用 n 次渲染，仅渲染一次，最后一次。这两种方案都是通过抛弃无效的渲染来实现，唯一的差异在于第一种方案要渲染两次。

绘制两次的实现是用户第一次调用 draw 时，马上绘制，然后启动一个 setTimeout(function(){}, 16); 在 16ms 再次调用 draw 时，不绘制仅设置 toDraw = true ，当setTimeout 触发时，判定如果存在 toDraw=true，立刻进行下一次绘制。

绘制一次的实现是用户第一次调用 draw 时不立刻绘制，而启动 setTimeout(function(){}, 16); 在 16ms 再次调用 draw 时，直接返回什么也不处理，当setTimeout 触发时，再进行绘制。

如果一次渲染的时长小于 16ms 时进行连续的绘制，其绘制如下图所示，方案一比方案二仅多一次绘制

![image-20230513155134299](D:\term4\毕业设计\论文写稿\image-20230513155134299.png)

当一次渲染大于 16ms 时，情况比渲染小于 16ms 时复杂，我们可以看到方案一的渲染是连续的，而方案二的渲染中间必定出现 16ms 的延迟。

![image-20230513155215659](D:\term4\毕业设计\论文写稿\image-20230513155215659.png)



方案一优点是是保证马上绘制：在测试时方便，不需要进行延迟测试；动画的执行也比较准确，而且绘制帧率高，相同时间内能够渲染的帧数要比第二种方案高。缺点是实现复杂，代码不易于理解，渲染过程中没有中断，没有留出交互反馈的时间。

方案二优点是实现简单，易于理解两次渲染中间给交互留出了响应时间，缺点是测试困难，动画的执行时间不准确，渲染会有延迟，相同时间内绘制的帧数低，如果在数据量非常大的情况下拾取时间 + 16ms 的延迟，能够出现明显的卡顿效果

在Cocos游戏引擎中，采用方案二的渲染，如果遇到帧率、性能、交互反馈等问题会切回方案一并搭配分片渲染技术，而且 Canvas 主要用于游戏场景的绘制和精灵的渲染。Cocos 项目里自动引入 Canvas 组件，在游戏场景中创建 Canvas 对象，并设置画布大小和背景色等属性。将精灵添加到 Canvas 对象中，并设置精灵的位置、缩放比例和旋转角度等初始属性。在主循环中更新 Canvas 对象的状态，并绘制精灵和其他元素。虽然 Cocos 使用 Canvas 技术虽然可以实现高性能的图形绘制，但也存在一些限制和缺陷，比如不支持复杂的阴影效果和透明度调节等，但应用于本文的2D地图探索轻量级游戏已经足够。





## 2.2 TypeScript语言

TypeScript 是一种由微软开发的自由和开源的编程语言。它是 JavaScript 的一个严格超集，并添加了可选的静态类型和基于类的面向对象编程。TypeScript 的设计目标是开发大型应用，然后转译成 JavaScript 运行。由于 TypeScript 是 JavaScript 的超集，任何现有的 JavaScript 程序都是合法的 TypeScript 程序。





- 

## 2.3 声音分类

通过声音，人的大脑会获取到大量的信息，其中的一个场景是:识别和归类。如：识别熟悉的亲人或朋友的声音、识别不同乐器发出的声音、识别不同环境产生的声音等等。

声音是由发声的物体的振动产生的，当发声物体的主体振动时会发出一个基音，同时其余各部分也有复合的振动，这些振动组合产生泛音。正是这些泛音决定了发生物体的音色，使人能辨别出不同的乐器甚至不同的人发出的声音。所以根据音色的不同可以划分出男音和女音；高音、中音和低音；弦乐和管乐等。我们可以根据不同声音的特征进行区分，这种区分行为的本质，就是对声音进行分类。

声音分类根据用途还可以继续细分为以下的识别类型：

副语言识别：说话人识别（Speaker Recognition）, 情绪识别（Speech Emotion Recognition），性别分类（Speaker gender classification）。

音乐识别：音乐流派分类（Music Genre Classification）。

场景识别：环境声音分类（Environmental Sound Classification）。

声音事件检测：各个环境中的声音事件和起始时间的检测。

![img](D:\term4\毕业设计\论文写稿\clip_image002.png)

图片来源：http://speech.ee.ntu.edu.tw/~tlkagk/courses/DLHLP20/Speaker%20(v3).pdf



下面通过一个例子观察音频文件的波形，直观地了解数字音频文件的包含的内容。佩佩wav展示

```python
from paddleaudio import load
data, sr = load(file='./dog.wav', mono=True, dtype='float32')  # 单通道，float32音频样本点
print('wav shape: {}'.format(data.shape))
print('sample rate: {}'.format(sr))

# 展示音频波形
plt.figure()
plt.plot(data)
plt.show()
```



## 2.4 短时傅里叶

对于一段音频，一般会将整段音频进行分帧，每一帧含有一定长度的信号数据，一般使用 25ms，帧与帧之间的移动距离称为帧移，一般使用 10ms，然后对每一帧的信号数据加窗后，进行离散傅立叶变换（DFT）得到频谱图。

通过按照上面操作对一段音频进行分帧后，我们可以用傅里叶变换来分析每一帧信号的频率特性。将每一帧的频率信息拼接后，可以获得该音频不同时刻的频率特征——Spectrogram，也称作为语谱图。

https://zhuanlan.zhihu.com/p/19763358

https://www.yvonshong.com/2016/04/09/fft/

采用 paddle.signal.stft 演示提取示例音频的频谱特征，并进行可视化

```python
import paddle
import numpy as np

data, sr = load(file='./dog.wav', sr=32000, mono=True, dtype='float32') 
x = paddle.to_tensor(data)
n_fft = 1024
win_length = 1024
hop_length = 320

# [D, T]
spectrogram = paddle.signal.stft(x, n_fft=n_fft, win_length=win_length, hop_length=512, onesided=True)  
print('spectrogram.shape: {}'.format(spectrogram.shape))
print('spectrogram.dtype: {}'.format(spectrogram.dtype))


spec = np.log(np.abs(spectrogram.numpy())**2)
plt.figure()
plt.title("Log Power Spectrogram")
plt.imshow(spec[:100, :], origin='lower')
plt.show()
```

## 2.5 mel频谱

研究表明，人类对声音的感知是非线性的，随着声音频率的增加，人对更高频率的声音的区分度会不断下降。

例如同样是相差 500Hz 的频率，一般人可以轻松分辨出声音中 500Hz 和 1,000Hz 之间的差异，但是很难分辨出 10,000Hz 和 10,500Hz 之间的差异。

因此，学者提出了梅尔频率，在该频率计量方式下，人耳对相同数值的频率变化的感知程度是一样的。

![img](D:\term4\毕业设计\论文写稿\clip_image002-16838759425802.png)

图片来源：https://www.researchgate.net/figure/Curve-relationship-between-frequency-signal-with-its-mel-frequency-scale-Algorithm-1_fig3_221910348

关于梅尔频率的计算，其会对原始频率的低频的部分进行较多的采样，从而对应更多的频率，而对高频的声音进行较少的采样，从而对应较少的频率。使得人耳对梅尔频率的低频和高频的区分性一致。

![img](D:\term4\毕业设计\论文写稿\clip_image002-16838760116144.png)

图片来源：https://ww2.mathworks.cn/help/audio/ref/mfcc.html

Mel Fbank 的计算过程如下，而我们一般都是使用 LogFBank 作为识别特征：

![img](D:\term4\毕业设计\论文写稿\clip_image002-16838760229506.png)

图片来源：https://ww2.mathworks.cn/help/audio/ref/mfcc.html



下面例子采用 paddleaudio.features.LogMelSpectrogram 演示提取示例音频的 LogFBank:

```python
from paddleaudio.features import LogMelSpectrogram

f_min=50.0
f_max=14000.0
#   - sr: 音频文件的采样率。
#   - n_fft: FFT样本点个数。
#   - hop_length: 音频帧之间的间隔。
#   - win_length: 窗函数的长度。
#   - window: 窗函数种类。
#   - n_mels: 梅尔刻度数量。
feature_extractor = LogMelSpectrogram(
    sr=sr, 
    n_fft=n_fft, 
    hop_length=hop_length, 
    win_length=win_length, 
    window='hann', 
    f_min=f_min,
    f_max=f_max,
    n_mels=64)

x = paddle.to_tensor(data).unsqueeze(0) # [B, L]
log_fbank = feature_extractor(x) # [B, D, T]
log_fbank = log_fbank.squeeze(0) # [D, T]
print('log_fbank.shape: {}'.format(log_fbank.shape))

plt.figure()
plt.imshow(log_fbank.numpy(), origin='lower')
plt.show()
```

![image-20230512151250395](D:\term4\毕业设计\论文写稿\image-20230512151250395.png)

# 第三章 地图的设计与实现

## 3.1 Tiledmap

## 3.2 三层

# 第四章 游戏的设计与实现

## 4.1 mapControl

## 4.2 场景切换

## 4.3 playerControl

## 4.4 talkControl

### 4.4.1 文本存储

### 4.4.2 音效播放

# 第五章 深度学习语音技术研究

## 5.1 语音合成

人类通过听觉获取的信息大约占所有感知信息的 20% ~ 30%。声音存储了丰富的语义以及时序信息，由专门负责听觉的器官接收信号，产生一系列连锁刺激后，在人类大脑的皮层听区进行处理分析，获取语义和知识。近年来，随着深度学习算法上的进步以及不断丰厚的硬件资源条件，文本转语音（Text-to-Speech, TTS） 技术在移动、虚拟娱乐等领域得到了广泛的应用。

文本转语音，又称语音合成（Speech Sysnthesis），指的是将一段文本按照一定需求转化成对应的音频，这种特性决定了的输出数据比输入输入长得多。文本转语音是一项包含了语义学、声学、数字信号处理以及机器学习的等多项学科的交叉任务。虽然辨识低质量音频文件的内容对人类来说很容易，但这对计算机来说并非易事。

按照不同的应用需求，更广义的语音合成研究包括：语音转换，例如说话人转换、语音到歌唱转换、语音情感转换、口音转换等；歌唱合成，例如歌词到歌唱转换、可视语音合成等。

在第二次工业革命之前，语音的合成主要以机械式的音素合成为主。1779年，德裔丹麦科学家 Christian Gottlieb Kratzenstein 建造了人类的声道模型，使其可以产生五个长元音。1791年， Wolfgang von Kempelen 添加了唇和舌的模型，使其能够发出辅音和元音。贝尔实验室于20世纪30年代发明了声码器（Vocoder），将语音自动分解为音调和共振，此项技术由 Homer Dudley 改进为键盘式合成器并于 1939年纽约世界博览会展出。

第一台基于计算机的语音合成系统起源于20世纪50年代。1961年，IBM 的 John Larry Kelly，以及 Louis Gerstman 使用 IBM 704 计算机合成语音，成为贝尔实验室最著名的成就之一。1975年，第一代语音合成系统之一 —— MUSA（MUltichannel Speaking Automation）问世，其由一个独立的硬件和配套的软件组成。1978年发行的第二个版本也可以进行无伴奏演唱。90 年代的主流是采用 MIT 和贝尔实验室的系统，并结合自然语言处理模型。

![image-20230512154231995](D:\term4\毕业设计\论文写稿\image-20230512154231995.png)

当前的主流方法分为基于统计参数的语音合成、波形拼接语音合成、混合方法以及端到端神经网络语音合成。基于参数的语音合成包含隐马尔可夫模型（Hidden Markov Model,HMM）以及深度学习网络（Deep Neural Network，DNN）。端到端的方法保函声学模型+声码器以及“完全”端到端方法。

语音合成流水线包含 文本前端（Text Frontend） 、声学模型（Acoustic Model） 和 声码器（Vocoder） 三个主要模块:

- 通过文本前端模块将原始文本转换为字符/音素。

- 通过声学模型将字符/音素转换为声学特征，如线性频谱图、mel 频谱图、LPC 特征等。

- 通过声码器将声学特征转换为波形。





### 5.1.1 文本前端

一个文本前端模块主要包含:

- 分段（Text Segmentation）
- 文本正则化（Text Normalization, TN）
- 分词（Word Segmentation, 主要是在中文中）
- 词性标注（Part-of-Speech, PoS）
- 韵律预测（Prosody）
- 字音转换（Grapheme-to-Phoneme，G2P）（Grapheme: **语言**书写系统的最小有意义单位; Phoneme: 区分单词的最小**语音**单位）
  - 多音字（Polyphone）
  - 变调（Tone Sandhi）
    - “一”、“不”变
    - 三声变调
    - 轻声变调
    - 儿化音
    - 方言
    - ...



（输入给声学模型之前，还需要把音素序列转换为 id）

其中最重要的模块是 文本正则化 模块和 字音转换（TTS 中更常用 G2P 代指） 模块。

各模块输出示例:

```

```



### 5.1.2 声学模型

### 5.1.3 声码器

## 5.2 声音克隆

### 5.2.1 多语言小样本

### 5.2.2 一句话

### 5.2.3 派蒙声线复现

安装 PaddleSpeech 并在 PaddleSpeech/examples/other/tts_finetune/tts3 路径下配置 tools，下载预训练模型

```
!pip install PaddleSpeech
```

配置 PaddleSpeech 开发环境

```
!git clone https://gitee.com/paddlepaddle/PaddleSpeech.git
%cd PaddleSpeech
!pip install . -i https://mirror.baidu.com/pypi/simple
# 下载 NLTK
%cd /home/aistudio
!wget -P data https://paddlespeech.bj.bcebos.com/Parakeet/tools/nltk_data.tar.gz
!tar zxvf data/nltk_data.tar.gz
```

```
# 删除软链接
# aistudio会报错： paddlespeech 的 repo中存在失效软链接
# 执行下面这行命令!!
!find -L /home/aistudio -type l -delete
```

```
# 配置 MFA & 下载预训练模型
%cd /home/aistudio
!bash env.sh
```

解压文件中的音频 work/dataset/派蒙/wav/xx.wav

标签 work/dataset/派蒙/wav/labels.txt

对齐的textgrid work/dataset/派蒙/textgrid/newdir/xx.TextGrid

解压数据集

```
!unzip /home/aistudio/data/data171682/yuanshen_zip.zip -d work/
!unzip /home/aistudio/work/yuanshen_zip/派蒙.zip -d work/dataset/
```

编写执行cmd函数代码

```python
import subprocess

# 命令行执行函数，可以进入指定路径下执行
def run_cmd(cmd, cwd_path):
    p = subprocess.Popen(cmd, shell=True, cwd=cwd_path)
    res = p.wait()
    print(cmd)
    print("运行结果：", res)
    if res == 0:
        # 运行成功
        print("运行成功")
        return True
    else:
        # 运行失败
        print("运行失败")
        return False
```

配置各项参数

```python
import os

# 试验路径
exp_dir = "/home/aistudio/work/exp"
# 配置试验相关路径信息
cwd_path = "/home/aistudio/PaddleSpeech/examples/other/tts_finetune/tts3"
# 可以参考 env.sh 文件，查看模型下载信息
pretrained_model_dir = "models/fastspeech2_mix_ckpt_1.2.0"

# # 同时上传了 wav+标注文本 以及本地生成的 textgrid 对齐文件
# 输入数据集路径
data_dir = "/home/aistudio/work/dataset/派蒙/wav"
# 如果上传了 MFA 对齐结果，则使用已经对齐的文件
mfa_dir = "/home/aistudio/work/dataset/派蒙/textgrid"

new_dir = "/home/aistudio/work/dataset/派蒙/textgrid/newdir"

# 输出文件路径
wav_output_dir = os.path.join(exp_dir, "output")
os.makedirs(wav_output_dir, exist_ok=True)

dump_dir = os.path.join(exp_dir, 'dump')
output_dir = os.path.join(exp_dir, 'exp')
lang = "zh"
```

检查数据集是否合法

```python
# check oov
cmd = f"""
    python3 local/check_oov.py \
        --input_dir={data_dir} \
        --pretrained_model_dir={pretrained_model_dir} \
        --newdir_name={new_dir} \
        --lang={lang}
"""
```

```python
# 执行该步骤
run_cmd(cmd, cwd_path)
```

数据预处理

```python
cmd = f"""
python3 local/extract_feature.py \
    --duration_file="./durations.txt" \
    --input_dir={data_dir} \
    --dump_dir={dump_dir}\
    --pretrained_model_dir={pretrained_model_dir}
"""
```

```
# 执行该步骤
run_cmd(cmd, cwd_path)
```

准备微调环境

```
cmd = f"""
python3 local/prepare_env.py \
    --pretrained_model_dir={pretrained_model_dir} \
    --output_dir={output_dir}
"""
```

```
# 执行该步骤
run_cmd(cmd, cwd_path)
```

微调并训练

不同的数据集是不好给出统一的训练参数，因此在这一步可以根据自己训练的实际情况调整参数，重要参数说明：

训练轮次： epoch

1. epoch 决定了训练的轮次，可以结合 VisualDL 服务，在 AIstudio 中查看训练数据是否已经收敛，当数据集数量增加时，预设的训练轮次（100）不一定可以达到收敛状态
2. 当训练轮次过多（epoch > 200）时，建议新建终端，进入/home/aistudio/PaddleSpeech/examples/other/tts_finetune/tts3 路径下, 执行 cmd 命令，AIStudio 在打印特别多的训练信息时，会产生错误



配置文件：/home/aistudio/PaddleSpeech/examples/other/tts_finetune/tts3/conf/finetune.yaml



```
# 将默认的 yaml 拷贝一份到 exp_dir 下，方便修改
import shutil
in_label = "/home/aistudio/PaddleSpeech/examples/other/tts_finetune/tts3/conf/finetune.yaml"
shutil.copy(in_label, exp_dir)
```

```
epoch = 250
config_path = os.path.join(exp_dir, "finetune.yaml")

cmd = f"""
python3 local/finetune.py \
    --pretrained_model_dir={pretrained_model_dir} \
    --dump_dir={dump_dir} \
    --output_dir={output_dir} \
    --ngpu=1 \
    --epoch={epoch} \
    --finetune_config={config_path}
"""
```

```
# 执行该步骤
# 如果训练轮次过多，则复制上面的cmd到终端中运行
run_cmd(cmd, cwd_path)
```

生成音频

输入需要生成的文字，即可生成对应的音频文件

```
text_dict = {
    "0": "大家好，我是 派蒙，今天 天气 很不错啊，大家一起来原神找我玩呀！",
    "1": "囊 代 右，派蒙 不 是 应急 食品。",
    "2": "我是kong la ，这是 我 的 毕业 论文 语音 部分，谢谢各位 老师"
}
```

```
# 生成 sentence.txt
text_file = os.path.join(exp_dir, "sentence.txt")
with open(text_file, "w", encoding="utf8") as f:
    for k,v in sorted(text_dict.items(), key=lambda x:x[0]):
        f.write(f"{k} {v}\n")
```

调训练的模型

```
# 找到最新生成的模型
def find_max_ckpt(model_path):
    max_ckpt = 0
    for filename in os.listdir(model_path):
        if filename.endswith('.pdz'):
            files = filename[:-4]
            a1, a2, it = files.split("_")
            if int(it) > max_ckpt:
                max_ckpt = int(it)
    return max_ckpt
```

生成语音

```
# 配置一下参数信息
model_path = os.path.join(output_dir, "checkpoints")
ckpt = find_max_ckpt(model_path)

cmd = f"""
python3 /home/aistudio/PaddleSpeech/paddlespeech/t2s/exps/fastspeech2/../synthesize_e2e.py \
                --am=fastspeech2_mix \
                --am_config=models/fastspeech2_mix_ckpt_1.2.0/default.yaml \
                --am_ckpt={output_dir}/checkpoints/snapshot_iter_{ckpt}.pdz \
                --am_stat=models/fastspeech2_mix_ckpt_1.2.0/speech_stats.npy \
                --voc="hifigan_aishell3" \
                --voc_config=models/hifigan_aishell3_ckpt_0.2.0/default.yaml \
                --voc_ckpt=models/hifigan_aishell3_ckpt_0.2.0/snapshot_iter_2500000.pdz \
                --voc_stat=models/hifigan_aishell3_ckpt_0.2.0/feats_stats.npy \
                --lang=mix \
                --text={text_file} \
                --output_dir={wav_output_dir} \
                --phones_dict={dump_dir}/phone_id_map.txt \
                --speaker_dict={dump_dir}/speaker_id_map.txt \
                --spk_id=0 \
                --ngpu=1
"""
```

```
run_cmd(cmd, cwd_path)
```

语音展示

```
import IPython.display as ipd

ipd.Audio(os.path.join(wav_output_dir, "0.wav"))
```

```
ipd.Audio(os.path.join(wav_output_dir, "1.wav"))
```

```
ipd.Audio(os.path.join(wav_output_dir, "2.wav"))
```





# 第六章 总结与展望

## 6.1 总结



## 6.2 展望



# 参考文献

[1] Guzhov, A., Raue, F., Hees, J., & Dengel, A.R. (2021). AudioCLIP: Extending CLIP to Image, Text and Audio. ArXiv, abs/2106.13043.

[2] Kong, Q., Cao, Y., Iqbal, T., Wang, Y., Wang, W., & Plumbley, M.D. (2020). PANNs: Large-Scale Pretrained Audio Neural Networks for Audio Pattern Recognition. IEEE/ACM Transactions on Audio, Speech, and Language Processing, 28, 2880-2894.

[3] Gong, Y., Chung, Y., & Glass, J.R. (2021). AST: Audio Spectrogram Transformer. ArXiv, abs/2104.01778.

[4] Gemmeke, J.F., Ellis, D.P., Freedman, D., Jansen, A., Lawrence, W., Moore, R.C., Plakal, M., & Ritter, M. (2017). Audio Set: An ontology and human-labeled dataset for audio events. 2017 IEEE International Conference on Acoustics, Speech and Signal Processing (ICASSP), 776-780.

[5] Piczak, K.J. (2015). ESC: Dataset for Environmental Sound Classification. Proceedings of the 23rd ACM international conference on Multimedia.



# 致谢
