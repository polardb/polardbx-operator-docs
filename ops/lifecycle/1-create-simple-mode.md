PolarDBXCluster 支持将 GMS 的功能合并到第一个 DN（DN-0）中来减少整体使用的资源，这种情况适合进行测试部署。

想要指定这种部署模式，将 `spec.shareGMS`设置为 true 即可。需要注意的是，极简模式和普通模式不能来回切换。
