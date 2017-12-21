# Activiti 多租户
## 一、多租户共享数据库
主要用于数据共享在一个数据库的使用场景，一个或者多个引擎共用一个数据库，因此，操作的时候需要区分部署的流程资源的来源。
![](/img/activiti-multi-tenant/1.png)
上图中的例子存在两个相同的流程定义Key，但是来源不同，引擎会使用TENANT_ID_来确保数据不会混乱。

1. 优点：部署比较方便，只需要在启动流程实例的同时设置TENANT_ID_即可。
2. 缺点：
- 操作数据的时候需要时刻使用TENANT_ID_，如果不传递该值或者传递错误，就会出问题。
- 引擎共享一个数据库会造成库中的数据激增，当数据达到一定规模的时候，会比较吃力。

## 二、多租户独立数据库（多引擎）
每一个引擎使用自身的数据库，引擎之间的数据完全隔离。
![](/img/activiti-multi-tenant/2.png)

1. 优点：每一个引擎可以有不同的资源配置，引擎之间相互隔离，每个引擎的性能会大大提升。
2. 缺点：配置更复杂，每一个引擎实例都会占用一定的资源。

## 三、多租户独立数据库（单引擎）
一个引擎支撑多个数据库的操作，需要有路由规则，例如 A 租户的数据存在 A 库，B 租户的数据存在 B 库。
![](/img/activiti-multi-tenant/3.png)
通过一个引擎来管理和配置，但数据之间是相互隔离的。只需要配置和管理不同的数据库即可，引擎会自动根据TENANT_ID_找到对应的资源。

```
//建立流程引擎
config = new MultiSchemaMultiTenantProcessEngineConfiguration(tenantInfoHolder);
config.setDatabaseType(MultiSchemaMultiTenantProcessEngineConfiguration.DATABASE_TYPE_H2);
config.setDatabaseSchemaUpdate(MultiSchemaMultiTenantProcessEngineConfiguration.DB_SCHEMA_UPDATE_DROP_CREATE);
config.registerTenant("alfresco", createDataSource("jdbc:h2:mem:activiti-mt-alfresco;DB_CLOSE_DELAY=1000", "sa", ""));
config.registerTenant("acme", createDataSource("jdbc:h2:mem:activiti-mt-acme;DB_CLOSE_DELAY=1000", "sa", ""));
config.registerTenant("starkindustries", createDataSource("jdbc:h2:mem:activiti-mt-stark;DB_CLOSE_DELAY=1000", "sa", ""));
processEngine = config.buildProcessEngine();
```

以上代码使用引擎注册租户，每一个租户需要一个租户的唯一标识以及数据源的配置。
路由规则需要配置哪些标识连接到哪个数据库，从TenantInfoHolder实例获取即可，TenantInfoHolder是一个接口，需要自己实现。

```
//TenantInfoHolder使用示例
private void setupTenantInfoHolder() {
   DummyTenantInfoHolder tenantInfoHolder = new DummyTenantInfoHolder();
   tenantInfoHolder.addTenant("alfresco");
   tenantInfoHolder.addUser("alfresco", "joram");
   tenantInfoHolder.addUser("alfresco", "tijs");
   tenantInfoHolder.addUser("alfresco", "paul");
   tenantInfoHolder.addUser("alfresco", "yvo");
    
   tenantInfoHolder.addTenant("acme");
   tenantInfoHolder.addUser("acme", "raphael");
   tenantInfoHolder.addUser("acme", "john");
   tenantInfoHolder.addTenant("starkindustries");
   tenantInfoHolder.addUser("starkindustries", "tony");
   this.tenantInfoHolder = tenantInfoHolder;
}
```

在向引擎发布流程定义时，需要传递租户的唯一标识。

```
//发布流程定义
repositoryService.createDeployment()
            .addClassPathResource(...)
            .tenantId("myTenantId")
            .deploy();
```

通过部署传递租户 id 的作用：

1. 所有包含在部署中的流程定义都会继承部署的tenantId。
2. 所有从这些流程定义发起的流程实例，都会继承流程定义的tenantId。
3. 所有流程实例运行阶段创建的任务都会继承流程实例的tenantId，单独运行的task也可以包含tenantId。
4. 所有流程实例运行阶段创建的分支都会继承流程实例的tenantId。
5. 触发一个信号抛出事件（在流程本身或通过API）可以通过tenantId实现。信号只会在租户环境下执行：比如，如果有多个信号捕获事件，并且名字相同，实际只有正确的tenantId下的事件会被调用。
6. 所有作业（定时器、异步调用）会集成tenantId，或者来自流程定义（比如定时开始事件），或流程实例（运行期创建的作业）。
7. 所有历史实体（历史流程实例、任务、节点）会从它们对应的运行状态集成tenantId。
8. 作为单独的一部分，model也可以设置tenantId。
所有的查询API都可以通过tenantId进行查询：

```
//查询示例
runtimeService.createProcessInstanceQuery()
    .processInstanceTenantId("myTenantId")
    .processDefinitionKey("myProcessDefinitionKey")
    .variableValueEquals("myVar", "someValue")
    .list();
```
