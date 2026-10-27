# 体育直播 · Sports Live

独立 Android TV 应用，包名 `com.hughdai.sportslive`，版本 `1.0.0`。支持足球、篮球及其他公开赛事，适合 Android TV、Google TV 和支持 Android 应用安装的智能电视。

关键词：体育直播、足球直播、篮球直播、Android TV、IPTV、HLS、Sports Live、Football Live、Basketball Live。

已在小米 L55MA-SPL、Xiaomi HyperOS 3.0、Android 14 设备上测试；小米电视只是测试设备，不是唯一支持设备。

## 下载与更新

APK 文件名格式为 `sports_live-版本号.apk`。App 从 `HughDai/sports_live` 的 GitHub Releases 检查更新，只在远程版本高于本机版本时提示升级。

从用户指定的 https://www.88kanqiu.tw/ 读取公开赛事列表，足球/篮球优先。赛事详情的正常页面脚本在受限WebView运行，读取可见的线路链接。已识别的直接HLS与88player HLS包装链接交由Media3播放。复杂网页、需解锁的入口不转换为原生播放源，不宣称全站覆盖。

## 操作

首页：上/下选择，确定播放。上移可选全部/足球/篮球/其他/刷新。
播放：上/下切换当前分类的直播赛事，左/右切线路，确定打开赛事列表，返回关闭列表或回首页，菜单刷新当前线路/列表/诊断。
列表优先显示源站标注直播的项目。网站状态可能过期；未开赛项目提示等待，不显示假画面。

健康播放20秒后记忆赛事ID和线路身份。重启重新获取动态线路，赛事已结束时留在首页。切源可能存在不同直播延迟。画质上限偏好1080p，轨道具备自适应才可自动降码率；固定码率不能本地转码。

## 有限故障恢复

单条线路最多两次尝试，401/403/410触发一次重新取源；持续累计缓冲15秒尝试其他线路。每次赛事选择的自动重取源最多一次，全失败时显示重试/换赛事提示。手动换源或刷新开始新的尝试预算。

WebView仅允许HTTPS的指定赛事详情页与同域资源，不加载广告接口、不提供JavaScriptInterface、禁用file/content/mixed content，不放行SSL错误。网页源公开可访问不代表长期可用或与其他源延迟一致。

## 打赏

设置菜单提供支付宝和微信支付二维码，采用电视横屏并列显示。收款码内置在 APK 中，不上传到公开仓库。

## 构建与测试

JDK17–21、Android SDK34/Build Tools34、Gradle8.2、AGP8.2.0。Media3 1.3.1沿用已缓存版本；验证版先固定依赖，正式版升级另行验证。

```sh
./test-core.sh
./gradlew assembleDebug lintDebug assembleRelease lintRelease
```

首次构建需要联网下载依赖。创建本机 `local.properties` 指定 `sdk.dir`；此文件不进入源码ZIP。release关闭调试，个人侧载沿用本机调试签名，源码包不含签名私钥，换机器构建签名可能不同。

桌面/移动HTML回归样本位于 tests/fixtures，仅作解析测试，不打包进App。没有在代码中硬编码本次赛事ID或签名媒体地址。

当前已验证的赛事和已知限制见交付目录“体育直播-0.1.0-验证记录.md”。
