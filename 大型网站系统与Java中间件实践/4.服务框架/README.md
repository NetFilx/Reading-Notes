1. 服务框架接入有两个很重要的问题要解决

   - 服务框架自身的部署方式问题

     1. 把服务框架作为应用的一个依赖包与应用一起打包，升级框架的时候会出现较多问题

     2. 把服务框架作为容器的一部分

        ![service_deploy](service_deploy.png)

        ![service_as_webapp](service_as_webapp.png)

        ![service_extend](service_extend.png)

   - 实现自己的服务框架所依赖的jar包与应用自身依赖的jar包之间的冲突问题

     ![jar_conflict](jar_conflict.png)

     ![jar_conflict_photo](jar_conflict_photo.png)

2. 负载均衡实现策略的选择：随机，轮询，权重，其中权重方式一般是指动态权重的方式，可以根据响应时间等参数进行计算。在服务提供者的机器能力对等的情况下，随机和轮询比较容易实现，不对等的情况下使用权重方式

