ColorOS Patchrom
==================

环境搭建：
------------------
1.参照AOSP或者CM编译环境搭建环境
2.主要需要安装Android SDK和Java SDK
代码同步：
----------
1.安装[Git and Repo](http://source.android.com/download/using-repo).
2.使用如下代码，同步ColorOS patchrom代码

     mkdir patchrom
     cd patchrom
     repo init -u git://github.com/ColorOS/patchrom.git -b jellybean42-mtk
     repo sync

步骤：
-------
1.将升级包命名为update.zip放在device目录下
2.在当前目录执行". build/envsetup.sh"
3."cd device; make firstpatch",根据device目录下的temp/reject目录修改插桩失败文件
4.如果smali/framework.jar.out文件夹太大会导致打包失败，需要进行手动分包
5.在device目录下执行"make fullota"，在device目录下生成color-update.zip就是生成的color升级包

注意：
----------------
1.可能需要修改boot.img（init.rc中的BOOTCLASSPATH加入oppo-frmework.jar）
2.device/makefile里有个变量可能需要修改
	ORGIN_SECOND_FRAMEWORK_NAME:	填入欲编译机器第二个framework文件名
	COLOR_FRAMEWORK_JARS:		填入欲编译机器的framework文件，主要是是否有第二个framework
3.如果需要分包，请在smali文件夹下新建XX.jar.out格式的文件夹，并手动进行分包
4."device/custom-update":定制的文件夹，make fullota时会直接覆盖过去，所以文件夹结构需要和升级包的保持一致，里面放一些原版不可删除的system/app/下面的apk或者自己新增或修改的一些文件
5.如果获取到的升级包是odex的，需要先使用deodex工具合并,默认使用apilevel15
	合并命令如下：
	deodex.sh update.zip
	如果执行出错，请尝试指定apilevel值，如下：
	deodex.sh -a 17 update.zip



