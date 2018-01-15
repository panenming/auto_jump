# 微信跳一跳外挂

思路和原理参考 https://github.com/wangshub/wechat_jump_game [自动跳跃算法细节参考](https://github.com/faceair/wechat_jump_game/blob/master/wechat_jump.py#L50)

用 Golang 重新实现是期望跨平台，方便打包给普通用户使用。
## ⚠️ 警告

目前微信开始大规模进行反作弊，作弊严重的可能记录清零拉黑名单，手动游戏都无法记录成绩。目前还没有找到合理的反作弊方案，请慎重使用。也欢迎参与讨论 [#74](https://github.com/faceair/youjumpijump/issues/74) 。


## 跳跃系数

目前一般设备初始值可以设为 2.04，但如果设备的分辨率比较特殊，用默认的系数不能每次跳到中心点，可以微调一下系数争取每次都跳到中心点。但本程序的系数跟其他版本比如 python 的系数不一样，算法不一样，不要混淆。程序处理逻辑是会先将图片 resize 成宽度 720 像素的图片然后再找点和跳跃。

## FAQ

1. 怎么安装 ADB？怎么开启手机的 USB 调试？为什么电脑连不上手机？为什么我 Push 时说找不到文件？这些命令怎么执行？

请自行搜索解决，也可以尝试使用 [下载地址](https://github.com/faceair/youjumpijump/releases/latest) 中的 windows.zip 一键运行包，实在连不上可以[用模拟器](https://github.com/wangshub/wechat_jump_game/tree/master/%E6%96%B0%E6%89%8B%E5%B0%8F%E7%99%BD%E8%AF%B7%E4%BD%BF%E7%94%A8%E8%BF%99%E4%B8%AA%E4%BB%A3%E7%A0%81%20%20%E4%B8%8D%E9%9C%80%E8%A6%81%E4%BD%BF%E7%94%A8%E7%9C%9F%E6%9C%BA%E7%9A%84%E4%B8%93%E7%94%A8%E4%BB%A3%E7%A0%81)（不保证安全性，自行承担风险）。

2. 程序执行输出了乱码？

Windows 在 CMD 字符编码不能显示中文，可以参考 [百度](https://www.baidu.com/s?wd=Windows%20ADB%20%E4%B9%B1%E7%A0%81) 设置解决。

3. 运行异常/第一下就飞了？

关大爆炸功能、关传送门、关悬浮球、运行时不能出现截图悬浮窗、关掉影响触摸和画面显示的相关功能。

4. 为什么要将文件 Push 到 Android 手机内执行？

我们发现会有偶尔的情况下定位的关键点都是准的，ADB 命令也执行了，但是就是没跳过去；有 MIUI 用户的截图被自动重命名了；也有朋友跑着跑着突然 ADB 报错了，无法继续执行了；同时也有朋友使用的电脑上的模拟器，非常的稳定。所以我们推测 ADB 命令在某些情况下会有问题，可能也跟连接的线材有关。所以我们决定将程序移植到 Android 上，直接在 Android 上运行。实验证明，确实稳定多了。

5. 怎么编译 Android 版本？

`CGO_ENABLED=0 GOARCH=arm GOOS=linux go build -o auto_jump android/main.go`

