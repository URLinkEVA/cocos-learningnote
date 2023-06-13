# 基于2D地图探索的轻量级游戏渲染的设计与实现

# 摘要

根据中国游戏产业年会现场正式发布的《2022年中国游戏产业报告》显示，2022年中国游戏市场实际销售收入2658.84亿元，同比下降10.33%。游戏用户规模6.64亿，同比下降0.33%。继2021年规模增长明显放缓之后，又出现过去八年来的首次下降，表明产业发展已经进入存量市场时代。面对存量市场且用户年龄层更迭的背景下，研发符合年轻人需求的创新游戏则是关键，而年轻用户对游戏品质的高要求，加速了游戏行业进入优胜略汰时代，以及内容创意型游戏的兴起。当前游戏需要做到版本更新周期短宣发力度大，可以通过一个简单易上手的轻量级游戏快速了解版本内容。

本文在分析国内外研究的基础上，对2D地图探索轻量级游戏的设计意义、设计重点、设计状况进行了介绍。通过 Typescript 语言开发学习游戏引擎 Cocos 相关知识以及Canvas渲染技术，并结合 Tiledmap 制作轻量级游戏渲染地图。游戏设计还引入了语音合成技术，使用了飞桨（PaddlePaddle）深度学习框架下语音方向的开源模型库 paddlespeech ，进行了声音分类、语音合成以及声音克隆的技术实现，并对不同网络模型采取搭建和训练。数据集采用了游戏里录制的 wav 和标注文本以及生成的 textgrid 对齐文件，在经过基于预训练模型上的300轮迭代训练后，通过文本生成近似于数据集音色的语音。代码开源在github.com/URLinkEVA/paperjour。

关键字： 2d游戏   Cocos2d  Typescript  语音合成  深度学习

# Abstract

According to the《China Game Industry Report 2022》 officially released at the annual meeting of China's game industry, the actual sales revenue of China's game market in 2022 was 265.884 billion yuan, down 10.33% year-on-year. The number of game users was 664 million, down 0.33% year-on-year. After the obvious slowdown in the growth rate in 2021, the scale declined for the first time in the past eight years, indicating that the industry has entered the era of the stock market. In the context of an existing market and changing age groups of users, it is critical to develop innovative games that meet the needs of young people. The high demand of young users for game quality has accelerated the game industry into an era of competition and the rise of creative content games. The current game needs to achieve a short update cycle of the version of the promotion of strong, you can quickly understand the version content through a simple and easy to use lightweight game.

Based on the analysis of research at home and abroad, this paper introduces the design significance, design focus and design status of 2D map exploration lightweight games. Learn knowledge related to game engine Cocos and Canvas rendering technology through Typescript language development, and make lightweight game rendering maps combined with Tiledmap. Speech synthesis technology was also introduced into the game design. paddlespeech, an open source model library for speech direction under the PaddlePaddle deep learning framework, was used to realize sound classification, speech synthesis and sound cloning technologies, and different network models were constructed and trained. The datasets uses the wav and annotated text recorded in the game and the locally-generated textgrid alignment file. After 300 rounds of iterative training based on the pre-training model, the text generates the voice that approximates the timbour of the dataset. Code is available at github.com/URLinkEVA/paperjour.

# 第一章 绪论

## 1.1 课题研究背景与选题意义

休闲娱乐已经成为了现代人生活中不可或缺的一部分，尤其是在工作和学习压力大、生活节奏快的今天。因此，选择一款画面音质好、品质优良的轻量级游戏已经成为了一种流行的休闲方式。据统计，全球在线游戏市场规模已经超过了1000亿美元，并且每年还在以10%以上的速度增长，这表明人们对休闲娱乐的需求越来越高，对高品质休闲互动游戏的需求也越来越大。休闲类游戏是覆盖年龄段最广的游戏类型之一，例如俄罗斯方块、五子棋、推箱子等经典游戏，它们曾经给不少玩家们带来美好的回忆。这些游戏不仅具有简单易上手的特点，而且还能够让玩家们在游戏中体验到挑战和成就感，因此开发出大家都喜欢的高品质休闲互动游戏能够受到人们的普遍欢迎。这样的游戏应该具备以下特点：首先游戏的画面要精美音效要逼真，能够给玩家带来身临其境的感觉；其次游戏的玩法要简单易懂但是又不失挑战性，能够让玩家们在游戏中得到成就感；最后游戏的品质要优良，不会出现卡顿闪退等问题，保证玩家们的游戏体验。

好的游戏不仅能够带给玩家们欢乐和放松，还能够激发他们的创造力和想象力。通过游戏，人们可以结交新朋友，拓展社交圈子；还可以通过游戏学习新知识，提高自己的技能水平。因此开发高品质休闲互动游戏不仅仅是为了满足人们的娱乐需求，更是为了推动整个游戏产业的发展和社会的进步。根据极光发布的《2021年中国手机游戏行业研究报告》**[1]2021年中国手机游戏行业产业报告[EB/OL].游戏产业网, 2022.** 数据显示，年轻一代（25岁及以下）用户占比过半，成为核心团体。这些年轻用户对游戏品质有较高要求，在以年轻人为主的TapTap平台《TapTap2021年度报告》**[2]TapTap2021年度报告[R].游戏大观，2021.** 显示，2021年二次元、射击、科幻、模拟、Roguelike、像素风、独立游戏以及休闲等类型的游戏更受欢迎。产品端游戏用户更倾向于优中挑优。

当前，游戏行业已经进入到精品化、多元化、全球化、年轻化的新时代。在政策方面，版号总量受到严格控制，审核趋于严格，这迫使游戏企业不断提高游戏品质和出海能力，但同时也增加了游戏研发投入门槛和风险。在产品方面，行业强调爆款逻辑，马太效应加剧，头部产品形成较强壁垒，格局稳定。此外，用户年龄层更迭，呈现年轻化趋势，对游戏品质和时间碎片化有更高的要求，也面临着其他娱乐产品竞争抢夺用户时长。中国游戏行业用户年龄层更迭，年轻用户对产品品类和品质提出了新要求，存在市场存量产品与年轻用户需求不匹配的情况。因此，游戏厂商需要在立项端做到足够超前，能够做出符合主流用户群体需求的产品力将在未来的竞争中至关重要。长期来看，国内外游戏市场都强调产品力，粗放式买量增长路径难以持续。因此，国内游戏厂商需要将更多精力放在打磨产品和提升自身工业化研发实力上。此外，内容型游戏和年轻向品类正在崛起，游戏企业需要关注市场变化和用户需求，不断开拓新的产品领域和创新点，以保持市场竞争力。

米哈游旗下律政恋爱推理手游产品《未定事件簿》项目组在2022年9月发布公告，称因莫奕配音演员无法参与语音收录的工作，为避免游戏内莫奕相关语音资源的缺失，经过慎重评估项目组将基于AI技术制作莫奕的语音【**3“纸片人”有救了？未定事件簿宣布AI换声配音**】。首批语音资源于2022年9月6日开启的“百味盈欢”活动中正式上线。莫弈是一名著名的心理医生，他的行为举止从容优雅，性格成熟稳重，因此该角色的声音更多主打温柔成熟的风格。逆熵AI合成的音色偏向沉稳的男声，与莫弈人设相契合，逆熵AI还会根据文本语境对声音做出相应改变。虽然AI配音不如真人配音真实，但能够增强代入感，使玩家更好地融入游戏世界。逆熵AI合成的声音自然且具有提升的空间，类似于米哈游推出的虚拟偶像鹿鸣。

从商业角度来看，与真人配音相比AI配音【**4AI配音**】具有成本低、效率高的优势。真人配音的不确定性通常会导致耗时、准确度不够等问题，而AI可以有效降低人力成本，同时提高配音效率。相对于真人配音的繁琐流程，AI配音可以通过对目标声音的未排序数据进行训练，快速生成流畅标准的音频，避免了人工配音时可能出现的发音错误等问题，从而提高了配音的质量和效率。另一方面，在作品适配性方面，AI也具有非常重要的优势。AI的声音通常是对数据的重复学习组成，如果有足够多的采集样本可以随意变换指定的音色，结合不同的场景配出不同的声音，这使得AI配音能够拓展自身对于作品的适配度，使得作品的呈现更加多样化和灵活性。相比之下，真人配音的音色和表现力相对固定，难以随意变换，因此在适配多种作品时存在一定的局限性。AI配音的这种灵活性和多样性，可以让作品的配音更加符合不同的场景和需求，从而提高作品的品质和竞争力。

近期在绘画界出现了AI画画，利用人工智能技术进行绘画创作。通过训练模型来学习和模仿艺术家的绘画风格，从而生成类似于艺术家绘画的图像。AI画画可以应用于许多领域，如数字艺术、电影特效、游戏开发等，为这些领域带来了更多的创作灵感和可能性。技术的发展不仅能够提高艺术作品的创作效率和质量，还可以让更多人参与到艺术创作中来，推动艺术和科技的融合发展。就游戏行业而言，AI在重新定义游戏研发的整个过程，并在持续影响关联领域。与此同时，不少相关就业冒出更多不同形式的游戏岗位需求出现。AI技术在带给游戏行业新一轮的变革，能够有效的降低游戏开发的成本，给更多的中小厂商带来新的机会，未来的游戏领域将不再是“巨头公司的天下”。通过AI，大型游戏，尤其是画面精美的3A游戏的成本会下降到小型工作室可承受的水平**[5]李海丹，郝博阳，赵杨博.专访Unity中国张俊波：8成原画师下岗，AI还将如何影响游戏行业？[EB/OL]. https://new.qq.com/rain/a/20230516A00UC200，2023-05-16**. 。

游戏结合AI的行业已经逐渐成为游戏行业的一个热点领域，涉及的技术和应用也越来越广泛。而现在游戏更新周期短急需生产力，通过AI辅助能节约时间成本，本文在通过基于Cocos游戏引擎【**6cocos**】下进行轻量级游戏的设计，并通过基于深度学习进行人物语音合成，两者互相结合进行了AI与游戏共同发展的可能性，通过这样的尝试能够达到提升游戏的趣味性与互动性，让玩家有着更好的体验。



## 1.2 课题研究现状

AI已经被广泛应用于各类行业，例如医疗保健行业的医学诊断【**7AI医疗**】、药物研发、疾病预测和健康管理等方面，通过分析大量的医学图像和数据，帮助医生更准确地诊断疾病和提供更好的治疗方案。在金融行业中上分析大量的金融数据【**8AI金融**】和市场信息，帮助金融机构更准确地评估风险和提高投资收益。在制造业【**9AI制造业**】中分析生产数据和质量数据，优化生产流程和提高产品质量，提高企业的效率和竞争力。在零售业【**10AI零售业**】中分析客户数据和市场趋势，优化营销策略和提高客户满意度，提高企业的销售额和盈利能力。在教育行业【**11AI教育**】中分析学生的学习数据和行为，为学生提供更加个性化的学习方案和教学评估，提高学生的学习效果和学习兴趣。下面本文简单介绍一下AI语音技术国内外研究现状。



