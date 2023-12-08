# 网易云歌词爬虫

本爬虫可以方便地爬取歌词，并根据结果，对高频歌词进行词云展示，具体参数
在settings里设置。


# 网易云热歌都唱了什么？记一次python练习

> 关键词：数据可视化、数据分析、Python爬虫

## 一

最近在学python，虽然还只是一点皮毛，但终于可以做做自己的小项目了。学习任何一门语言的最好方法就是实践，能完成一个小项目，还是很有成就感的。

这次我想知道，网易云的热门歌曲都唱了什么。

想要获得热歌，我的第一个想法是可以爬爬网易歌单，幸运的是，网易云有一个歌单（云音乐热歌榜）专门收录当下的热门歌曲。

而想知道唱了什么，则可以从歌词入手，在歌单页面下有一个列表，每一个歌名都有链接，可以通过循环的方式获取每一个歌曲页面，并想办法获得歌词信息。因为无法直接在歌曲页面上爬取歌词，就需要寻找网易云的api，还好，这个也没花太久时间。需要注意的是，每一个歌曲都有唯一的id，可以通过这个id来指定歌词的链接。最后爬取的结果是一张表，有200首歌曲，涵盖的信息分别为id、歌名、链接、歌词，保存为csv到本地。

这个项目一共用到这些库：

BeautifulSoup、lxml、requests、wordcloud、pkuseg、matplotlib、re、pandas。

其中BeautifulSoup、lxml、re用于网页解析或字符匹配，requests用于获取网页，pandas用于数据处理，wordcloud库用于制作词云，pkuseg用于分词，matplotlib用于加载图像。

从上面的思路出发，我先写了一个爬虫脚本。

爬虫写好后，再写一个主程序，就可以运行爬虫，下载歌单信息。

保存的csv大概是这样的:

![image-20200223150250918](%E7%BD%91%E6%98%93%E4%BA%91%E7%83%AD%E6%AD%8C%E9%83%BD%E5%9C%A8%E5%94%B1%E4%BB%80%E4%B9%88%EF%BC%9F%E8%AE%B0%E4%B8%80%E6%AC%A1Python%E7%BB%83%E4%B9%A0.assets/image-20200223150250918.png)



接下来写了数据可视化的程序。

最后用主程序把爬虫和数据分析链接起来。经过好一番调试，终于跑了起来。

以下是实验的结果，生成的词云均通过停词表过滤。

![image-20200224212514172](%E7%BD%91%E6%98%93%E4%BA%91%E7%83%AD%E6%AD%8C%E9%83%BD%E5%9C%A8%E5%94%B1%E4%BB%80%E4%B9%88%EF%BC%9F%E8%AE%B0%E4%B8%80%E6%AC%A1Python%E7%BB%83%E4%B9%A0.assets/image-20200224212514172.png)

可以看到排名靠前的词中有非常多感染力强的的拟声词或语气词，这里列举一下：

> doo、oh、ya、da、yeah

为啥da这个词会这么多呢？查看文件发现了这么一首歌：

![image-20200224002730017](%E7%BD%91%E6%98%93%E4%BA%91%E7%83%AD%E6%AD%8C%E9%83%BD%E5%9C%A8%E5%94%B1%E4%BB%80%E4%B9%88%EF%BC%9F%E8%AE%B0%E4%B8%80%E6%AC%A1Python%E7%BB%83%E4%B9%A0.assets/image-20200224002730017.png)

...

这也许说明热门歌曲之所以热门，是因为其歌曲含有许多较为原始的发声吧，而这些发声虽然原始却很能引人注意，带动情绪。

那么除去了这些语气词后呢？

![image-20200224212840916](%E7%BD%91%E6%98%93%E4%BA%91%E7%83%AD%E6%AD%8C%E9%83%BD%E5%9C%A8%E5%94%B1%E4%BB%80%E4%B9%88%EF%BC%9F%E8%AE%B0%E4%B8%80%E6%AC%A1Python%E7%BB%83%E4%B9%A0.assets/image-20200224212840916.png)

