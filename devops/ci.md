# 持续集成

## 创建构建计划

使用团队模板“函数计算项目-**环境”即可。

不同环境后缀的区别主要在于环境变量的`STAGE`参数是开发环境`dev`、预生产环境`testing`、生产环境`master`，以及默认使用的数据库、COS、私有网络等配套云计算资源。

此流程参考[官方文档](https://cloud.tencent.com/document/product/1154/47290#.E5.9F.BA.E4.BA.8E-coding-.E7.9A.84.E8.87.AA.E5.8A.A8.E5.8C.96.E9.83.A8.E7.BD.B2)制作。

### 授权凭据

腾讯云API密钥在凭据中管理，步骤如下：

1. 在“项目设置”的“凭据管理”创建从腾讯云控制台获取的API密钥。
2. 在“流程配置”的第三步“部署”阶段的“withCredentials凭据”部分授权此凭据。

注意要在创建凭据阶段或者从流程配置的授权凭据进入，手动授权给该构建计划，遵循最小授权原则、尽可能小范围授权凭据。

### 配置环境变量

步骤如下：

1. 在“变量与缓存”配置环境变量；
2. 在“流程配置”的第三步“部署”阶段的“withCredentials凭据”部分，把需要在云函数中使用的环境变量写入`.env`文件。
3. 在云函数的`serverless.yml`配置的环境变量部分写入这个云函数需要的。

其中第二步的参考样式如下：

```shell
echo <ENV_NAME_IN_SERVERLSS>=${ENV_NAME_IN_CI}\n > .env
```

注意写换行符`\n`。

通常我们习惯把云函数`serverless.yml`的环境变量和CI的环境变量用同样的名称以防止搞混。云函数到代码里的环境变量配置也尽可能统一。