多写点国内外现状

AI+游戏

### 1.2.1 国外研究现状

语音合成技术【**12AI语音合成**】是一种将文字或其他非语音信号转换为语音信号的技术。它可以将文本、符号、数字等信息转换为声音，并输出成语音信号，使得计算机可以通过语音的方式与人类进行交互。从20世纪中期以来语音合成领域在快速发展，能应用自到动问答系统、手机语音助手、智能家居、语音交互机器人、车载语音导航各种方向。在这些应用中，语音合成技术可以将计算机生成的文本或其他非语音信号转换为自然语音，从而实现人机交互【**13人机交互**】。国外在20世纪90年代后期就提出基于数据驱动的统计参数语音合成方法【**14Yoshimura T , Tokuda K , Masuko T , et al. Simultaneous Modeling of Spectrum, Pitch and Duration in HMM-Based Speech Synthesis[J]. Transactions of the Institute of Electronics Information & Communication Engineers, 1999, 83(3):2099-2107**.】，从训练语料中提取基频和谱参数等声学特征，采取HMM算法对声学特征进行建模，并在合成时给出待合成文本相对应的语音参数序列，最后由语音合成器合成出对应音频[**15何鑫. 基于 HMM 的单元挑选语音合成方法研究[D]. 西安:西安工业大学, 2017.**]。再后来出现了基于深度神经网络的统计声学建模方法[**16Qian Y, Fan Y, Hu W, et al. On the training aspects of deep neural network (DNN) for parametric TTS synthesis[C]//2014 IEEE International Conference on Acoustics, Speech and Signal Processing (ICASSP). IEEE, 2014: 3829-3833**]，对比之前的传统语音合成方法，能够对输入文本特征和输出声学特征间进行更准确的建模，改进合成语音的音质。

2016年谷歌提出了WaveNet[**17Oord A, Dieleman S, Zen H, et al. Wavenet: A generative model for raw audio[J]. arXiv preprint arXiv:1609.03499, 2016**]替换了传统的声码器，内含的卷积网络通过扩张因子使卷积层感受野随深度呈指数增长，覆盖长音频特征信息，提升了合成音质。在2017年谷歌为了合成流程速度又提出了声学模型Tacotron[**18Wang Y, Skerry-Ryan R J, Stanton D, et al. Tacotron: Towards end-to-end speech synthesis[J]. arXiv preprint arXiv:1703.10135, 2017.**]，该模型是基于注意力机制[**19Vaswani A, Shazeer N, Parmar N, et al. Attention is all you need[J]. arXiv preprint arXiv:1706.03762, 2017.**]的序列到序列语音生成模型，将经过编码的字符向量作为模型输入，不需要音素级别的对齐和标注，可以轻松地在大量数据上进行训练，最后通过声码器即可合成出高自然度的语音。由于语音合成的过程和机器翻译过程类似，在2019年由微软亚洲研究院提出基于变压器(TransFormer)网络的语音合成方法[**20Li N, Liu S, Liu Y, et al. Neural speech synthesis with transformer network[C]//Proceedings of the AAAI Conference on Artificial Intelligence. 2019, 33(01): 6706-6713**]，该方法则是将注意力机制换成了训练速度更快的变压器结构，最终实验效果接近人的自然语音。



### 1.2.2 国内研究现状

对比国外语音合成技术国内也是在不断发展，从基于传统语音合成方法做起，后面随着深度学习的流行以及国内研究机构和企业的研究投入，使语音合成技术走向市场。例如阿里巴巴NLP实验室采用基于对抗生成网络方法，提出了Meta-Voice GAN模型，通过语音信号特征之间的映射关系学习，实现高保真度的语音合成效果。京东采用了基于Transformer的神经网络模型，结合预训练微调技术，在物流提醒上有着专门的应用。腾讯提出多模态语音合成技术DurIAN模型[**21Yu C, Lu H, Hu N, et al. Durian: Duration informed attention network for multimodal synthesis[J]. arXiv preprint arXiv:1909.01700, 2019**]，摒弃传统用于学习对齐的注意力机制，使用单独模块来学习预测对齐，在直播虚拟主播场景下有着独特的合成效果。百度公司从2017年开始提出DeepVoice[**22Arık S Ö, Chrzanowski M, Coates A, et al. Deep voice: Real-time neural text-tospeech[C]//International Conference on Machine Learning. PMLR, 2017: 195-204.**]语音系列，用模块提取音素不同的特征信息，生成对应的声学特征，在后续迭代的DeepVoice3[**23Ping W, Peng K, Gibiansky A, et al. Deep voice 3: Scaling text-to-speech with convolutional sequence learning[J]. arXiv preprint arXiv:1710.07654, 2017.**]中，采用全卷积序列到序列结构，将文本特征转换多种声学参数，对应不同的解码器合成出不同质量的音频。再发展到现在的paddlespeech【**24paddlespeech**】集成了Deepspeech2【**25deepspeech2**】、Conformer【**26conformer**】和Transfromer，本文中采用的预训练模型小样本微调，也采用了基于深度强化学习的方法来进一步提高语音合成效果。  





## 1.3论文主要内容和结构安排

### 1.3.1 论文主要内容

本文根据目前2d游戏前景以及语音合成技术及其发展形势，进行了游戏场景的搭建，并实现了依靠特定声音进行新文本的语音合成方法。主要研究内容如下：

（1）研究了游戏地图整体的设计和实现。通过Tiledmap搭建了地图，该地图使用了像素级渲染的图块集组合，并进行了角色预设体自定义属性。针对扮演角色的特点，本文采用了spine骨骼动画，完成了交互、尴尬、待机、惊恐、攻击、疑问和跑步多个行为状态的功能。此外，在模型中添加了更多的语音特征如音高、能量和时长等，增加了音频特征。最后使用了梅尔-生成对抗网络声码器，重构语音波形。

（2）研究了地图碎片拼合的方法。将整体地图进行切割并打乱，用户能进行图块的拖动进行拼合。利用坐标系以及锚点的作用，设计出一套合理的吸附规则，在放下图块时能动态计算所有节点的周围位置，使拼图位置可以随意变换到自己想要放置的正确位置。最后通过所有图块正确还原的规则进行场景切换，进入到图块地图的场景中去，探索新的地图并交互获得特殊体验。

（3）研究了基于paddlespeech的中英文单人双语和多数据的语音合成方法。系统结构由inference network、domain adversarial training和synthesis network组成，inference network作为非监督学习的模块使用了变分自编码来学习音频的隐含变量，DAT模块主要功能把输入的语言信息和固定的speaker进行解耦。synthesis network模块主要功能是把语言特征转成声学特征。通过这种系统结构设计的预训练模型进行小样本微调，合成具有不错的音频质量和音色相似度的音频。


### 1.3.2 论文结构安排

本课题主要分为六章节，课题设计的具体章节信息如下：

第一章，绪论。分析了当前语音合成技术研究的背景及意义和当前国内外研究现状，简单介绍了一些深度学习语音合成方法，并介绍了本文主要研究内容以及论文结构。

第二章，理论基础及相关技术。介绍游戏开发中针对2d游戏的 Canvas 渲染技术和近期新起的 TypeScript 语言等相关知识，再由声音分类展开到音频文件的波形信息，并从公式方面对傅里叶变换介绍到项目涉及的短时傅里叶，再简单描述了音频所需的识别特征必不可少的梅尔频谱。

第三章，地图的设计与实现。基于Tiledmap设计由图块集组成的地图，进行合适的可交互人物的预设置，添加后期进行调用的自定义属性，并将地图导入cocos作为游戏场景。

第四章，游戏的设计与实现。首先从拼图前置场景设计介绍，接着按照设计所需要的功能进行介绍其每个模块的具体实现过程。

第五章，深度学习语音技术研究。从语音合成技术中最为重要的文本前端、声学模型和声码器进行介绍，再进行特定人物声线的复刻。

第六章，总结与展望。对本文游戏和语音两部分内容进行了总结，分析了研究内容的可取与不足之处，并对未来可拓展的研究重点进行了展望。

# 第二章 理论基础及相关技术

本章介绍了在游戏设计上的Canvas渲染技术【**27canvas渲染**】和Typescript语言【**28Typescript**】以及语音合成技术原理中的相关基础知识。基于上述基础知识，本文研究了异步渲染机制、 声音中的细节和短时傅里叶的变换。

## 2.1 Canvas渲染技术

基于HTML的图形渲染主要有SVG和Canvas两种主流解决方案。SVG操作简单且不依赖分辨率，适合区域大面积渲染，例如百度地图这种应用程序。而Canvas潜力更大，能以图片形式保存场景，适合图像密集型的游戏。在对于简单的统计图表，两者性能差别不明显，但在关系分析这种场景下，Canvas在万级规模需求下可以做到流畅交互，而SVG要低1至2个数量级。本文该部分主要关注如何使用Canvas绘制出更多的图形，提供更加流畅的交互。接下来将从渲染机制、性能瓶颈、绘制更多的图形、让交互更流畅、webGL实现2D渲染逐步展开讲解。

SVG是一种使用XML描述2D图形的语言，其中SVG DOM中的每个元素都是可用的，可以为某个元素附加JavaScript事件处理器。在SVG中，每个被绘制的图形都被视为对象，如果SVG对象的属性发生变化，那么浏览器能够自动重现图形。相比之下，Canvas通过JavaScript来逐像素绘制2D图形。在Canvas中，一旦图形被绘制完成，就不需要浏览器的资源关注。如果其位置发生变化，那么整个场景也需要重新绘制，包括任何被图形覆盖的对象。在渲染机制上，SVG同其他HTML标签一样，每个图形对应一个标签，图形的绘制方式同HTML标签一致。而Canvas本质上作为一个图片在多少个图形下只有一个标签，需要使用JavaScript来绘制图形。

以画圆为例来对比 SVG 和 Canvas 的渲染

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

方式使用简单，对所有图形都适用，但是鼠标每在画布上移动一次，都会导致所有的图形绘制一遍，使资源开销成本过大，而数学拾取需要对每种图形提供判断是否在图形内部和图形边上

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

在图形更新上， SVG 图形可以直接修改对应的标签属性，浏览器来刷新图形。 Canvas 则需要清除整个画布重新进行所有图形绘制，如果 Canvas 画布上有十万个个图形，对一个图形进行更新时，剩余的 99999 个图形也需要重新绘制。

```
function drawAll() { 
  // 绘制所有图形
}
function repaint() {
  ctx.clearRect(0, 0, width, height);
  drawAll();
}
```



从上面的渲染机制中可以自然推导出 Canvas 的图形渲染的性能瓶颈主要有以下三方面：同一时间绘制过多的图形会阻塞浏览器的进程，导致页面无法响应。鼠标在画布上移动时，如果不能及时捕捉鼠标会导致卡顿。图形更新时，重绘的时间过长导致帧率过低。

当一次渲染的图形过多时，将一次渲染分成多次渲染，每次渲染时间增加几毫秒的间隔，能够有效避免部分卡顿情况。这种方案虽然会增加总的渲染时长，但是可以降低页面的卡顿感，对所有图形进行整体更新时也可以使用这个方案，但是进行交互时这种方案会带来一定的延迟。

现在的屏幕都有固定的刷新率，电脑端一般正常以 60Hz 为主，浏览器在两次硬件刷新之间进行两次重绘没有意义且消耗性能。 浏览器可以利用这个间隔 16ms（1000ms/60hz）适当地对绘制进行节流，保证 60 帧的重绘频率即可，因此重绘的间隔不能小于 16ms，我们可以将持续渲染的同步机制，改成每 16 ms 渲染的异步延迟渲染机制，这样可以大大降低重绘的频率。

![image-20230513154122165](D:\term4\毕业设计\论文写稿\image-20230513154122165.png)

延迟渲染需要保证在 16ms 内至少渲染一次。目前通用的有两种解决方案，第一种是在 16ms 内调用 n 次渲染，保证第一次和最后一次有渲染，第二种是则是在 16ms 内调用 n 次渲染，仅渲染最后一次。这两种方案都是抛弃无效的渲染来实现，唯一的差异在于第一种要渲染两次。

绘制两次的实现是用户第一次调用 draw 时立刻绘制，然后启动一个 setTimeout(function(){}, 16)，在 16ms 再次调用 draw 时不绘制，仅设置 toDraw = true ，当setTimeout 触发时判定如果存在 toDraw = true，立刻进行下一次绘制。绘制一次的实现是用户第一次调用 draw 时不立刻绘制，而启动 setTimeout(function(){}, 16)，在 16ms 再次调用 draw 时，直接返回不处理，当setTimeout 触发时，再进行绘制。

如果一次渲染的时长小于 16ms 时进行连续的绘制，其绘制如下图所示，方案一比方案二仅多一次绘制

![image-20230513155134299](D:\term4\毕业设计\论文写稿\image-20230513155134299.png)

当一次渲染大于 16ms 时，情况比渲染小于 16ms 时复杂，可以看到方案一的渲染是连续的，而方案二的渲染中间必定出现 16ms 的延迟。

![image-20230513155215659](D:\term4\毕业设计\论文写稿\image-20230513155215659.png)



方案一优点是是保证立刻绘制，在测试时方便不需要进行延迟测试，动画执行准确且绘制帧率高，相同时间内能够渲染的帧数要比第二种方案高。缺点是实现复杂代码不易于理解，渲染过程中没有中断，没有留出交互反馈的时间。

方案二优点是实现简单，易于理解两次渲染中间给交互留出了响应时间，缺点是测试困难，动画执行时间不准确，渲染会有延迟，相同时间内绘制的帧数低，如果在数据量非常大的情况下拾取时间 + 16ms 的延迟，能够出现明显的卡顿效果。

在Cocos游戏引擎中，采用方案二的渲染，如果遇到帧率、性能、交互反馈等问题会切回方案一并搭配分片渲染技术，而且 Canvas 主要用于游戏场景的绘制和精灵的渲染。Cocos 项目里自动引入 Canvas 组件，在游戏场景中创建 Canvas 对象，并设置画布大小和背景色等属性。将精灵添加到 Canvas 对象中，并设置精灵的位置、缩放比例和旋转角度等初始属性。在主循环中更新 Canvas 对象的状态，并绘制精灵和其他元素。虽然 Cocos 使用 Canvas 技术虽然可以实现高性能的图形绘制，但也存在一些限制和缺陷，比如不支持复杂的阴影效果和透明度调节等，但应用于本文的2D地图探索轻量级游戏已经足够。





## 2.2 TypeScript语言

TypeScript 是一种由微软开发的自由和开源的编程语言。它是 JavaScript 【**29javascript**】的一个严格超集，并添加了可选的静态类型和基于类的面向对象编程。TypeScript 的设计目标是开发大型应用，然后转译成 JavaScript 运行。由于 TypeScript 是 JavaScript 的超集，任何现有的 JavaScript 程序都是合法的 TypeScript 程序。

Cocos 很多项目开发者之前都是使用强类型语言例如 C++或者C#来编写游戏，因此在使用 Cocos 的时候也希望能够使用强类型语言来增强项目在较大规模团队中的表现。所以从 v1.5 版本开始 Cocos 官方支持在项目中使用 TypeScript 编写脚本，用户的源码可以完全使用 TypeScript，或者 TypeScript 和 JavaScript 混合使用进行游戏开发。和其他 JavaScript 脚本一样，项目 assets 目录下的 TypeScript 脚本（.ts 文件) 在创建或修改后激活编辑器，就会被编译成兼容浏览器标准的 ES5 JavaScript 脚本，编译后的脚本存放在项目下的 library（还包括其他资源）目录。

TypeScript 强制要求开发者在编写代码时指定变量和函数的类型，这可以减少错误和调试时间。如果在编译期间发现类型错误 TypeScript 会发出警告或错误，而不是等到运行时才发现问题。由于 TypeScript 强制执行类型检查和类型推断，因此可以减少错误和不必要的重构。这种类型的强制执行可以使代码更加易于阅读和理解，因为开发者可以立即看到变量的预期类型。此外 TypeScript 还提供了一些工具和插件，例如 Lint 和 Prettier ，可以帮助自动化代码格式化和检查。TypeScript 通过提供更好的类型检查和智能提示，可以大幅减少开发时间。提供的高级功能例如类和接口可以使代码更加模块化和可重用。强制执行类型检查和类型推断能够提高代码质量，这可以减少代码中的错误和漏洞，并使代码更加稳定可靠。TypeScript 可以通过编译器将代码转换为 JavaScript 代码，以便在浏览器或其他 JavaScript 运行时环境中运行，这也使得 TypeScript 可以在多种平台上使用，包括 Web 、Node.js 等，所以在本文项目实现上选用了 TypeScript 语言进行游戏的开发。





## 2.3 声音分类

声音是由物体振动产生的机械波在空气、水或固体中传播所产生的听觉感觉，当发声物体的主体振动时会发出一个基音，同时其余各部分也有复合的振动，这些振动组合产生泛音。正是这些泛音决定了发生物体的音色，使人能辨别出不同的乐器甚至不同的人发出的声音。所以根据音色的不同可以划分出男音和女音；高音、中音和低音；弦乐和管乐等。通过声音，人的大脑会获取到大量的信息，其中的一个场景是识别和归类。如如识别熟悉的亲人或朋友的声音、识别不同乐器发出的声音、识别不同环境产生的声音等等。根据不同声音的特征进行区分，就是对声音进行分类。

声音分类【**30声音分类**】根据用途还可以继续细分为以下的识别类型：

副语言识别：说话人识别（Speaker Recognition）, 情绪识别（Speech Emotion Recognition），性别分类（Speaker gender classification）。其目的是将原始文本转换成结构化信息或语义表示，以便更好地支持该领域的自然语言处理任务，如信息抽取、问答系统、文本分类、文本摘要等。

音乐识别：音乐流派分类（Music Genre Classification）。通过计算机技术对音频信号进行分析，从而识别出音乐作品的基本信息，如音乐曲目、演唱者、作曲者、发行日期等。音乐识别技术可以通过比对音频信号与已有的音乐库进行匹配，从而找到相应的音乐作品，也可以通过音乐特征分析、机器学习等方法进行识别。音乐识别技术可以对音乐版权进行保护，防止盗版现象的发生，对用户喜好的音乐进行分析和识别，可以为用户推荐相关的音乐作品，也可以实现对广告投放的精准定位，将广告投放到目标用户喜好的音乐作品中去。

场景识别：环境声音分类（Environmental Sound Classification）。这种识别可以应用于各种不同的场景，如城市交通、自然环境、工业生产、家庭生活等。通过环境声音分类的场景识别技术，可以实现智能交通系统对交通流量、交通事故等的监测和分析，也可以环境监测系统对自然环境中的动物、风、水等声音的监测和分析，能够实现工业生产系统对机器运行状态、设备故障等的监测和分析，还可以实现智能家居系统对家庭生活中的婴儿哭声、狗吠声等的监测和提醒。

![img](D:\term4\毕业设计\论文写稿\clip_image002.png)

图片来源：http://speech.ee.ntu.edu.tw/~tlkagk/courses/DLHLP20/Speaker%20(v3).pdf



WAV是一种通用的音频文件格式，也是一种无损压缩格式，它可以提供较高的音频质量。WAV文件通常包括音频样本、采样率、采样位数和声道数等信息，因此WAV格式的音频文件通常比其他格式的文件更大，但也保留了更多的音频信息。Shape是音频信号的振幅波形，它描述了音频信号在一个时间段内的振荡情况。在数字音频中，振幅波形通常以数字方式存储，并且可以在计算机上进行处理和编辑。Sample rate是指音频信号每秒采样的次数，通常以赫兹（Hz）为单位。采样率越高，音频信号的质量就越高，但文件大小也会随之增加。通常，CD音质的采样率为44.1kHz，而高清音乐的采样率可能高达96kHz或更高。在数字音频中，采样率决定了采样点的数量，采样点是音频信号在特定时间点上的振幅值。采样率越高，采样点数量就越多，音频信号的细节和精度就越高。

在数字音频处理中，音频信号被转换成一系列数字样本点，每个样本点代表音频信号在特定时间点上的振幅值。在单通道的情况下，每个样本点只包含单个声道的振幅值。而在float32的表示方法中，每个样本点通常会用一个32位的浮点数来表示，可以表示更广泛的动态范围和更高的精度。相比于16位整数或24位整数表示方法，float32更加精确，能够处理更高质量的音频信号。同时由于float32采用了浮点数的表示方法，可以避免在数字信号处理过程中出现削波或截断等问题，从而提高了数字音频的质量和可靠性。

下面对一段音频文件进行展示，直观了解数字音频文件的包含的内容。

```python
from paddleaudio import load
data, sr = load(file='./peipei.wav', mono=True, dtype='float32')  # 单通道，float32音频样本点
print('wav shape: {}'.format(data.shape))
print('sample rate: {}'.format(sr))

# 展示音频波形
plt.figure()
plt.plot(data)
plt.show()
```

![image-20230514155539891](D:\term4\毕业设计\论文写稿\image-20230514155539891.png)

peipei.wav

![image-20230527101839112](D:\term4\毕业设计\论文写稿\image-20230527101839112.png)

## 2.4 短时傅里叶变换

平稳信号是指其统计特性不随时间变化的信号。对于一个长度为N的平稳信号$x(t)$，如果它的均值和方差在整个时间段内保持不变，那么就称它为平稳信号。与之相反，如果均值和方差在不同时间段内发生变化，那么就称它为非平稳信号。平稳信号和非平稳信号的主要区别在于它们的统计特性，由于平稳信号的统计特性不随时间变化，因此它们可以用相同的数学模型来描述和分析。而非平稳信号的统计特性随时间变化，所以它们需要不同的数学模型来描述和分析。在实际应用中心电图、语音信号等这些信号都不是完全平稳的，需要采用特殊的方法来进行分析和处理，比如说差分方程、自回归模型等。

31百纳知识.浅析信号处理：人们认识信号本质的大飞跃[EB/OL].[2018-08-14].https://www.sohu.com/a/246972969_607269.

32Marple L .Computing the discrete-time "analytic" signal via FFT[J].IEEE Transactions on Signal Processing,1999.

33Qian S , Chen D . Joint time-frequency analysis[J]. IEEE Signal Processing Magazine, 1999, 16(2):P.52-67.

34frostime.时频分析之STFT：短时傅里叶变换的原理与代码实现（非调用Matlab API）[EB/OL].[2020-06-20]https://blog.csdn.net/frostime/article/details/106816373.

35Heinrich.傅里叶分析之掐死教程（完整版）[EB/OL].[2014-06-06].https://zhuanlan.zhihu.com/p/19763358.

36Yvon Shong.深入理解快速傅里叶变换[EB/OL].[2016-04-09].https://www.yvonshong.com/2016/04/09/fft/.

通常傅里叶变换只适合处理平稳信号，由于非平稳信号频率特性会随时间变化，为了捕获这一时变特性，需要对信号进行时频分析，例如本文采用的短时傅里叶变换方法。

例如一个连续信号的傅里叶变换和它的反变换
$$
F(\omega ) =  F[f(t)]  =  \int_{ - \infty }^{ + \infty } f{\left( {t} \right)} {e^{ - j\omega t}}dt
$$

$$
F(t) =  {F^{ - 1}}[F(\omega )]  =  {1 \over {2\pi }}\int_{ - \infty }^{ + \infty } {F\left( \omega  \right)} {e^{j\omega t}}d\omega
$$

在实际应用中计算机只能处理离散信号，需要通过对连续信号进行时域采样得到离散样本，对它进行傅里叶变换得到离散时间傅里叶变换(DTFT)
$$
X(\omega ) = \sum\limits_{n =  - \infty }^\infty  {x(n){e^{ - j\omega n}}}
$$
变换后得到的频域值仍然是连续的，需要继续对频域进行采样得到离散傅里叶变换(DFT)，当前计算机中常用的快速傅里叶变换(DFT)，就是FFT的快速算法。
$$
X(k) = \sum\limits_{n = 0}^{N - 1} {x(n){e^{ - j{{2\pi kn} \over N}}}}
$$
利用频谱分析可以发现一些时域中看不到的信息，比如信号的频率分量组成、信号能量在频域上的分布等。但是傅里叶变换是一种全局的变换，时域信号经过傅里叶变换后，就变成了频域信号，从频域上无法看到时域信息，从傅里叶变换和反变换公式上来说，进行正变换时积分区间为整个时域，变换结果不包含时域信息，反变换同理。如果信号的频率特性在任何时间都不发生改变，即该信号是平稳信号时，使用傅里叶变换没有任何影响，当该信号是非平稳信号时，时域信息就极为重要。例如图中的分别是0~100Hz线性递增扫频信号和100~0Hz线性递减扫频信号的幅度谱，这两个信号都是非平稳信号，可以看到它们的幅度谱是相同的，但我们无法知道每一个频率分量出现的时间，即无法知道扫频信号的模式。 

![image-20230514145348039](D:\term4\毕业设计\论文写稿\image-20230514145348039.png)

对于一些复杂的信号和它的幅度谱，例如两路扫频信号叠加后的信号和它的幅度谱，通过时域信号或者幅度谱很难分析非平稳信号的特征。所以需要引入一个时频分析( Time-Frequency Analysis)方法——短时傅里叶变换(STFT)。

$$
x(n,\omega ) = \sum\limits_{m =  - \infty }^\infty  {x(m)\omega (n - m){e^{ - j\omega m}}}
$$
其中 $x(m)$ 为输入信号，$${\omega (m)}$$是窗函数，它在时间上反转并且有n个样本的偏移量。 $$x(n,\omega )$$是时间 $$n$$ 和频率 $$\omega $$ 的二维函数，这个信号分析带来了时频分析的概念，同时也带来了时间和频率的信息。其方法形象化的描述就是把整个时域过程分解成无数个等长的小过程，每个小过程近似平稳，再做傅里叶变换，就知道得知在哪个时间点上出现了什么频率。它将信号的时域和频域联系起来，可以通过它对信号进行时频分析，比如 $$S(n,\omega ) = |X(n,\omega ){|^2}$$ 就是语音信号所谓的语谱图(Spectrogram)。

语谱图是一种用于分析和研究语音信号的时频特征和声学特征的方法。在计算语谱图时，采用不同窗长度可以得到两种不同的语谱图，即窄带和宽带语谱图。长时窗通常用于计算窄带语谱图，短时窗则用于计算宽带语谱图。窄带语谱图具有较高的频率分辨率和较低的时间分辨率，而宽带语谱图则具有较高的时间分辨率和较低的频率分辨率。选择合适的窗长度需要根据具体的研究目的和数据特征进行综合考虑。窄带语谱图适合用于分析和识别语音中的特定频率成分，例如基频和谐波分量；而宽带语谱图则适合用于研究语音的时频特性和声学特征。同时，还需要注意窗函数的选择和处理，以保证得到准确可靠的语谱图结果。

// 语谱图更改为所有频谱图

处理音频通常会将整段音频进行按25ms间隔来分帧，每一帧含有一定长度的信号数据，帧之间的移动距离采用10ms，然后对每一帧的信号数据加窗后，进行离散傅立叶变换（DFT）得到频谱图。对一段音频进行分帧后，通过傅里叶变换来分析每一帧信号的频率特性。将每一帧的频率信息拼接后，可以获得该音频不同时刻的频率特征，即为上文谈到的语谱图。

![fft](D:\term4\毕业设计\论文写稿\fft.png)

采用 paddle.signal.stft 演示提取示例音频的频谱特征，并进行可视化

```python
import paddle
import numpy as np

data, sr = load(file='./peipei.wav', sr=32000, mono=True, dtype='float32') 
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

<img src="D:\term4\毕业设计\论文写稿\image-20230514155728548.png" alt="image-20230514155728548"  />



peipei.wav

![image-20230527101913547](D:\term4\毕业设计\论文写稿\image-20230527101913547.png)

## 2.5 梅尔频谱

研究表明，人类对声音的感知是非线性的，随着声音频率的增加，人对更高频率的声音的区分度会不断下降。

例如同样是相差 500Hz 的频率，一般人可以轻松分辨出声音中 500Hz 和 1,000Hz 之间的差异，但是很难分辨出 10,000Hz 和 10,500Hz 之间的差异。

因此，学者提出了梅尔频率【**37梅尔频率**】，在该频率计量方式下，人耳对相同数值的频率变化的感知程度是一样的。

<img src="D:\term4\毕业设计\论文写稿\clip_image002-16838759425802.png" alt="img" style="zoom:50%;" />

图片来源：https://www.researchgate.net/figure/Curve-relationship-between-frequency-signal-with-its-mel-frequency-scale-Algorithm-1_fig3_221910348

关于梅尔频率的计算，其会对原始频率的低频的部分进行较多的采样，从而对应更多的频率，而对高频的声音进行较少的采样，从而对应较少的频率。使得人耳对梅尔频率的低频和高频的区分性一致。

<img src="D:\term4\毕业设计\论文写稿\clip_image002-16838760116144.png" alt="img"  />

图片来源：https://ww2.mathworks.cn/help/audio/ref/mfcc.html

Mel Fbank 的计算过程如下，而我们一般都是使用 LogFBank 作为识别特征：

<img src="D:\term4\毕业设计\论文写稿\clip_image002-16838760229506.png" alt="img"  />

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

peipei.wav

![image-20230527101939854](D:\term4\毕业设计\论文写稿\image-20230527101939854.png)



## 2.6 本章小结

在本章中，介绍了游戏引擎图形渲染的Canvas，从与SVG画图开始到性能测试做对比，再分析两种渲染方案的优缺点；后面介绍Typescript语言的特性；在声音分类一节中，对音频文件进行了特征提取展示并进行了声音识别；接着在短时傅里叶变换这一节中讲解了原理和频谱图；最后对梅尔频谱进行了介绍。



# 第三章 地图设计和游戏场景、任务设计

本文研究的基于2D地图探索的轻量级游戏渲染的设计实现，主要是以游戏地图为载体，设计一个合适的地图极为重要，本章依次介绍如何做出一个完整的2D地图场景以及相关概念的介绍。

## 3.1 Tiledmap

要制作一个合格的 2D 地图探索游戏，需要一个完整的游戏地图场景来作为载体。在 2D 游戏中，要制作游戏地图特别是涉及多个关卡地图，业界通常都是使用 Tiledmap 【**38tiledmap**】瓦片地图来实现， 因为其操作简单，效率很高，支持的软件完善可以随时迁移，很多游戏都采用它来设计地图，例如小时候耳熟能详的平面游戏超级玛利亚、坦克大战、魂斗罗等等。

一个游戏场景就是一个简单的世界，通过素材可以为这个世界添加很多有趣的元素，让玩家有兴趣去探索。图块地图由很多小图块组成，这些小图块称之为Tile ，图块集可以由多个小图块组成，每个小图块都是一个独立的图像，并且可以拥有自己的属性和动画。这些小图块可以根据需要进行排列组合，构成不同的地图元素，如草地、沙漠、山区等。图块使用起来非常简单，在同一个游戏世界里大小都是统一规格。地图的类型常用的主要有三种类型： 90° 直角俯视地图（ Orthogonal/Square ）、45° 等距斜视地图（ Isometric ）、等六边形地图（ Hexagonal ）。这三种类型在 Tiledmap 中都是支持的，本文直接采用第一种地图类型来制作游戏场景。

![image-20230527134348915](D:\term4\毕业设计\论文写稿\image-20230527134348915.png)

在 TiledMap 中，图块集可以通过导入外部图像文件或手动绘制来创建。当图块集创建完成后，可以将其应用到 TiledMap 中的图层（Layer）中，以便在游戏运行时显示。通过在图层中使用不同的小图块，可以创建出具有不同地形、建筑物、物品等元素的游戏地图。为了保证游戏场景下的精细程度和考虑到渲染资源消耗程度，本文地图像素点采用了6144×6144的设置，块大小通过图块集的大小以及显示效果的考虑采用了32×32的块大小，进行了大地图的初步搭建。

![image-20230527134844132](D:\term4\毕业设计\论文写稿\image-20230527134844132.png)



![image-20230527135219331](D:\term4\毕业设计\论文写稿\image-20230527135219331.png)





## 3.2 人物预设体

在 TiledMap 中，对象层（Object Layer）【**39对象层**】是一种用于添加自定义元素的图层类型。与图块层不同的是，对象层不会包含任何预定义的元素，而是由开发者自行添加和编辑。对象层通常用于添加一些非常规的元素，如动态效果、特殊标记等。例如，开发者可以在对象层中添加一个特殊的标记，用于标记某个位置需要触发一个事件；或者在对象层中添加一个动态效果，如裂开、闪光等。

对象层的编辑方式与图块层类似，可以使用 TiledMap 中提供的编辑工具，如画笔、填充、选择等，进行添加和编辑。开发者可以在对象层中添加文本、图片、自定义对象等元素，并可以对这些元素进行移动、缩放、旋转等操作。对象层还支持多层嵌套，可以将多个对象层叠加在一起，实现更加复杂的效果。例如将一个对象层用于添加特殊标记，另一个对象层用于添加动态效果，然后将这两个对象层叠加在一起，以实现特殊标记和动态效果的同时呈现。

本文这里对初始出生点设置了一个命名为 startpos 的对象层，并添加了一个 isPlayer的自定义属性方便后面预设体的调用。在 TiledMap 中，图层遮盖是一种用于控制图层显示范围的技术。通过图层遮盖，开发者可以将某个图层的显示范围限制在一个特定的区域内，从而实现立体化的地图效果。使用图层遮盖时，需要先将遮盖图层添加到 TiledMap 中，并将其设置为“遮盖”类型。在被遮盖的图层中选择需要进行遮盖的区域，并将其绑定到遮盖图层上。这样被遮盖图层中的元素就只会在绑定区域内显示，而在其他区域则会被遮盖。本文这里直接在图层位置将三个图块层和对象层进行好排序，地图显示渲染会依次从上到下，得到同样的图层遮盖效果。



![image-20230527135809420](D:\term4\毕业设计\论文写稿\image-20230527135809420.png)

## 3.3 导入cocos

TiledMap 的 TSX 格式是一种用于存储图块集（Tileset）信息的 XML 格式文件。TSX 格式文件可以用于描述图块集中的各个小图块的特征、属性、动画等信息，比如说图块集的名称、图像文件路径、图块大小、碰撞信息、图块间隔、背景颜色等，以及小图块的动画信息例如帧数、播放顺序。将设计好的图块以及对应的 TSX 文件拖拽到 cocos 项目的 assets 中，再放置到要渲染的 canvas 场景中，就可以看到在 Tiledmap 中做好的地图。

![image-20230527141958880](D:\term4\毕业设计\论文写稿\image-20230527141958880.png)

![image-20230527142652672](D:\term4\毕业设计\论文写稿\image-20230527142652672.png)



## 3.4 本章小结

本章介绍了Tiledmap如何制作游戏场景地图；接着在人物预设体这一小节中介绍对象层以及如何使用；最后通过TSX格式导入Cocos引擎进行后续开发，引出下一章节的具体操作。



# 第四章 游戏整体设计

游戏的机制要设置有趣并使玩家有代入感，为解决直接进入地图落差感的问题，本章通过拼合地图碎片为引入，判断拼接完成后进行场景切换到第三章设计好的地图场景中，通过spine骨骼设计人物在大世界地图中的运动状态，并将会交互物体的弹窗文本对话音频进行存储。

## 4.1 拼图

动态加载【**40动态加载**】可以将游戏所需的资源分散到多个文件中，减小游戏包体大小，降低游戏安装包的下载时间和存储空间，提高游戏下载和使用效率。通过场景需要加载资源，，避免因资源加载到内存过多而导致内存不足或卡顿等问题，提高游戏的性能和稳定性。方便扩展和更新游戏资源，根据游戏需求，随时添加新的资源文件或替换旧的资源文件，从而满足不同的游戏需求和玩家反馈，提高游戏的品质和可持续性。本文采用下面的图片作为拼图资源，进行图片的动态加载。

![image-20230527144735386](D:\term4\毕业设计\论文写稿\image-20230527144735386.png)

```
// 加载图片
cc.loader.loadRes("pic", cc.Texture2D, function (err, texture) {
    self.curTexture = texture;
    self.__initItems();
    self.__shuffleItemPos();
});
```

切割图片操作是设置 spriteFrame 的时候，在 texture 中划分指定区域（rect）作为 spriteFrame，并改变 sprite 所属节点的大小，然后进行图片的随机打乱。首先是遍历切割成九块的图片，然后将每个图片与另一个随机选中的图片交换位置，从而实现图片的随机合理分布。

```
let rect = cc.rect(cfg.x, cfg.y, cfg.w, cfg.h);
this.picSprite.spriteFrame = new cc.SpriteFrame(cfg.texture, rect);
this.picSprite.node.setContentSize(cfg.w, cfg.h);

for (let i = 0; i < self.items.length; i++) {
     let randomSeed = Math.floor(Math.random() * self.items.length);
     let item = self.items[i];
     let rItem = self.items[randomSeed];
     let x = item.cfg.posX;
     let y = item.cfg.posY;

     item.setItemPosition(rItem.cfg.posX, rItem.cfg.posY);
     rItem.setItemPosition(x, y);
 }
```

Cocos 中的TOUCH_MOVE 事件是一种用于处理触摸移动的事件类型，通常用于实现手势操作、角色移动、摄像机跟随等功能。当用户在屏幕上滑动手指时，Cocos Creator 会触发 TOUCH_MOVE 事件，并将触摸点的坐标和移动距离等信息传递给事件回调函数。开发者可以通过监听 TOUCH_MOVE 事件，并在事件回调函数中处理触摸点的位置和移动距离等信息，从而实现游戏中角色的移动、摄像机的跟随等效果。本文中使用 TOUCH_MOVE 事件进行图片的移动，通过监听事件随时改变元素的位置（x 和 y），拖动时设置了透明度从拖动时为100到拖动完成后为255达到一个移动效果正反馈。

```
this.node.on(cc.Node.EventType.TOUCH_MOVE, function (event) {
    this.opacity = 100;
    let delta = event.touch.getDelta();
    this.x += delta.x;
    this.y += delta.y;
}, this.node);

this.node.on(cc.Node.EventType.TOUCH_END, function () {
    this.opacity = 255;
}, this.node);
```

在 Cocos 中，坐标系是用来描述场景中节点位置和方向的一组规则和体系。坐标系遵循右手坐标系规则，即 X 轴向右，Y 轴向上，Z 轴指向屏幕外方向，这样定义的坐标系与数学中的笛卡尔坐标系类似，但与屏幕坐标系略有不同。屏幕坐标系的原点通常在屏幕左下角，X 轴向右，Y 轴向上。而游戏场景中的坐标系的原点通常在场景的中心，X 轴向右，Y 轴向上。因此场景中节点的位置和方向通常使用游戏世界坐标系来描述。锚点通常表示节点在自身坐标系中的一个固定点，用于确定节点的位置和旋转中心，从而实现不同的游戏效果和布局方式，本文使用的锚点就是默认的中心点位置 (0.5,0.5)。得到图块中心点位置进行以此为中心的半径为80的扩张圆，移动图块时一旦落到扩张圆所处的范围内，就把图块位置自动移动到扩张圆对应圆心处，能达到一种吸附效果。

![image-20230527152940209](D:\term4\毕业设计\论文写稿\image-20230527152940209.png)



## 4.2 场景切换

标记出所有图块号，一旦每块地图相对位置正确，就回调检查拼图完成的方法，进行场景切换方法的调用。场景切换【**41场景切换**】是指在游戏运行时切换不同场景的操作，通常用于实现游戏的关卡切换、菜单切换、场景跳转等功能，根据游戏需求和场景布局，灵活使用场景切换功能，可以实现不错的游戏效果，在游戏运行时，通过代码调用 cc.director.loadScene() 方法来切换场景，loadScene() 方法的参数为目标场景的名称或资源路径，本文是切换到第三章设置好资源名为 game2 的地图。

```
// 预加载
cc.director.preloadScene("game2", function(){
   // 场景加载到内存了，还没有切换
   cc.director.loadScene("game2");
});
```

预加载是在游戏运行前提前加载部分场景资源，以提高游戏的启动速度和流畅度。通常应用于需要频繁切换场景的游戏中，可以减少场景切换时的加载时间和卡顿现象。

## 4.3 人物状态控制

Spine 【**42spine**】是一种用于创建骨骼动画的工具和文件格式。在 Cocos 中，可以通过 Spine 插件来加载和播放 Spine 动画。Spine 文件通常由以下几个部分组成：

1. 骨骼：用于描述动画中的骨骼结构，通常包含骨骼名称、父子关系、位置、旋转、缩放等属性。
2. 插槽：用于描述骨骼上的插槽，通常包含插槽名称、绑定的图片或纹理等属性。
3. 图片/纹理：用于描述动画中使用的图片或纹理，通常包含图片/纹理名称、路径、尺寸、裁剪区域等属性。
4. 动画：用于描述动画的关键帧和补间动画，通常包含动画名称、帧率、关键帧、曲线类型等属性。

本文使用png格式的精灵图，采用包含状态的atlas纹理图集以及对应图集的json骨架，制作了交互、尴尬、待机、惊恐、攻击、疑问和跑的多个人物控制状态，导入到 cocos 中并转换成预设体资源，玩家移动或者交互时会触发不同对应的人物状态。

![image-20230527160028362](D:\term4\毕业设计\论文写稿\image-20230527160028362.png)

![image-20230527160417131](D:\term4\毕业设计\论文写稿\image-20230527160417131.png)

![image-20230528194923081](D:\term4\毕业设计\论文写稿\image-20230528194923081.png)

## 4.4 文本音效存储

大多数游戏中的文本通常使用 JSON 或 XML 等格式进行存储，以便于在游戏中加载和使用。文本文件中通常包含对话内容、角色名、头像、位置、动画等信息，开发者可以根据游戏需求和场景布局，灵活设计和编辑文本文件，例如下面简单交互的存储：

```
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
```

加载文本则需要使用 cocos 的资源管理器，并使用 JSON.parse() 或 XML.parse() 等方法将文本文件解析成 JavaScript 对象或数组。然后在游戏中根据需要使用文本内容来显示对应信息。

对话游戏中的音效通常使用 MP3、WAV、OGG 等格式进行存储，以便于在游戏中加载和播放。音效文件通常包含对话声音、音效特效、背景音乐等内容，使用 cocos 的音频管理器加载音效文件，并使用 cc.audioEngine.play() 或 cc.audioEngine.pause() 等方法来播放、暂停、停止音效。可以在游戏中使用音效来增强对话的氛围和情感表达。本文将训练好的音频文件放在 resources 文件夹下统一管理，使用不同的事件函数加载对应的音频达到完整的效果。



## 4.5 本章小结

本章介绍了拼图场景，拼好进行场景切换，再介绍spine组合人物状态以及文本音频存储，做出一个完整的游戏，存储的音频由下一章节进行训练合成。



# 第五章 深度学习语音研究

## 5.1 语音合成

人类通过听觉获取的信息大约占所有感知信息的 20% ~ 30%。声音存储了丰富的语义以及时序信息，由专门负责听觉的器官接收信号，产生一系列连锁刺激后，在人类大脑的皮层听区进行处理分析，获取语义和知识。近年来，随着深度学习算法上的进步以及不断丰厚的硬件资源条件，文本转语音（Text-to-Speech, TTS） 技术在移动、虚拟娱乐等领域得到了广泛的应用。

文本转语音，又称语音合成（Speech Sysnthesis），指的是将一段文本按照一定需求转化成对应的音频，这种特性决定了输出数据比输入输入长得多。文本转语音是一项包含了语义学、声学、数字信号处理以及机器学习的等多项学科的交叉任务，按照不同的应用需求，更广义的语音合成研究包括：语音转换，例如说话人转换、语音到歌唱转换、语音情感转换、口音转换等；歌唱合成，例如歌词到歌唱转换、可视语音合成等。

在第二次工业革命之前，语音的合成主要以机械式的音素合成为主。1779年德裔丹麦科学家 Christian Gottlieb Kratzenstein 建造了人类的声道模型，使其可以产生五个长元音。1791年Wolfgang von Kempelen添加了唇和舌的模型，使其能够发出辅音和元音。贝尔实验室于20世纪30年代发明了声码器（Vocoder），将语音自动分解为音调和共振，此项技术由 Homer Dudley 改进为键盘式合成器并于 1939年纽约世界博览会展出。

世界上第一台基于计算机的语音合成系统可追溯到20世纪50年代。1961年，IBM 的 John Larry Kelly和Louis Gerstman使用IBM704计算机合成语音，成为贝尔实验室最著名的成就之一。1975年，第一代语音合成系统之一MUSA（MUltichannel Speaking Automation）问世，由一个独立的硬件和配套的软件组成，在之后1978年发行的第二个版本可以进行无伴奏演唱。

![image-20230512154231995](D:\term4\毕业设计\论文写稿\image-20230512154231995.png)

当前的主流方法分为基于统计参数的语音合成、波形拼接语音合成、混合方法以及端到端神经网络语音合成。基于参数的语音合成包含隐马尔可夫模型（Hidden Markov Model,HMM）以及深度学习网络（Deep Neural Network，DNN）。端到端的方法保函声学模型+声码器以及“完全”端到端方法。

![image-20230517142957068](D:\term4\毕业设计\论文写稿\image-20230517142957068.png)



语音合成流水线包含 文本前端（Text Frontend） 、声学模型（Acoustic Model） 和 声码器（Vocoder） 三个主要模块，通过文本前端模块将原始文本转换为字符/音素，通过声学模型将字符/音素转换为声学特征，如线性频谱图、mel 频谱图、LPC 特征等，通过声码器将声学特征转换为波形。



### 5.1.1 文本前端

在大部分语音合成场景中无法保证所有的输入文本都是标准格式，例如存在句子的分词、词的变调和儿化音和多音字这些情况。人类在读文本时不需要结合太多专业的语义学知识和经验就能够读出正确的发音，但不具备先验知识的计算机做不到，这些文本并不能直接被声学模型所读取，所以需要在文本前端模块从给定文本中提取语言特征、字符和音素。主要包括:文本分割、文本归一化(TN)、单词分割(WS)、词性标注、韵律预测和字母到音素(G2P)

![image-20230528144633251](D:\term4\毕业设计\论文写稿\image-20230528144633251.png)

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

其中最重要的模块是文本正则化模块和字音转换（TTS 中更常用 G2P 代指） 模块。

各模块输出示例:

```
• Text: 全国一共有112所211高校
• Text Normalization: 全国一共有一百一十二所二一一高校
• Word Segmentation: 全国/一共/有/一百一十二/所/二一一/高校/
• G2P（注意此句中“一”的读音）:
    quan2 guo2 yi2 gong4 you3 yi4 bai3 yi1 shi2 er4 suo3 er4 yao1 yao1 gao1 xiao4
    （可以进一步把声母和韵母分开）
    q uan2 g uo2 y i2 g ong4 y ou3 y i4 b ai3 y i1 sh i2 er4 s uo3 er4 y ao1 y ao1 g ao1 x iao4
    （把音调和声韵母分开）
    q uan g uo y i g ong y ou y i b ai y i sh i er s uo er y ao y ao g ao x iao
    0 2 0 2 0 2 0 4 0 3 ...
• Prosody (prosodic words #1, prosodic phrases #2, intonation phrases #3, sentence #4):
    全国#2一共有#2一百#1一十二所#2二一一#1高校#4
    （分词的结果一般是固定的，但是不同人习惯不同，可能有不同的韵律）
```

文本前端模块的设计需要结合很多专业的语义学知识和经验。人类在读文本的时候可以自然而然地读出正确的发音，但是这些先验知识计算机并不知晓。 例如，对于一个句子的分词：

```
我也想过过过儿过过的生活
我也想/过过/过儿/过过的/生活

货拉拉拉不拉拉布拉多
货拉拉/拉不拉/拉布拉多

南京市长江大桥
南京市长/江大桥
南京市/长江大桥
```

或者是词的变调和儿化音：

```
你要不要和我们一起出去玩？
你要不（2声）要和我们一（4声）起出去玩（儿）？

不好，我要一个人出去。
不（4声）好，我要一（2声）个人出去。

（以下每个词的所有字都是三声的，请你读一读，体会一下在读的时候，是否每个字都被读成了三声？）
纸老虎、虎骨酒、展览馆、岂有此理、手表厂有五种好产品
```

又或是多音字，这类情况通常需要先正确分词：

```
人要行，干一行行一行，一行行行行行;
人要是不行，干一行不行一行，一行不行行行不行。

佟大为妻子产下一女

海水朝朝朝朝朝朝朝落
浮云长长长长长长长消
```

PaddleSpeech Text-to-Speech的文本前端解决方案:

- [文本正则]([PaddleSpeech/examples/other/tn at develop · PaddlePaddle/PaddleSpeech · GitHub](https://github.com/PaddlePaddle/PaddleSpeech/tree/develop/examples/other/tn))

- [G2P]([PaddleSpeech/examples/other/g2p at develop · PaddlePaddle/PaddleSpeech · GitHub](https://github.com/PaddlePaddle/PaddleSpeech/tree/develop/examples/other/g2p))

  - 多音字模块: pypinyin/g2pM

  - 变调模块: 用分词 + 规则



构造文本前端对象

传入`phones_dict`，把相应的`phones`转换成`phone_ids`。

```
# 传入 phones_dict 会把相应的 phones 转换成 phone_ids
frontend = Frontend(phone_vocab_path=phones_dict)
print("Frontend done!")
100%|██████████| 575056/575056 [00:13<00:00, 41310.49it/s]
[2023-5-14 11:17:55,818] [    INFO] - Downloading https://bj.bcebos.com/paddle-hapi/models/bert/bert-base-chinese-vocab.txt and saved to /home/aistudio/.paddlenlp/models/bert-base-chinese
[2023-5-14 11:17:55,822] [    INFO] - Downloading bert-base-chinese-vocab.txt from https://bj.bcebos.com/paddle-hapi/models/bert/bert-base-chinese-vocab.txt
100%|██████████| 107k/107k [00:00<00:00, 6.80MB/s]
[2023-5-14 11:17:55,917] [    INFO] - tokenizer config file saved in /home/aistudio/.paddlenlp/models/bert-base-chinese/tokenizer_config.json
[2023-5-14 11:17:55,919] [    INFO] - Special tokens file saved in /home/aistudio/.paddlenlp/models/bert-base-chinese/special_tokens_map.json
Frontend done!
```

调用文本前端

文本前端对输入数据进行正则化时会进行分句，若`merge_sentences`设置为`False`，则所有分句的 `phone_ids` 构成一个 `List`；若设置为`True`，`input_ids["phone_ids"][0]`则表示整句的`phone_ids`。

```
input = "你好，欢迎使用百度飞桨框架进行深度学习研究！"
input_ids = frontend.get_input_ids(input, merge_sentences=True, print_info=True)
phone_ids = input_ids["phone_ids"][0]
print("phone_ids:%s"%phone_ids)
Building prefix dict from the default dictionary ...
[2023-5-14 11:17:58] [DEBUG] [__init__.py:113] Building prefix dict from the default dictionary ...
Dumping model to file cache /tmp/jieba.cache
[2023-5-14 11:17:59] [DEBUG] [__init__.py:147] Dumping model to file cache /tmp/jieba.cache
Loading model cost 0.760 seconds.
[2023-5-14 11:17:59] [DEBUG] [__init__.py:165] Loading model cost 0.760 seconds.
Prefix dict has been built successfully.
[2023-5-14 11:17:59] [DEBUG] [__init__.py:166] Prefix dict has been built successfully.
```

```
text norm results:
['你好，', '欢迎使用百度飞桨框架进行深度学习研究！']
----------------------------
g2p results:
[['n', 'i2', 'h', 'ao3', 'sp', 'h', 'uan1', 'ing2', 'sh', 'iii3', 'iong4', 'b', 'ai3', 'd', 'u4', 'f', 'ei1', 'j', 'iang3', 'k', 'uang1', 'j', 'ia4', 'j', 'in4', 'x', 'ing2', 'sh', 'en1', 'd', 'u4', 'x', 've2', 'x', 'i2', 'ian2', 'j', 'iou1']]
----------------------------
phone_ids:Tensor(shape=[38], dtype=int64, place=Place(gpu:0), stop_gradient=True,
       [155, 73 , 71 , 29 , 179, 71 , 199, 126, 177, 115, 138, 37 , 9  , 40 ,
        186, 69 , 46 , 151, 89 , 152, 204, 151, 80 , 151, 123, 260, 126, 177,
        51 , 40 , 186, 260, 251, 260, 73 , 83 , 151, 140])
```

用深度学习实现文本前端

![image-20230517143932024](D:\term4\毕业设计\论文写稿\image-20230517143932024.png)



### 5.1.2 声学模型

声学模型将字符或者音素转换为声学特征，如线性频谱图、mel 频谱图、LPC 特征等，声学特征以 “帧” 为单位，一个音素对应 5~20 帧左右，一帧通常为10ms， 声学模型需要解决的是 “不等长序列间的映射问题”，“不等长”是指同一个人发不同音素的持续时间不同，同一个人在不同时刻说同一句话的语速可能不同，对应各个音素的持续时间不同，不同人说话的特色不同，对应各个音素的持续时间不同。这是一个困难的“一对多”问题。

```
# 卡尔普陪外孙玩滑梯
000001|baker_corpus|sil 20 k 12 a2 4 er2 10 p 12 u3 12 p 9 ei2 9 uai4 15 s 11 uen1 12 uan2 14 h 10 ua2 11 t 15 i1 16 sil 20
```

声学模型主要分为自回归模型和非自回归模型，其中自回归模型在 t 时刻的预测需要依赖 t-1 时刻的输出作为输入，预测时间长，但是音质相对较好，非自回归模型不存在预测上的依赖关系，预测时间快，音质相对较差。

主流声学模型发展的脉络:

- 自回归模型:
  - Tacotron
  - Tacotron2
  - Transformer TTS
- 非自回归模型:
  - FastSpeech
  - SpeedySpeech
  - FastPitch
  - FastSpeech2



这里使用fastspeech2

<img src="D:\term4\毕业设计\论文写稿\image-20230517144122585.png" alt="image-20230517144122585" style="zoom:50%;" />

PaddleSpeech TTS 实现的 FastSpeech2 与论文不同的地方在于，前者使用的的是 phone 级别的 `pitch` 和 `energy`(与 FastPitch 类似)，这样的合成结果可以更加稳定。

<img src="D:\term4\毕业设计\论文写稿\image-20230517144204544.png" alt="image-20230517144204544" style="zoom:50%;" />

[语音合成模型的发展及改进](https://github.com/PaddlePaddle/PaddleSpeech/blob/develop/docs/source/tts/models_introduction.md)

初始化声学模型 FastSpeech2

```
with open(phones_dict, "r") as f:
    phn_id = [line.strip().split() for line in f.readlines()]
vocab_size = len(phn_id)
print("vocab_size:", vocab_size)
odim = fastspeech2_config.n_mels
model = FastSpeech2(
    idim=vocab_size, odim=odim, **fastspeech2_config["model"])
# 加载预训练模型参数
model.set_state_dict(paddle.load(fastspeech2_checkpoint)["main_params"])
# 推理阶段不启用 batch norm 和 dropout
model.eval()
stat = np.load(fastspeech2_stat)
# 读取数据预处理阶段数据集的均值和标准差
mu, std = stat
mu, std = paddle.to_tensor(mu), paddle.to_tensor(std)
# 构造归一化的新模型
fastspeech2_normalizer = ZScore(mu, std)
fastspeech2_inference = FastSpeech2Inference(fastspeech2_normalizer, model)
fastspeech2_inference.eval()
print("FastSpeech2 done!")
```

调用声学模型

```
with paddle.no_grad():
    mel = fastspeech2_inference(phone_ids)
print("shepe of mel (n_frames x n_mels):")
print(mel.shape)
# 绘制声学模型输出的梅尔频谱
fig, ax = plt.subplots(figsize=(16, 6))
im = ax.imshow(mel.T, aspect='auto',origin='lower')
plt.title('Mel Spectrogram')
plt.xlabel('Time')
plt.ylabel('Frequency')
plt.tight_layout()
```

<img src="D:\term4\毕业设计\论文写稿\image-20230517144349757.png" alt="image-20230517144349757" style="zoom:50%;" />



### 5.1.3 声码器

声码器能够将声学特征转换为波形。要解决的问题是信息缺失的补全。在音频波形转换为频谱图的时候通常会存在相位信息的缺失，在频谱图转换为梅尔频谱图时存在频域压缩导致的信息缺失。以16kHZ采样率的音频为例，10ms为一帧，1s 的音频有16000个采样点，每一帧就会有160个采样点，声码器能够将一个频谱帧变成音频波形的160个采样点，所以声码器中一般会包含上采样模块。

与声学模型类似，声码器也分为自回归模型和非自回归模型, 更细致的分类如下:

- Autoregression
  - WaveNet
  - WaveRNN
  - LPCNet
- Flow
  - WaveFlow
  - WaveGlow
  - FloWaveNet
  - Parallel WaveNet
- GAN
  - WaveGAN
  - Parallel WaveGAN
  - MelGAN
  - Style MelGAN
  - Multi Band MelGAN
  - HiFi GAN
- VAE
  - Wave-VAE
- Diffusion
  - WaveGrad
  - DiffWave



PaddleSpeech TTS 主要实现了百度的 WaveFlow 和一些主流的 GAN Vocoder, 在本文中使用 Parallel WaveGAN 作为声码器。

<img src="D:\term4\毕业设计\论文写稿\image-20230517144624912.png" alt="image-20230517144624912" style="zoom:50%;" />

各 GAN Vocoder 的生成器和判别器的 Loss 的区别如下表格所示:

|       Model        |                        Generator Loss                        |                    Discriminator Loss                    |
| :----------------: | :----------------------------------------------------------: | :------------------------------------------------------: |
|      Mel GAN       |             adversial loss<br/>Feature Matching              |                Multi-Scale Discriminator                 |
| Parallel Wave GAN  |        adversial loss<br/>Multi-resolution STFT loss         |                      adversial loss                      |
| Multi-Band Mel GAN | adversial loss<br/>full band Multi-resolution STFT loss<br/>sub band Multi-resolution STFT loss |                Multi-Scale Discriminator                 |
|      HiFi GAN      | adversial loss<br/>Feature Matching<br/>Mel-Spectrogram Loss | Multi-Scale Discriminator<br/>Multi-Period Discriminator |





## 5.2 声音克隆

本课题这部分是在小数据量上进行特定说话人的语音合成，具体流程如下图，在预训练模型上采用微调的方式去训练一个类似于fastspeech2的新声学模型，该声学模型可以生成与训练样本同一音色的音频梅尔谱。

![image-20230529113756158](D:\term4\毕业设计\论文写稿\image-20230529113756158.png)





### 5.2.1 数据集处理

训练数据包含高质量的训练音频以及对应的标签文件。想要得到最优模型在训练时需要高质量的数据来完成模型调优，音频数据的质量决定到声学模型的效果。需要高质量无噪声且大于等于预训练模型的24000高采样率数据音频，wav音频格式是原始数据，无损不压缩恰好符合paddlespeech对格式的需求，音频时长控制在5s以内，也就是一句话的时长。人声数据需要在安静环境下录制，减少噪音产生，混杂进去噪声也要在训练前对音频数据使用软件进行降噪，将影响控制到最小。录制的声音情绪尽量稳定，以说话的文本为主，不要参杂『嗯』『啊』『哈』之类的语气词。

这里根据游戏场景需要录制了一套旅行者荧的数据集，为尽可能符合文本对话，语音数据集范围为日常聊天，其中表达方式符合交流形式，在官方资料语音中进行内置扬声器的采集，录制了300条合适的语音，录制标准如下：

（1）采样率为24kHz；

（2）音频为wav格式；

（3）通道数为单通道；

（4）前后切除多余部分。

![](D:\term4\毕业设计\论文写稿\image-20230528172306767.png)

![image-20230529202658724](D:\term4\毕业设计\论文写稿\image-20230529202658724.png)





```
from paddlespeech.cli.asr.infer import ASRExecutor
import csv
import moviepy.editor as mp
import auditok
import os
import paddle
import soundfile
import librosa
from xpinyin import Pinyin

# 语音转文本
asr_executor = ASRExecutor()
# 实例拼音转换对象
p = Pinyin()
def audio2txt(path):
    # 返回path下所有文件构成的一个list列表
    print(f"path: {path}")
    wavlist = os.listdir(path)
    # 保证读取按照文件的顺序
    wavlist.sort()
    # 遍历输出每一个文件的名字和类型
    for file in wavlist:
        text = asr_executor(
            audio_file=path + '/' + file,
            device=paddle.get_device(), force_yes=True) # force_yes参数需要注意
        textpinyin = p.get_pinyin(text,tone_marks='numbers',splitter=' ')
        words.append(file.split('.')[0] + "|" + textpinyin + "\n")
        print(file)
    out.writelines(words)
    out.flush()
    return words

outtxt = '/home/aistudio/work/labels.txt'
out = open(outtxt, 'w', encoding='utf8')
path = "/home/aistudio/work/"+my_dir+"/"
words = []
audio2txt(path)
```



对应的标签文件依照paddlespeech官方格式是音频名（去掉格式后缀）|拼音（中文）/单词（英文）

中文示例：001|lv3 tu2 zhong1 de1 shou1 huo4

英文示例：LJ001-001|The harvest of the journey



生成labels.txt

- Step1: 生成标准数据集
- Step2: 检查非法数据
- Step3: MFA 对齐
- Step4: 生成时长信息文件
- Step5: 数据预处理
- Step6: 生成训练环境
- Step7: 微调训练
- Step8: 导出静态图模型



### 5.2.2 训练推理

```
/demo/
├── exp ------- 日志文件（可删除）
├── inference ------- 生成的语音模型
│   ├── lumine ------- 生成的语音模型文件（用于合成）
│   └── lumine.zip ------- 语音模型压缩文件
├── output ------- 生成音频的文件夹
│   └── lumine ------- 每个发声器会有对应的文件夹
│       └── xxx.wav
└── work ------- 训练文件夹
    ├── ann_status.json ------- 数据标注状态文件
    ├── labels_lumine.json ------- 语音标注文件
    ├── temp ------- 过渡音频文件夹 存放格式统一后的音频 每次生成标注文件都会清空
    ├── data ------- 音频源文件
    │   ├── lumine
    │   └── lumine.zip
    └── exp_lumine ------- 模型训练文件夹（保存模型后可以删除，但是训练记录也会被删除）
        ├── data ------- step1 生成的文件夹 存放训练音频和标注文件
        │   ├── labels.txt ------- 标注文件
        │   └── XXX.wav ------- 用于训练的音频
        ├── new_dir ------- step2 生成的文件夹 存放mfa需要的音频和标注
        ├── mfa ------- step3 生成的文件夹 存放mfa对齐文件
        ├── dump ------- step5 生成的文件夹 存放数据预处理文件
        ├── output ------- 训练生成的文件 存放语音合成的模型
        │   ├── checkpoints
        │   └── log ------- 用于可视化
        ├── finetune_status.json ------- 模型微调状态文件
        └── generate_status.json ------- 语音合成状态文件
```



```
batch_size = 32
max_finetune_step = 100
learning_rate = 0.001

# start_step 和 end_step 分别控制开始和结束，方便分步调试
finetuneTTS(label_path, exp_name, start_step, end_step,max_finetune_step, batch_size, learning_rate)
```

batch_size指的是数据的个数，等于2的幂次方能达到GPU使用的最大化，降低GPU占用率，batch_size的选取要在GPU承受范围内，录制音频都短于10s，且数据总数大于等于128，在V100 32g训练环境下，batch_size最大能达到128。

在一定范围内epoch越大训练效果越好，但达到一定程度后效果的增长率降低，训练收益下降。epoch过小会导致欠拟合，效果不达标，epoch过大则会导致过拟合，模型的偏差小而方差大，效果误差变大。

max_finetune_step指的是最大训练步数，一个step会将一组batch_size扔进神经网络训练一次，learning_rate作为学习率在刚开始训练时以 0.01 ~ 0.001 为宜。

本文进行微调的数据量远远小于预训练模型的训练数据量，很多训练参数需要做对应的修改和调整。learning_rate微调需要比预训练模型的lr（0.001）小，num_snapshots在paddlespeech中用于保存模型，便于查看合成效果。在数据量、精度与处理数据量速度的均衡考量下，本文这里batch_size设置为32，epoch设置为100，max_finetune_step设置为100，learning_rate设置为0.0001，num_snapshots设置为-1。、

微调好参数调用预处理模型进行训练，训练结束后输入需要生成的文字，生成对应的音频文件

```
text_dict = {
    "01":"Muhe ye",
    "02":"在蒙德，难道每个有鱼游动的地方，都有一个跳动的蹦蹦炸弹吗",
    "03":"即使是自由的风，也有它眷恋的事物吧",
}
import os
# 生成 sentence.txt
text_file = os.path.join(exp_dir, "sentence.txt")
with open(text_file, "w", encoding="utf8") as f:
    for k,v in sorted(text_dict.items(), key=lambda x:x[0]):
        f.write(f"{k} {v}\n")
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
# 配置一下参数信息
model_path = os.path.join(output_dir, "checkpoints")
ckpt = find_max_ckpt(model_path)
```









#### 胡桃声音训练

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
    "01": "大家好，我是 派蒙，今天 天气 很不错啊，大家一起来原神找我玩呀！",
    "02": "囊 代 右，派蒙 不 是 应急 食品。",
    "03": "我是kong la ，这是 我 的 毕业 论文 语音 部分，谢谢各位 老师"
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

ipd.Audio(os.path.join(wav_output_dir, "01.wav"))
```

```
ipd.Audio(os.path.join(wav_output_dir, "02.wav"))
```

```
ipd.Audio(os.path.join(wav_output_dir, "03.wav"))
```



### 5.2.3 深度学习合成的语音结果

将两个人物之间的对话分为两份数据集都进行了训练，以第二个文本"在蒙德，难道每个有鱼游动的地方，都有一个跳动的蹦蹦炸弹吗"生成音频为例，如下图展示了派蒙和旅行者空LA的声线波形，再提取对应的频谱图，明显可以看出模型输出了符合文本的音频结果。

![image-20230603210805565](D:\term4\毕业设计\论文写稿\image-20230603210805565.png)



![image-20230603211218745](D:\term4\毕业设计\论文写稿\image-20230603211218745.png)

p2

![image-20230603204426309](D:\term4\毕业设计\论文写稿\image-20230603204426309.png)

l2

![image-20230603204452187](D:\term4\毕业设计\论文写稿\image-20230603204452187.png)

p2

![image-20230603204645228](D:\term4\毕业设计\论文写稿\image-20230603204645228.png)

l2

![image-20230603204703002](D:\term4\毕业设计\论文写稿\image-20230603204703002.png)





### 5.2.4 语音质量评价

训练后依照文本生成数据集数据同种声线的音频时，听起来达到非常高的自然度，而且能做到没有读错或者漏掉文本的错误。考虑到个人会因主观因素对生成语音期望较低而造成数据不严谨，所以本文该部分设置三组测评人员对合成音频以及数据集音频进行评价，测试环境为安静无背景噪音的空教室。本文共邀请9位同学作为测试人员分为三组进行语音评价打分，分数制度为1到10分，第一组由3位长时间在线该游戏玩家组成；第二组由3位短时间在线该游戏玩家组成；第三组由3位从未接触过该游戏及语音的同学组成。

| 分组   |      | 打分 |      | 平均分 |
| ------ | ---- | ---- | ---- | ------ |
| 第一组 | 6    | 9    | 7    | 7.3    |
| 第二组 | 9    | 6    | 8    | 7.6    |
| 第三组 | 9    | 8    | 7    | 8      |

由表可知，在本模型中合成音频比起真实人工配音质量稍差。个人推测数据量较少且训练次数不够，完全依赖预训练模型也存在较大影响。从平均分上看，第三组测试人员对合成音频认可度较高，第一组测试人员长期接触游戏中的语音数据，对合成音频中细节瑕疵和整体较为敏感，所以接受度低于其他两组。



## 5.3 本章小结

在本章首先简单介绍了语音合成中的历史，并详细阐述了文本前端 、声学模型和声码器三个主要模块。在5.2节中采用数据集在TTS3预训练模型上进行了训练，并介绍了训练的参数说明。最后经过训练生成音频，通过两种声线合成波形和频谱图之间进行简单对比，引入9位测试人员，在安静无噪声环境下听完生成音频和训练的数据集，对说话人声线和自然度进行评价打分。

# 第六章 总结与展望

## 6.1 总结

在杜振龙老师的指导下我完成了毕设基于2D地图探索的轻量级游戏渲染的设计与实现，年初时和杜老师讨论并确定了毕设的课题，当时毕设选题有考虑想做嵌入式相关的智能桌面设备，在移动互联网这门课上展示过复刻好的别人开源硬件成品，后续也建了个仓库打算将MPU6050更换为PAJ7620u2芯片接入手势检测，再在上位机上进行新的应用开发，在那个时候萌发了想要做款小游戏的想法。毕设选题也考虑过将计算机专业写作这门课上交的论文再次进行详细研究，主要参考了GAN图像修复模型**[43]**以及MaskR-CNN实例分割模型**[44]**，专门应用在图像修复领域，能做到8k高清级别的修复，在学校厚学楼处制作了部分数据集，将人物或者特定物品进行消除，当时对算法的效果十分惊奇。但最后还是选择了现在的课题，一方面对制作游戏产生了浓厚的兴趣，亲自动手做出能交互的游戏有成就感，另一方面是对网页h5游戏前景比较看好，以前依赖flash的游戏逐渐销声匿迹，近些年来合成大西瓜、羊了个羊的网页游戏短时间爆火足以看出技术发展跟上时代的重要性，最主要的还是想去参加当时刚举办的2023全球游戏创作节。

敲定课题后就开始进行游戏的设计，一开始准备单靠前端html、css和javascript以及three.js库来实现，后来发现技术难度过大且作为项目完全不现实，与此同时认识了从事游戏行业的9梦菌，在他建议下开发2D游戏用cocos比较多，于是转变为基于游戏引擎进行游戏开发。在这期间逐步调试各种功能组件，在仓库上更新记录自己的学习笔记，例如游戏图形学中推导贝塞尔曲线公式，在这方面跟算法一样，利用公式结合代码实现特定的功能，而且学习了Typescript语言，作为Javascript的超集更为简洁。复刻了多个小游戏，也踩过很多坑，比如cocos键盘监听、物理碰撞移动以及预设体问题等等，这些问题更多的是去看官方的api文档，逐行调试代码进行改动。

后期想了想单做游戏不太合适，光秃秃的文本显示远不如剧情中加上配音能给人带来沉浸感。于是添加了语音合成的算法设计，通过过往人物的配音音频作为数据集进行训练，对给定的文本输出同种声线的音频，在尝试训练了各种方案，最终选择了百度paddlespeech便于学习与使用。看了文本前端、声学模型和声码器部分发现AI算法NLP领域知识也是前人知识不断更新迭代到现在，碰巧的是最近有篇关于利用语音技术在加速医学mRNA合成**[45]**的论文上了Nature封面，其中音标部分类似于本论文中的labels.txt的生成。



## 6.2 展望

毕设从介绍基础知识再到游戏完整的实现最后实践声音克隆，从涉及的知识点与细节性讲解目前来看毕设还是存在一些不足。游戏后期可以添加道具、剧情主线以及boss战等等，今后会用unity结合c#对该项目重新进行开发，拓展更多的可能性。

语音合成部分会采取构建新的模型，针对特定声线生成完全贴合配音的音频，遇到多语言场景也能处理，对配音产业进行优化，避免因特殊意外而无法补上音频的情况产生，目前也是越来越多的人在进行AI声线唱歌的研究。

在大多数语音合成场景中，合成音频基本上都是感情起伏过少，无法让用户感受到合成音频所想要表达的情绪，后期打算加入音频情感模块，使声线更加富有表现力。



# 参考文献

[1] Guzhov, A., Raue, F., Hees, J., & Dengel, A.R. (2021). AudioCLIP: Extending CLIP to Image, Text and Audio. ArXiv, abs/2106.13043.

[2] Kong, Q., Cao, Y., Iqbal, T., Wang, Y., Wang, W., & Plumbley, M.D. (2020). PANNs: Large-Scale Pretrained Audio Neural Networks for Audio Pattern Recognition. IEEE/ACM Transactions on Audio, Speech, and Language Processing, 28, 2880-2894.

[3] Gong, Y., Chung, Y., & Glass, J.R. (2021). AST: Audio Spectrogram Transformer. ArXiv, abs/2104.01778.

[4] Gemmeke, J.F., Ellis, D.P., Freedman, D., Jansen, A., Lawrence, W., Moore, R.C., Plakal, M., & Ritter, M. (2017). Audio Set: An ontology and human-labeled dataset for audio events. 2017 IEEE International Conference on Acoustics, Speech and Signal Processing (ICASSP), 776-780.

[5] Piczak, K.J. (2015). ESC: Dataset for Environmental Sound Classification. Proceedings of the 23rd ACM international conference on Multimedia.

作者.文献题名[EB/OL].http://…，日期．

百纳知识.浅析信号处理：人们认识信号本质的大飞跃[EB/OL].[2018-08-14].https://www.sohu.com/a/246972969_607269.

Marple L .Computing the discrete-time "analytic" signal via FFT[J].IEEE Transactions on Signal Processing,1999.

Qian S , Chen D . Joint time-frequency analysis[J]. IEEE Signal Processing Magazine, 1999, 16(2):P.52-67.

 frostime.时频分析之STFT：短时傅里叶变换的原理与代码实现（非调用Matlab API）[EB/OL].[2020-06-20]https://blog.csdn.net/frostime/article/details/106816373.

Heinrich.傅里叶分析之掐死教程（完整版）[EB/OL].[2014-06-06].https://zhuanlan.zhihu.com/p/19763358.

Yvon Shong.深入理解快速傅里叶变换[EB/OL].[2016-04-09].https://www.yvonshong.com/2016/04/09/fft/.





# 致谢

时间如手中沙，在毕业设计完成之际，才恍惚明白大学生活已近尾声。回顾大学四年心中感慨万千，这些年我收获良多，有敬爱的师长，有关心的父母，有热心的技术人员，在此向大家表示由衷的感谢！

首先感谢杜振龙老师对我的关心并给予的帮助指导和鼓励，能在求学生涯中碰到杜振龙老师这样认真负责的老师非常幸运。接着要感谢父母的照顾，能提供学习的各种帮助。另外还要感谢毕设过程中提供知识参考的9梦菌，unity官方人员在spine技术上的指点，百度官方算力的调用以及米哈游官方素材的授权使用。感谢参与配合语音质量评价的同学。

最后感谢答辩组和所有给过我帮助和意见的老师，给我提出宝贵的意见，谢谢你们！