可以看到，“baby”和“世界”是出现最多的，其次还有“时间”、“爱情”、“也许”、“未来”、“离开”。



**至于有什么含义，我也没有太好的想法，大家自由发挥吧，欢迎留言评论。**

前20词汇排名：

| 前1-10  | 次数 | 前11-20 | 次数 |
| :------ | ---: | :------ | ---: |
| baby    |   97 | love    |   50 |
| 世界    |   68 | time    |   48 |
| 时间    |   68 | night   |   42 |
| 爱情    |   64 | running |   40 |
| 也许    |   58 | yeah    |   40 |
| 未来    |   58 | 回忆    |   39 |
| ma      |   56 | 北京    |   39 |
| 离开    |   55 | 想要    |   39 |
| move    |   54 | body    |   39 |
| dancin' |   53 | да      |   39 |





## 二

在完成上面的小项目后，我又不满足于此了。我想获得更多歌曲的信息。

通过网易云歌单搜索可以获得歌单列表，这是歌单的歌单。如搜索“摇滚”，将会得到成千上万条歌单结果，并且是按照收藏人气排名的。通过网易云api搜索歌单列表，得到一个保存歌单的表，再通过上文方法循环爬取数据。最终可得到上万首歌曲歌词信息，不过需要注意的是，同一首歌可能出现在不同歌单里，因此出现重复歌曲时选择跳过。

最后所有参数在settings.py里设置，以方便多测试几个类别。我终于可以知道“摇滚”、“民谣”、“说唱”都唱了什么了。

以下是我实验的结果。

### 摇滚

爬取了“摇滚”歌单搜索结果的前50个歌单（按收藏人气排名），一共有6215首歌，其中无歌词的歌曲有478首，因此有效数据为5737首。

![image-20200224220812489](%E7%BD%91%E6%98%93%E4%BA%91%E7%83%AD%E6%AD%8C%E9%83%BD%E5%9C%A8%E5%94%B1%E4%BB%80%E4%B9%88%EF%BC%9F%E8%AE%B0%E4%B8%80%E6%AC%A1Python%E7%BB%83%E4%B9%A0.assets/image-20200224220812489.png)



去语气词后：

![image-20200224221356112](%E7%BD%91%E6%98%93%E4%BA%91%E7%83%AD%E6%AD%8C%E9%83%BD%E5%9C%A8%E5%94%B1%E4%BB%80%E4%B9%88%EF%BC%9F%E8%AE%B0%E4%B8%80%E6%AC%A1Python%E7%BB%83%E4%B9%A0.assets/image-20200224221356112.png)

看来“时间”和“爱”是摇滚里出现最多的，此外还有“baby”、“回来”、“生活”、“感受”、“心”、“夜晚”，这是英文的语境。中文歌词不太明显，我们之后做进一步处理。从表中看去，“爱”的频次明显高于其他，不得不说，搞摇滚的对**爱**真的很执着。

前20词汇：

| 前1-10 | 次数 | 前11-20 | 次数 |
| :----- | ---: | :------ | ---: |
| love   | 4926 | home    | 1221 |
| time   | 3199 | eyes    | 1214 |
| baby   | 2081 | 世界    | 1185 |
| feel   | 2072 | find    | 1179 |
| back   | 2035 | long    | 1152 |
| life   | 1894 | mind    | 1140 |
| heart  | 1714 | good    | 1113 |
| day    | 1570 | man     | 1010 |
| night  | 1553 | run     |  939 |
| world  | 1405 | tonight |  932 |



我想了解中文歌曲的情况，因此尝试在原有基础上去除英文词汇：

