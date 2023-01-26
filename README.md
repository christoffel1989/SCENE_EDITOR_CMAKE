# SCENE_EDITOR_CMAKE
本程序来源自于Repos https://github.com/Linloir/SceneEditor
直接下载该Repos程序用VS打开无法编译,需安装qt vs插件等 设置十分麻烦
为此专门制作了无需安装qt插件的cmake工程版本 只需cmake build工程即可编译运行
目前只做了qt5以及vs2022的配置实验，使用的是c++17标准  c++20标准应该是不能用的 因为使用了u8字符串 会出问题

目前我发现这个程序存在许多许多的问题，希望原作者能改进
1. warning过多，特别是类型转换方面 我全给你改掉了。
2. 使用了一个enterEvent的widget虚函数，这个虚函数在qt5下的输入参量类型有问题，我改成QEvent后可以编译了
3. 使用希腊字母和特殊unicode的地方 我把所有字符串前面都加了u8""，不然界面上全是问号
4. shader代码存在问题，原代码一运行控制台就疯狂提示uniform变量不存在，我debug了半天发现把最后一个函数fragmentshader.glsl最后一个函数全注释掉就没有问题了，但是这个结果导致程序运行时的光照会出现不稳定。
5. 程序运行后模型加载存在问题，FOV等等设置有问题，模型无法显示完全。
6. 3D旋转写的太烂了，手感过差，原本想给你加上我以前写的trackball 但是想想太麻烦了 就放弃了
7. 还有很多很多问题，最初下你这个代码是因为你的UI看起来挺不错的 想看看你UI部分怎么美化的 以及 3D模型的拾取是怎么做的 结果大失所望，代码水平有待提升。