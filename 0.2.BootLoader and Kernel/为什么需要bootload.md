
> 网上搜索过为什么需要bootload，但是感觉没太多人讲明白，或者讲得太复杂，可能是自己境界不够把。以下是根据自己所学知识的对这一问题的简单归纳，**文不赘述**。

为什么需要bootload？首先，CPU要能够读取程序，程序必须先加载到内存当中。内存又分为静态内存SRAM和动态内存DRAM。静态内存的特点是容量小但不需要软件初始化，动态内存是容量大但需要软件初始化。嵌入式产品内存需求量大需要使用动态内存，那么就需要面临一个问题——谁来初始化动态内存？

所以在加载系统之前需要有一段代码来初始化动态内存DRAM并引导内核，这段代码后来就发展为bootload。并且这段代码也需要加载到内存当中，所以一般的嵌入式产品就是内置SRAM+外接DRAM。SRAM用来加载bootload,DRAM用来加载OS。

**另外**，bootload和OS都是放在外存Flash中，需要能够读取Flash到SRAM的代码，同样这段代码也要有地方能够存放，并且掉电后代码能保存，同时CPU上电后可以直接读取。于是还需要NorFlash。（注意：因为不知道将来板子会接什么样的动态内存，所以不负责初始化DRAM）

另外你可能会问，直接NorFlash+DRAM可以吗？可以啊，PC机就是这么处理，如果你启动代码较小或者**你比较有钱** 装的NorFlash比较大就可以。

CPU上电之后先从内部NorFlash（64KBiROM）读取固化代码，设置时钟，看门狗等，然后初始化Flash并读取bootload到静态内存SRAM初始化动态内存DRAM，并从Flash加载OS到动态内存DRAM。如果bootload体积过大，还涉及到重定位的问题