![image-20200225013058240](%E7%BD%91%E6%98%93%E4%BA%91%E7%83%AD%E6%AD%8C%E9%83%BD%E5%9C%A8%E5%94%B1%E4%BB%80%E4%B9%88%EF%BC%9F%E8%AE%B0%E4%B8%80%E6%AC%A1Python%E7%BB%83%E4%B9%A0.assets/image-20200225013058240.png)

| 前1-10 | 次数 | 前11-20 | 次数 |
| :----- | ---: | :------ | ---: |
| 世界   | 1185 | 希望    |  358 |
| ない   |  695 | 生命    |  358 |
| 时间   |  648 | 地方    |  356 |
| って   |  515 | 自由    |  340 |
| 离开   |  505 | 城市    |  328 |
| 生活   |  462 | 明天    |  305 |
| 永远   |  432 | 天空    |  300 |
| 等待   |  389 | 这是    |  294 |
| 未来   |  388 | して    |  284 |
| 忘记   |  365 | 孤独    |  278 |

日文我看不懂，机器翻译又有失准确性，还是略过好了。

“时间”和“世界”排在前，其次是“离开”和“生活”。

我特地看了“离开”的语境，发现大多是描述人与人之间的离开：

![image-20200225014309542](%E7%BD%91%E6%98%93%E4%BA%91%E7%83%AD%E6%AD%8C%E9%83%BD%E5%9C%A8%E5%94%B1%E4%BB%80%E4%B9%88%EF%BC%9F%E8%AE%B0%E4%B8%80%E6%AC%A1Python%E7%BB%83%E4%B9%A0.assets/image-20200225014309542.png)

而“生活”则是各种各样的，“生活”太丰富，这里太挤就不举例的。

他们在“等待”，查看歌词才发现绝大多数都是在等待着某人。

他们谈论了许多次“未来”，又害怕“忘记”自己，忘记了对方和方向。怀揣“希望”又或者只是“希望”着什么，身处“城市”里感受“孤独”。

他们关心“生命”，也像每一个摇滚的人一样，关心“天空”和“自由”，却对明天有着不一样的看法，有的乐观，有的充满疑惑。

![image-20200225020144883](%E7%BD%91%E6%98%93%E4%BA%91%E7%83%AD%E6%AD%8C%E9%83%BD%E5%9C%A8%E5%94%B1%E4%BB%80%E4%B9%88%EF%BC%9F%E8%AE%B0%E4%B8%80%E6%AC%A1Python%E7%BB%83%E4%B9%A0.assets/image-20200225020144883.png)



### 民谣

爬取了“民谣”歌单搜索结果的前50个歌单（按收藏人气排名），一共有6506首歌，其中无歌词的歌曲有564首，因此有效数据为5942首。

未去语气词的词云：

![image-20200225004811710](%E7%BD%91%E6%98%93%E4%BA%91%E7%83%AD%E6%AD%8C%E9%83%BD%E5%9C%A8%E5%94%B1%E4%BB%80%E4%B9%88%EF%BC%9F%E8%AE%B0%E4%B8%80%E6%AC%A1Python%E7%BB%83%E4%B9%A0.assets/image-20200225004811710.png)

过滤语气词后的词云：

![image-20200225005756964](%E7%BD%91%E6%98%93%E4%BA%91%E7%83%AD%E6%AD%8C%E9%83%BD%E5%9C%A8%E5%94%B1%E4%BB%80%E4%B9%88%EF%BC%9F%E8%AE%B0%E4%B8%80%E6%AC%A1Python%E7%BB%83%E4%B9%A0.assets/image-20200225005756964.png)

| 前1-10 | 次数 | 前11-20 | 次数 |
| :----- | ---: | :------ | ---: |
| 世界   | 1760 | 喜欢    |  999 |
| love   | 1610 | 永远    |  992 |
| 时间   | 1450 | 远方    |  938 |
| 姑娘   | 1435 | 孤独    |  937 |
| 生活   | 1316 | 城市    |  916 |
| 离开   | 1066 | 故事    |  902 |
| 时光   | 1054 | 青春    |  898 |
| 爱情   | 1051 | 也许    |  886 |
| 再见   | 1039 | 地方    |  856 |
| 回忆   | 1024 | 记得    |  845 |

