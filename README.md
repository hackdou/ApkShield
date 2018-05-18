# ApkShield
一代壳加固方案的demo

大致思路就是先写一共壳apk,将它的dex文件取出,将源apk放到壳dex中,再将合并的dex文件放回到源apk中,这个方案本身也不安全因为apk最后还是会落地的，这里的意义更多的只是个人学习．

最近正好看了一点相关的东西,一代壳还是比较简单的,而且涉及到一些dex文件处理和apk动态加载等等我想折腾一下的点,所以就想实现一个简单的版本玩一下.

项目下有两个文件夹

### androidpjt
主要是android项目,一个壳应用,一个用来演示的源应用.

因为涉及到apk的动态加载,所以现在还是一个helloworld的app,没有写的太复杂.

另外一个是壳app,这个应用只用来生成dex,再和源apk合并用的,主要涉及的就是动态加载

### tool
主要是两个脚本

apkshield.py用来合并壳dex和源apk,然后生成加固后的apk.

get_payload_dex.py 用来从合并后的dex(注意这里是dex不是合并后的apk,这里的dex就是放回到源apk里面的),取出源apk,一开始是调试用的,现在也一并留着了

### 进度
目前没有做完,并没有让加固后的apk跑起来

#### 已经完成的部分
将壳dex和源apk 合并,生成加固后的apk

在apk中解析出源apk,工具和应用内都完成了该部分功能

调用源app的application 的oncreate方法

目前只是一个简单的demo,回头再看看jni吧.

#### 踩到的坑
emmmm....又弄到很晚,还是没有太大的进展,倒是发现一些其他的东西,今天试了一下修改manifest,shieldapplication去代替源apk的application时,需要写上完整包名,现在解出来的apk长度太长了,java不支持32位以上的下标,刚刚查知乎说c++可以,明天再试一下看看.

今天又试了一下这个东西,大小超长并不是因为本身apk超长了,而是为了修改manifest,使用了apktool反编译apk,然后修改manifest后,再重新对apk打包,这个重新打包的过程肯定是有些动作的,比如说优化了之类的,实际情况下看了一下重打包之前的apk,
大小和没有加壳之前差不多,晚上吃饭的时候和土豆师傅聊了下这个.现在打算仍然重打包apk,但是仅仅取出重打包apk后的manifest文件,然后再加入到重打包之前的apk中,这样就还是成功改动了manifest文件并且不让dex有什么变化,实际操作后当然也是成功了,
源程序重新启动了.本身代码没什么改动的地方,毕竟现在情况很简单.