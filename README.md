# 我的2023年数学建模华中赛A题方案

大家好，我是湖北第二师范学院数学与统计学院的一个大三学生，本次参加华中赛并完成了其中的A题，在团队中充当编程手，谨以这个开源项目对本次比赛做一个小结，供大家参考，也给自己做一个记录。

## 2023/5/13更新

刚刚收到通知，拿到了华中赛省级一等奖，大家放心参考，这套方案还是有一定价值的。

我已经将本次作品及其LaTeX源文件开源，欢迎大家访问：
https://github.com/Jack-TanXin/2023-HuaZhong-Paper

## 简要评价

这场比赛4月30日晚上8点公布赛题，我们团队仨八点以前就在机房等着赛题公布了，拿到赛题的时候还是相当震惊的，我的第一感觉是A题是传统的统计题，B题有可能是自然语言处理（后来想了一下这点数据量应该做不了深度学习，没再深入了），C题典型的时间序列分析（也能算统计题）。我们团队一般都是以我为核心，打了五六场数学建模全都是做统计题，我思考了一会觉得A应该比C难，所以在半个小时内选择了A题。

接着我就让我的俩队友一起查资料，我直接拿起Python就开始乱杀，最后花了整整一天时间（5月1日7点）完整地做完了整个A题，后面的两天就在排版、查阅资料、还偶尔找点办法优化结果。

## 项目介绍

赛题我放在项目的根目录里了，附件统一放在“赛题附件”里，没有做过任何修改，一些稍微重要的表我放在“探索信息”文件夹里。解决方案全都在根目录里。

## 具体解题思路

拿到赛题后直接开干，对第一题发起猛烈进攻。

拿到赛题以后我还是太单纯了，浪费了很多时间用来探索数据，打Kaggle的经验告诉我先了解数据集中特征的分布是非常重要的，但是。。。数学建模的数据集实在太没意思了，而且特征数量非常多，没用的特征也特别多，我还一本正经地没看题目直接开始EDA，EDA到没有耐心了，就直接开始解题了。

### 第一题

第一题是很直接的，简而言之两件事：看看关于不同药品的八个不良反应是否有显著性差异，那我们也别纠结EDA的事了，直接选出自己需要的特征然后对需要的特征进行EDA就行了。药品是二分类的特征，八个不良反应也同样是，所以就用卡方检验来研究他们之间是否具有显著差异。然而在这之前我非要装逼，让评委看看我的数据可视化能力，当然这只能是定性的结论，关键还得看卡方检验哈！然后是预判，我的天啊怎么会有这么直接的问法，我一看是个二分类的任务，本来打算直接上Voting的，结果不用不知道，一用吓一跳，KNN、决策树的AUC都超过0.9，我思考了一下，决定不亮出杀招，选用了KNN方便我的建模手进行数学解释（这也提醒各位统计题的编程手，做题时别为了模型精度乱套算法，你选个神经网络，你的建模手怎么解释？？？画个黑盒子？？？），并用混淆矩阵和ROC图解释了一下测试集上的结果，本来还想搞个灵敏度分析的，一想到有四个小题就感觉paper可能正文篇幅要超过20面（最后超过了31面呜呜呜）。

### 第二题

第二题也很直接，同样是选取自己需要的特征就行了，所以数学建模竞赛千万别上来就把数据集完整分析、清洗、预处理一遍，大可不必。又是显著性检验，但赛题并没有问具体哪些生命体征数据有显著差异，而是问是否具有，于是看着一堆生命体征数据，我果断上主成分分析法对特征空间进行数据降维。使用主成分分析法也是一门学问，这个算法数学解释性很强，原理也很简单，但其类别非常多，完全搞清楚也很不容易！这里注意降维程度越大，保留的原数据所具有的信息也会越少，所以要像我一样充分考察降维的程度，这样才能让后面的分析有说服力！随后就是独立样本的参数检验和非参数检验，要注意先进行正态性检验，通过就上T检验，没通过就只能非参数检验了。然后他居然问我什么造成了这种显著差异，我从没见过这种问法，我采取的方式是以有显著差异的生命体征主成分为标签、包括药品类别的众多特征构成特征空间进行回归任务，选用的回归器可不一般——名震江南的机器学习杀手LightGBM，通过数据对模型进行训练后，借助树模型独有的特征重要性评价直接导出各个特征对有显著差异的主成分的影响相对大小，本题要的东西一目了然全在图上。

### 第三题

第三题别太离谱，训练六个回归模型即可。那么这里对精度的要求恐怕就很高了，回归器本来就比较难选，这里我是私下先用了树模型和线性回归，感觉效果都差不多，最后选用的是两个数学解释性强的算法：岭回归和SVM，并使用加权平均进行模型融合，还上了灵敏度分析（毕竟这是最后一个监督学习任务了，再不上灵敏度分析评委就要以为我不会了。。。），最后我的模型当然是比较好的，兼具数学解释性、精度与稳定性。

### 第四题

第四题我刚看到的时候一脸迷茫，我有特别多种方法，逻辑回归、线性判别分析查看线性系数，或者相关性分析等等，但是这题真不太好做，如果用线性模型的话系数特别接近0，相关性分析也是，基本没超过0.2。我推断所谓的应该不是线性关系，所以上面的方法都不到位，而题目特意给出“是怎样的关系”，着实给人当头一棒——这是要给出明确的定量联系啊！还好我对树模型情有独钟，就连我们paper的标题都是“基于树的影响因素预判与信息挖掘”（2022年国赛我们队伍的paper标题也是“基于树的XXX”，具体忘了）。因为前面用了LightGBM，这里也不好再用XGBoost和Catboost了，但我还不至于黔驴技穷，直接上了Bagging树模型——随机森林。通过上采样等操作把模型精度调高，一手树状图直接把分类标准可视化得明明白白，每一步选用那个特征、如何划分全都清晰可见，正道赛题的代码实现完全结束。