民谣歌里提到了最多次“世界”和“love”，“姑娘”居然能排在第四的位置，根据我直男的第一反应，唱民谣的肯定大都是大老爷们，而且还是很想要交女朋友的那种。

从结果中可以看出，与时间有关的词汇非常多，有“时间”、“时光”、“回忆”、“永远”、“岁月”、“记得”，在词云中还可以看到“忘记”、“日子”、“匆匆”、“消失”、“未来”、“从前”、“遗忘”、“忘记”。看来歌手对于时光也有一些执着，有的人想忘记，有的人却在努力回忆，他们很在乎“故事”。他们似乎有许多不太确定的想法，说了比别人多得多的“也许”。

表达离别的词汇“离开”和“再见”出现了很多次，后来他们心中有了“远方”、“城市”、“地方”、“南方”和“北方”，他们在“生活”，感受着情绪，有“快乐”也有“悲伤”，有时“微笑”，但也时常“沉默”，甚至一个人流“眼泪”。更多时候，只是“孤独”、“孤单”、“寂寞”，多么渴望“温柔”，渴望一个“拥抱”。

他们提到了很多很多次“爱情”、还有“喜欢”的人和东西。也许民谣就像这样，明明唱着歌，却好像和你聊了一次家常，讲了讲一些有趣或伤感的故事。



### 说唱

爬取了“说唱”歌单搜索结果的前50个歌单（按收藏人气排名），一共有11438首歌，其中无歌词的歌曲有7374首，有效数据为4064首。

未过滤语气词的词云：

![image-20200225003346657](%E7%BD%91%E6%98%93%E4%BA%91%E7%83%AD%E6%AD%8C%E9%83%BD%E5%9C%A8%E5%94%B1%E4%BB%80%E4%B9%88%EF%BC%9F%E8%AE%B0%E4%B8%80%E6%AC%A1Python%E7%BB%83%E4%B9%A0.assets/image-20200225003346657.png)

看起来还好，就是不容易找到重点，那么过滤语气词之后呢？

![image-20200225002013991](%E7%BD%91%E6%98%93%E4%BA%91%E7%83%AD%E6%AD%8C%E9%83%BD%E5%9C%A8%E5%94%B1%E4%BB%80%E4%B9%88%EF%BC%9F%E8%AE%B0%E4%B8%80%E6%AC%A1Python%E7%BB%83%E4%B9%A0.assets/image-20200225002013991.png)

“love”是最多的，其次是“baby”、“back”，无论中英文，也无论歌曲的种类，大家似乎都对“时间”很关心。“感觉”出场了，“man”和“girl”几乎一样多。



去除英文内容后的词云（虽然不是很干净）：

![image-20200225024053290](%E7%BD%91%E6%98%93%E4%BA%91%E7%83%AD%E6%AD%8C%E9%83%BD%E5%9C%A8%E5%94%B1%E4%BB%80%E4%B9%88%EF%BC%9F%E8%AE%B0%E4%B8%80%E6%AC%A1Python%E7%BB%83%E4%B9%A0.assets/image-20200225024053290.png)

| 前1-10 | 次数 | 前11-20 | 次数 |
| :----- | ---: | :------ | ---: |
| نى     | 2136 | 喜欢    | 1112 |
| ئا     | 1825 | لى      | 1112 |
| مە     | 1567 | 兄弟    | 1099 |
| 时间   | 1392 | uh      | 1076 |
| 世界   | 1363 | قا      | 1028 |
| ىم     | 1326 | سە      | 1010 |
| 真的   | 1270 | يا      | 1010 |
| 想要   | 1263 | ىن      |  957 |
| 生活   | 1176 | دى      |  935 |
| بى     | 1119 | دە      |  935 |

