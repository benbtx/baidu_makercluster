# baidu_makercluster
baidu_makercluster
聚合  多级聚合 分类聚合



这个有点麻烦，每个人不同图标涉及分组分类聚合；多个不同的人一起还要图标展示涉及分级聚合，要改聚合的源码；最好轨迹播放 加减点也要改。。。



百度地图clusterMarker效率低下的原因，以及解决方案，以及建议
本人在公司开发过程中使用了百度api的聚类，但是直接调用百度的类后，发现效率特别低下，于是决定看看它代码到底怎么写的，并做了如下修改。

1.地图平移过程中速度特别慢
原因：clusterMarker给地图添加了两个侦听，分别是zoomend和moveend，都调用了redraw()方法。我不知道是开发者图省事还是考虑不周，这个redraw方法是先清空所有的覆盖物然后重新聚合。zoomend清空无可厚非，但是moveend需要吗，明显不需要好不，要是地图点多，根本就没法用。

解决方案：添加一个方法，或者改进redraw（），让moveend之后不用清空地图覆盖物即可。其实只要注释掉this._clearLastClusters();这行就行。

2.点击聚合点，展开的时候，基本卡死
原因：我找到了对应得代码，发现程序居然给这个聚合点注册了n-1次点击事件侦听，也就是说要是这个点有100，那就有99次点击事件执行，执行的是thatMap.setViewport(thatBounds);这行代码，调整地图用的。汗，这能不慢么。

解决方案：我将事件绑定变成了一次，这样就解决了点击展开慢的情况。具体方式有点走偏门，先不说了。

哎，用百度api的时候就听说谷歌的聚合点效率比百度的高多了，的确，如果不改的情况下，基本几百个点地图基本就快不可用了，希望，百度能在下一个版本进行改进。还有啊，就是this._clusterMarker.removeEventListener("click")为什么不好用啊，求解答。
http://blog.sina.com.cn/s/blog_c1e3e3ab0102v7hk.html


百度地图点聚合仿链家定位点多级聚合，且滑动、刷新加载定位点
https://blog.csdn.net/baidu_36600645/article/details/86489816?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.edu_weight&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.edu_weight


百度地图API开发：大量坐标点进行分组聚合
https://blog.csdn.net/guoweish/article/details/47418953?utm_source=blogxgwz0


百度地图点聚合提高效率
https://blog.csdn.net/ztop_f/article/details/55256003


百度地图：解决点聚合marker卡顿方案
https://www.itsvse.com/thread-4929-1-1.html
