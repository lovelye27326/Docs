#Android App 内存泄露之工具(1)

###使用内存监测工具 DDMS –> Heap

操作步骤
 
 ![](https://raw.githubusercontent.com/loyabe/Docs/master/%E5%86%85%E5%AD%98%E6%B3%84%E9%9C%B2/res/DDMS%E5%B7%A5%E5%85%B71.jpg)

1. 启动eclipse后，切换到DDMS透视图，并确认Devices视图、Heap视图都是打开的，没打开的直接Window>ShowView>自己选



2. 将手机通过USB链接至电脑，链接时需要确认手机是处于“USB调试”模式

3. 链接成功后，在DDMS的Devices视图中将会显示手机设备的序列号，以及设备中正在运行的部分进程信息；


4.  点击选中想要监测的进程，如果在进程列表中未出现你的进程的话随便选中一条让Device一排的工具处于可用状态，再击下Update Heap  让其自动找到我们跑的应用的进程，比如小马临时跑的两个应用进程如图；


5. 点击Heap视图中的“Cause GC”按钮；
6. 点击Cause GC之后就可以看到我们应用的内存情况如下图：
 
 ![](https://raw.githubusercontent.com/loyabe/Docs/master/%E5%86%85%E5%AD%98%E6%B3%84%E9%9C%B2/res/DDMS%20Heap%20%E5%B7%A5%E5%85%B71.jpg)

### 说明： #

**1点击“Cause GC”按钮相当于向虚拟机请求了一次gc操作**

**2当内存使用信息第一次显示以后，无须再不断的点击“Cause GC"**

    Heap视图界面会定时刷新，在对应用的不断的操作过程中就可以看到内存使用的变化

**3 内存使用信息的各项参数根据名称即可知道其意思，不知道具体意思的朋友自行用工具（有道、词霸查去）**
 	
	知道工具使用了，那么如何才能知道我们的程序是否有内存泄漏的可能性呢。
 	这里需要注意一个值：Heap视图中部有一个Type叫做data object，即数据对象，也就是我们的程序中大量存在的类类型的对象。
	在data object一行中有一列是“Total Size”，其值就是当前进程中所有Java数据对象的内存总量，
	如果大家想要看“Total Size”是分配的具体信息可以点击“data object这一行来查看详细信息，如下图”（大家看不清楚的点击看大图）

####  一般情况下，在data object行的“Total Size”这个值的大小决定了是否会有内存泄漏。可以这样判断： ##
 	 a) 不断的操作当前应用，同时注意观察data object的Total Size值；
	 b) 正常情况下Total Size值都会稳定在一个有限的范围内，也就是说由于程序中的的代码良好，没有造成对象不被垃圾回收的情况，
	    所以说虽然我们不断的操作会不断的生成很多对 象，而在虚拟机不断的进行GC的过程中，这些对象都被回收了，
		内存占用量会会落到一个稳定的水平；
	 c) 反之如果代码中存在没有释放对象引用的情况，则data object的Total Size值在每次GC后不会有明显的回落，
		随着操作次数的增多Total Size的值会越来越大，直到到达一个上限后导致进程被杀掉。
         


敬请期待下一章(*^__^*) 嘻嘻……
	