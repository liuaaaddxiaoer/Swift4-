dsym调试

http://blog.csdn.net/xiaofei125145/article/details/50408051

dsymtools

https://github.com/answer-huang/dSYMTools


1. 下载工具后我们可以在xcode的BuildSettings下的BuildOptions 打开 DWARF with dSYM File
2. 编译后生成dsym文件在
/Users/xxx/资源/Libr/Deve/Xcode/DriData/对应项目/build/ 可以找到dsym文件将文件拖入到dsymTools中可以生成对应的uuid等信息
3. xcode devices查看真机项目的崩溃信息 在binary image下一行有一个地址 填到 dsymtools上
4. 可以用命令 otool -l 
xx.app/xx 查看 加载命令
5. 可以使用友盟进行统计线上的dsymbug日志进行分析