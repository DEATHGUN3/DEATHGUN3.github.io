
# codepush命令转化为接口调用

## 将生产环境的app的度量信息整合成接口
对应的code-push-cli命令:code-push deployment h MyApp Production

## 改造发布版本更新
对应的code-push-cli命令:code-push release-react <AppName> <PlatName>
去除由react-native bundle:命令打包文件对应的代码，转化成直接人为压缩bundle文件，将剩余的代码整合成一个接口.

# code-push-server使用常见问题
1. code-push-server 关闭ssh终端窗口后，服务就停止了。怎么能够让它在后台运行？

用pm2 start code-push-server启动服务。
2. code-push-server在windows上无法下载更新包

在windows下默认有环境变量Public=C:\User\Public,把这个环境变量删除或者将/config/config.js中的public:process.env.PUBLIC || '/download'改成public: '/download'


