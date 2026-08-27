# RPA+AI，才是真正能让你躺平的自动化真神。

> 公众号: 数字生命卡兹克
> 发布时间: 2025-05-08（微信原文）
> 剪藏来源: https://cloud.tencent.cn/developer/article/2518639（全文转载，图片为外部链接）

---

想写关于RPA的话题很久了。

这次终于找到了一个机会，看到了一些转折点。

是因为昨天打开影刀RPA，想搓一个自动跑图的自动化RPA流程的时候，发现他们更新了一个新功能。

![Image](https://developer.qcloudimg.com/http-save/yehe-11609394/be5f8e1b6ea8167fdda57542f37ec575.png)

魔法指令。

这个功能的作用非常简单，就是你现在可以通过AI，来用嘴搭建一套关于网页和数据的RPA流程了。

![Image](https://developer.qcloudimg.com/http-save/yehe-11609394/5a843f43f2b16e50427a844ff6bca914.png)

这是我觉得，一个非常棒的转折点。

很多朋友可能不知道这意味着什么，也有可能完全不知道RPA是什么。

所以我觉得在给大家看这个魔法指令功能之前，还是先跟不太了解RPA的朋友，说一下他跟Agent的区别和意义。

坦率的讲，最近我听Agent已经听得耳朵旁有点起茧了，特别是MCP火了之后，突然感觉万物皆可Agent了。

听的我快有一点叛逆了。。。

当然，不是因为Agent不牛逼，我很喜欢Agent，Agent很牛逼，我也认为，必然是未来。

只不过当下的时间点，我觉得很多人都已经陷入了某种AI幻觉里，觉得在当下的时间里，在当前的模型能力下，一切都可以Agent，一切都该Agent，一切都是Agent牛逼。

但真的去做落地项目，做业务流程，做一些自动化任务的朋友都知道，你让Agent做着做着，就会从“卧槽牛逼哎，他自己看到错了自己去别的地方找哎”，变成“尼玛怎么又崩了？”、“怎么这一步又理解错了？”、“神经病吧。”

尤其是，越是复杂的流程，越是讲究高精度、不能出错、重复运行的业务，一旦Agent的推理链条超过十步，它有的时候就越来越像是在抽卡。

这个痛点，我在好几个访谈里都说过。

比如你现在做一个复杂、重复的操作流程，要Agent完成20个步骤，哪怕每一步成功率99%，那整体成功率是多少？

0.99的20次方，大概是82%。

听起来不低是吧？但是这个就代表着，每5次任务，就会有一次失败。

而且我给的是每一步99%的成功率，其实大家都知道，这个成功率很多时候是不可能的。

如果给到每一步90%的成功率呢。

![Image](https://developer.qcloudimg.com/http-save/yehe-11609394/83d1c4965490c58bb1b725f55016ca2e.png)

10次只能成功一次。

你告诉我，这玩意在真正需要重复、自动化、稳定运行的生产环境里，咋用嘛。

这个逻辑，在今天很多人聊Agent的时候，是不被认真讨论的。

大家都沉迷在AI帮我做了一件很酷的事的表演性场景里，但是却并不在乎，这个事能不能每天执行100次、1000次、10000次，连续跑3个月不出错。

我之所以这么说，是因为，我真的有一堆浪费时间的重复性任务，是要交给程序自动跑的，是不是AI其实我根本不在乎，我只希望，能稳定、能十年如一日、不出错的运行。

所以，在这种场景上，根本就不应该用什么Agent，而是上，RPA。

RPA，Robotic Process Automation。

![Image](https://developer.qcloudimg.com/http-save/yehe-11609394/8ed47bea157eb694436e873c3e7251ad.png)

我不知道大家有没有玩过一些手游，我自己十年前玩碧蓝航线和阴阳师的时候，每天都在里面肝肝肝，我还记得玩阴阳师的时候我还在上大学，为了刷升一个六星茨木童子，每天一边看剧一边刷狗粮。

![Image](https://developer.qcloudimg.com/http-save/yehe-11609394/079fe9f5915a8047e633d5bcd3ecfa2e.jpg)

狗粮就是要升级式神的素材，得到的途径很简单，就是刷副本。然后副本里面是可以开自动的，一把我记得可能几分钟，但是结算和重新开始，还是得手动点几下。

我刷了好几天，实在是烦炸了。

于是就装了一个类似于按键精灵的APP，具体名字我忘了，他的作用，就是记录我点了屏幕哪里，隔了多少时间后又点了什么，是按什么顺序点的。

从而实现，可以把我解放出来，完全自动化的刷狗粮。稳定、重复、日复一日的运行。

于是，在刷了将近一个月之后，我的式神质量突飞猛进，吊打身边一众朋友，甚至还能在我们那个区竞技场打到排行前十。。。

这个东西，我们一般称为脚本。

同时，也是RPA最最最最初级的用法。

真正的RPA，比这玩意高级多了，涉及到各种数据的处理和流转、规则的判断、自动化点击操作等等。

![Image](https://developer.qcloudimg.com/http-save/yehe-11609394/fa762f0f0ac6646f9464abb9807abacc.png)

过去大家一提RPA就觉得老、慢、土、企业服务感太强，觉得AI才是未来。

但现在你回头看，Agent像是带情绪的实习生，做事随机、有创意、有想法，但经常会搞砸一些细节。

而RPA，像是老成稳重的机器人工人，没啥脑子，但给个流程它就一遍遍照着跑，重复、稳定、精准、不多想。

国内我一直觉得最有代表性的RPA产品，就是影刀。

![Image](https://developer.qcloudimg.com/http-save/yehe-11609394/8b9b7e8ee2bd76f18fd4f53981d9eb53.png)

网址在此：https://www.yingdao.com/

他们自己也总结了一个图，这个图我深表赞同。

![Image](https://developer.qcloudimg.com/http-save/yehe-11609394/e773f748a3b1add4bb1bf4aaec2f7ece.png)

Agent非常酷，像Manus、DeepResearch等等，几乎都是我最常用的产品，特别是OpenAI那个DeepResearch，每个月150次现在甚至都不够我用，我吹了无数次，200刀Pro会员最大的价值所在，我非常的尊敬他们，我也觉得它们很强。

但是场景不一样，在重复性场景上，他们还是太有个性，太聪明了。

在很多时候，自动化真正需要的，是 更稳、更可控、更可复用 。

而不是更聪明。

毕竟，做这种极度重复性的工作，我要的就是你稳定的十年如一日的运行，你要聪明干嘛，你聪明但不靠谱，还不如不聪明。

但是RPA，我认为一直有一个非常大的问题。

就是上手太难了。

你即使不懂AI，不懂编程，你依然还是可以用Cursor、Trae搓出一个小产品，还是可以用DeepResearch给你生成一篇很牛逼的几万字的报告。

但是RPA这玩意，你不会，那就是真不会。

比如我们想做一个网页的自动化，你打开界面，你如果没用过，你是真的有可能懵逼的。

![Image](https://developer.qcloudimg.com/http-save/yehe-11609394/ec0ea942e0ee3a87345b238557c039f2.png)

你想从抖音扒个评论，这是RPA做好的详细流程，别说搭了，大概率，你看都看不懂。

![Image](https://developer.qcloudimg.com/http-save/yehe-11609394/7759fa85444bd129b19084dc935ccc4b.png)

你要搭流程，要画模块，要定义变量，要像程序员一样思考流程逻辑。

后来影刀这么多年一直都在努力降低门槛，加入了更多可视化、模块化、拖拽式逻辑配置。

但，还是太复杂了。

你本质上还是在做开发。

只是形式换了。

这两年，AI Coding飞速发展，我其实一直想看到，像影刀这种产品，能出一个用AI来搭建RPA流程的功能。

AI本质上就是降维，就是技术平权，RPA很有用，但是太难，大家用不会搭不来，那为什么不能有，用嘴来跟AI提需求，然后转变成RPA的流程呢？

直到现在，在等了半年多以后，我终于看到了。

虽然他现在还是只能用AI生成网页和数据处理的RPA流程，还不能生成桌面端跨软件比如微信的RPA流程，但是至少，我看到了一个非常棒的转折点。

RPA+AI，才是自动化任务的YYDS。

你们把影刀下载下来安装，就能在主界面上看到新建，点击新建 - 新建PC自动化应用，进入到流程搭建页面。

![Image](https://developer.qcloudimg.com/http-save/yehe-11609394/2356abfc0309ee464a0b7c5227dcd274.png)

你就能在标准指令里面，看到这个彩色的魔法指令。

![Image](https://developer.qcloudimg.com/http-save/yehe-11609394/fc552ef322aeca327ace0921fe503a58.png)

这个就是用嘴搭流程的功能。

不过在用之前，一定记得现在主界面 - 头像 - 工具 - 自动化插件那。

![Image](https://developer.qcloudimg.com/http-save/yehe-11609394/b22eb6b56aa89a207a224598d3759858.png)

先把各种浏览器的插件给装上，要不然后续运行不成功。

![Image](https://developer.qcloudimg.com/http-save/yehe-11609394/96ec8bb025142762a92e634756ee8c8b.png)

现在，我们举个小例子，比如，想做一个自动化流程用于我自己的学习。

流程是每天在小红书上搜索关键词“AI”，然后把前50个笔记都爬下来，存到一个excel里，再按照一定的数据公式，找出优质内容，把那部分优质的笔记提取出来存储，最后扔到飞书的多位表格里。

我们直接就可以点开魔法指令，然后直接点击进入浮窗。

![Image](https://developer.qcloudimg.com/http-save/yehe-11609394/dc5923894f5c0ccb285bca2478a8ab4d.png)

你就会发现，她变小了，然后缩到屏幕的一边了。

![Image](https://developer.qcloudimg.com/http-save/yehe-11609394/152440192edfb01425b58368b975a082.png)

默认就是网页自动化，所以我们不需要修改，然后打开浏览器（推荐还是Chrome），打开小红书的网页。

现在，你的界面应该长这个样子，左边是小红书的网页，右边是影刀的魔法指令浮窗。

![Image](https://developer.qcloudimg.com/http-save/yehe-11609394/46bbc6768dc5466c1f225cfbabf4b43e.png)

然后，我们就直接用嘴，来写我们的指令。

不过影刀跟所有的AI产品都有一个不一样的点，就是他有一个必须的步骤，叫做捕获元素区。

比如我们说，在搜索栏里输入AI，然后点击搜索按钮。

这个Prompt其实不够精准，可能后续运行时会出现一些问题，所以，你需要先捕获元素区，非常明确的告诉它，这是搜索栏，这是搜索按钮。

我们直接点击左下角这个icon。

![Image](https://developer.qcloudimg.com/http-save/yehe-11609394/895ec44e5dfc397b5ce102368e97768e.png)

你就会发现，你的屏幕变绿了，你可以，在页面上框选各种区域了。

![Image](https://developer.qcloudimg.com/http-save/yehe-11609394/55d958de5b3eeada9c9423db17658a93.gif)

我们把鼠标放在搜索框的选区上，然后按下Ctrl+Shift再加鼠标左键单击，它就会给你个确认，你点对勾就行。

然后你就会发现，这个选区，出现在你的对话框里面了。

![Image](https://developer.qcloudimg.com/http-save/yehe-11609394/ea4659af0b13a0b7c083cf11319dbc6e.png)

这个就是一个元素块，我们现在，可以用嘴，来告诉AI，应该对这个元素块进行什么操作了。

比如我就会说。在输入框中输入“AI”。

![Image](https://developer.qcloudimg.com/http-save/yehe-11609394/535725ac40ee9a027d1309be2d29988e.png)

输入完以后，下一步就是点击搜索按钮进行搜索了。我们继续如法炮制。选中选区。

![Image](https://developer.qcloudimg.com/http-save/yehe-11609394/1add9711b6ab88995f69f9d00837f88a.png)

然后在prompt里面写，点击块元素（就是搜索按钮）进行搜索。

![Image](https://developer.qcloudimg.com/http-save/yehe-11609394/9a1a62538c8339c604474ef3890cb927.png)

后面就都是一样的流程了，圈中整个笔记区，让他按照笔记链接、作者、标题、正文、点赞数、收藏数、评论数等等来进行爬取，给我存到一个本地的excel里。

![Image](https://developer.qcloudimg.com/http-save/yehe-11609394/70463636df5a8f51b7d74cc138dd29b7.png)

![Image](https://developer.qcloudimg.com/http-save/yehe-11609394/c477ffe38028ef3289aa78f65d7f8263.png)

这样，一个用AI生成RPA的Prompt就写完了，会跟你直接对话，有一点不一样。写完以后，我们可以再点击一下，他们自己的优化提问，提高成功率。

优化完以后，就会变成这个格式，正规很多。

![Image](https://developer.qcloudimg.com/http-save/yehe-11609394/77da97ebeb649edbe1ca34993829ae12.png)

一切完毕，我们把他，发送出去就行。

过了一会，就会生成魔法指令的窗口，有提示需要安装依赖库的，你就点击安装就行。

这个基本就代表了，RPA流程已经生成完了，哪些选项可以不用管，保存到本地的文件夹路径，是可以手动修改的。

![Image](https://developer.qcloudimg.com/http-save/yehe-11609394/6a7f64a3ab69fa3a8328cba10ed24316.png)

我手动改成到了D盘。

![Image](https://developer.qcloudimg.com/http-save/yehe-11609394/d95ae1678484a0f51e16ca9501524c8c.png)

然后，我们点击左下角的那个，运行指令，测试一下。

一次，直接完美的成功了。我直接放我的原始未加速录屏，大家可以感受一下。

真的，看RPA干活，是一种享受。

再来看看本地excel结果。

![Image](https://developer.qcloudimg.com/http-save/yehe-11609394/6d2b39cfee9a79ce6af9a82545bc9570.png)

完美无瑕。

而且，这个东西，是保存了以后，可以十年稳定如一日的运行的（前提是小红书不改界面），做一次，以后可以一直用。

而且不只是10个，我们完全可以让他存50个笔记，100个笔记，200个笔记。每天抓取一次，这个对于自己学习作用有多大，不言而喻。

![Image](https://developer.qcloudimg.com/http-save/yehe-11609394/4226f51d62d22254d0ef2e04eb15f6bb.png)

生成完的流程，大家一定要记得，点击确定，因为他只是流程，还不算应用。

![Image](https://developer.qcloudimg.com/http-save/yehe-11609394/1c2662f6d8a6a541c76f1280ab9d06ca.png)

点击确定后，就会回到我们的主界面。

![Image](https://developer.qcloudimg.com/http-save/yehe-11609394/7c1215ecfa5fa1c01dbcce8c1d7e4988.png)

记得先拖一个网页自动化里的打开网页，到主流程中，以后一运行，就会自动打开小红书了。

![Image](https://developer.qcloudimg.com/http-save/yehe-11609394/865d9b20550a56929483d8d54bbda213.png)

刚才保存下来的内容，我们如果想自动再让RPA帮我们做一轮数据筛选。就可以继续点击魔法指令，但是这一会，用数据处理，帮我们搞excel。

![Image](https://developer.qcloudimg.com/http-save/yehe-11609394/002d6e994f60774ae94641cc2bc158a3.png)

在得到结果之后，把输出文件可以自定义一个。

![Image](https://developer.qcloudimg.com/http-save/yehe-11609394/90d9c40ef90e39adceb1c5eb37ca1437.png)

我们再运行测试一下。结果出现了BUG，但是不用慌，直接点击智能修复就行。

![Image](https://developer.qcloudimg.com/http-save/yehe-11609394/109ae5ba64ae7cb3d584fb17222b3d20.png)

然后继续，成功了，筛选出来了22条，Nice。

![Image](https://developer.qcloudimg.com/http-save/yehe-11609394/2bbc4ece1f7353c16afa1246566d97ca.png)

最后，你甚至可以，把这些处理完的数据，直接自动化的，上传到飞书多维表格里。

![Image](https://developer.qcloudimg.com/http-save/yehe-11609394/944cfa2065872c0f841a72aebb523cdc.png)

以后，每天，你只要需要打开影刀，运行一下，你就有了，全新的AI领域的小红书爆款内容。

![Image](https://developer.qcloudimg.com/http-save/yehe-11609394/e209cc7f3c1816fbfb26d9f0540ef5c5.png)

你甚至还可以进一步当个懒癌，跟触发器打通，每天定时自动运行。。。

![Image](https://developer.qcloudimg.com/http-save/yehe-11609394/02b2a2a076182109dd6549cabcf68245.png)

除了这个小红书自动爬取之外。

我们还手搓了几个其他的流程，比如每次新生图模型出来，我用我的excel测试集去测，都巨麻烦，因为新模型几乎不可能开放API。

那现在，直接可以用嘴搓个自动生图流程，比如我就搓了个即梦的。

![Image](https://developer.qcloudimg.com/http-save/yehe-11609394/986eabf2e95f84e1d1dd366c0f8211be.png)

直接批量生图。

![Image](https://developer.qcloudimg.com/http-save/yehe-11609394/4b4da4c026cd0727f425e0594fa98b2d.png)

然后塞到飞书里，甚至我打算，把生视频的也做了。

![Image](https://developer.qcloudimg.com/http-save/yehe-11609394/932f33622a316db147131dbee44cd3ae.png)