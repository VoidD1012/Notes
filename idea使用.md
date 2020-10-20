1. alt+数字：展示侧边栏的快捷键
2. module：Eclipse中的workspace相当于idea里面的project；Eclipse中的project相当于idea里面的module
3. idea设置module的原因：目前主流的大型项目都是分布式部署的，结构都是类似这种多Module的，这些module之间都是处于同一个项目业务下的模块，彼此之间是有不可分割的业务关系的
4. out目录：存放编译后的字节码文件
5. 删除模块：先remove module，防止误删，然后在进行delete删除module
6. 手动导包快捷键：alt+ enter
7. 自动导包和优化设置：general->auto import
8. 导入同一个包下的类超过指定个数的时候，导包合并为*：Editor->Code Style->Java->Imports->Class count to use import with ' * '
9. 打开多个类不折叠，多行显示：Editor->Editor Tabs ->Tab limit、Show Tabs in group
10. 修改类前面的文档注释模板（只对新建的类有效）：Editor->File and Code Templates->Includes->File Header
11. 设置项目文件编码：Editor->File Encodings：把所有的都改成utf-8，勾选Transparent native-to-ascii conversion
12. 自动编译：Build->Compiler->Build project automatically、Compile independent modules in parallel
13. 导入jar包：Project Settings->Libraries->+进行添加
14. 生成序列化版本号：Editor->inspections->输入serialization->勾选Serializable class without serialVersionUID，最后在类上用alt点击，就可以选择生成序列化版本号了
15. 【常用快捷键】alt + insert：创建内容
16. psvm ： 创建main方法
17. .sout ：输出语句
18. Ctrl + d ：复制
19. ctrl + y ：删除一行
20. ctrl + shift + z ：redo
21. ctrl + shift + 上/下 ：行上移或者下移
22. ctrl + n ：搜索类
23. alt + insert ：生成代码
24. alt + enter ：导包、生成变量等
25. ctrl + / 或者 ctrl + shift + / ：单行注释或者多行注释
26. shift + f6 ：重命名
27. fori： for循环
28. ctrl + alt + t ：代码块包围，try-catch，if，while等
29. ctrl+空格 ：代码自动补全提示
30. ctrl + alt + ←/→：代码层级回退和调用
31. 【代码模板】配置一些常用代码缩写，比如sout、psvm
32. 【断点调试】debugger->transport： 把socket模式换为shared memory，这个模式在Windows下更省内存
33. 【断点调试】alt + f8：debug中可以动态的查看变量的值