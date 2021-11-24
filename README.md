# Flexijob - 基于 Shardingsphere ElasticJob 实现的动态作业启动器

## 使用方法

启动 Flexijob

```java
ZookeeperConfiguration zookeeperConfiguration = new ZookeeperConfiguration("10.98.174.202:2181", "ns0");
CoordinatorRegistryCenter registryCenter = new ZookeeperRegistryCenter(zookeeperConfiguration);
registryCenter.init();
Flexijob flexijob = new Flexijob(registryCenter);
flexijob.start();
```

Flexijob 启动后，会获取 "ns0" 下已有的作业配置并启动，同时开始监听 "ns0" 下新创建的作业配置。
如果需要添加动态作业，可以使用 FlexijobClient 或直接向 Zookeeper 写入作业配置，即可自动启动新的作业调度。

Flexijob Client 添加作业

```java
CoordinatorRegistryCenter registryCenter = RegistryCenters.createRegistryCenter("127.0.0.1:2181,127.0.0.1:2182,127.0.0.1:2183", "flexijob-examples");
JobConfiguration jobConfiguration = JobConfiguration.newBuilder("flexijob-0", 3).cron("0/10 * * * * ?").build();
FlexijobClient client = new FlexijobClient(registryCenter);
client.addJob(jobConfiguration, DemoJob.class);
```

## 功能实现

- [x] 启动时调度已有作业配置
- [x] 动态添加调度作业
- [ ] 自动清理调度已完成的作业（指定明确时间的 Cron，未来不会再执行）
- [ ] 动态作业管理 UI