（尽量）去掉英文常用词后，出人意料的是有一些中东歌曲乱入了，大家忽略就好。中文里“时间”和“世界”仍然出现最多次，其次是“真的”，如何理解？先不慌想这个，我们看看其他词。

看看“想要”、“生活”、“喜欢”、“希望”、“不用”，还有“老子”、“兄弟”，以及那些密密麻麻卡在缝里的关键词，如“都会”、“只能”、“放弃”、“别人”、“习惯”、“不停”、“从不”、“从来”，让人觉得这是一群有些憋屈却仍然逞能的年轻人在唱着自己的主张，然而在说唱里，直接提到“爱情”却比民谣要少得多。

说回来“真的”这个有些奇妙的词，我去翻了翻歌词，结果是这样的：

![image-20200225000534639](%E7%BD%91%E6%98%93%E4%BA%91%E7%83%AD%E6%AD%8C%E9%83%BD%E5%9C%A8%E5%94%B1%E4%BB%80%E4%B9%88%EF%BC%9F%E8%AE%B0%E4%B8%80%E6%AC%A1Python%E7%BB%83%E4%B9%A0.assets/image-20200225000534639.png)

个人理解，词“真的”在情绪较强烈、或表强调的时候使用，所以在说唱中为了起到强调的效果，就不知不觉地用多了，我真的是这样想的。

除了“真的”，就是“想要”了：

![image-20200225001143614](%E7%BD%91%E6%98%93%E4%BA%91%E7%83%AD%E6%AD%8C%E9%83%BD%E5%9C%A8%E5%94%B1%E4%BB%80%E4%B9%88%EF%BC%9F%E8%AE%B0%E4%B8%80%E6%AC%A1Python%E7%BB%83%E4%B9%A0.assets/image-20200225001143614.png)

“想要”是一种明确表达欲望的说法，看来歌手“想要”让大家都知道他的想法，是一种主动的表现。





大家有其他想法吗？还请留言评论。



## 三

分词和词云的效果与文本清理、分词工具有关。文本清理要注意去掉多余的符号，还要去掉多余的空格，尤其是在生成词云之前。分词结果直接影响词云效果，要选择合适的分词工具，之前测试了jieba，始终效果不太好，后来改用北大开发的pkuseg，效果要好一些，分词性能也更高。

在写代码的过程中，有一些经验教训也总结总结吧。

**关于爬虫：**在获取歌词的函数中，见`get_lyric()`，要考虑到歌词不存在的情况，如果歌词不存在直接跳过，会造成csv表项目不匹配，导致尾部数据缺失。虽然考虑到了，若直接设为空值，后果则是默认属性将为{float}NaN，浮点数空值。歌词项应该统一为str，若中间穿插其他类型，将为后续造成许多麻烦。

**关于Python：**`.remove()`与`.append`

分词时，有一个过滤常用词的操作，开始时不懂，去掉常用词直接用的`.remove()`，后来才发现这严重影响性能，文本越大，性能影响越大。可以另定义一个列表，用`.append()`操作。我估计用`.remove()`操作时需要移动列表之后的每一个元素，复杂度直接指数增加了。

**关于正则表达式：**在使用正则表达式替换字符时，或运算不宜太多，不必要的或运算可以转为其他方式处理，比如无效词/字符可以放入停用词表处理。

**关于使用停用词表：**要特别注意英文单词有大小写形式，最好保证停词表统一为大写或小写，在词语匹配的时候分词要相应的用`.upper()`或`.lower()`形式。

**关于工具使用：**使用wordcloud库的时候没认真看文档，入了一个坑，使用`.generate()`生成的词云不一定按照词频处理，有些词频高的词语反而无法显示或显示较小，这是库算法自行决定的，若要按照词频生成词云，需要用`.generate_from_frequencies()`方法。



代码已经放在Github上了，感兴趣的同学可以点击阅读全文查看。