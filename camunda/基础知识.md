## Rest api

> [ 官方文档 ]: https://docs.camunda.org/rest/camunda-bpm-platform/7.19/

## Java api

> [官方文档]:https://docs.camunda.org/manual/latest/user-guide/process-engine/process-engine-api

![img](https://docs.camunda.org/manual/latest/user-guide/process-engine/img/api.services.png)

在 Camunda 中，使用 Java API 可以进行流程管理和自动化操作。以下是一些常用的 Java API 对象和方法，用于与 Camunda 引擎进行交互

### ProcessEngine

`ProcessEngine` 是与 Camunda 引擎交互的入口点。它代表整个 Camunda 引擎实例，并提供访问其他服务的方法，如 `RepositoryService`、`RuntimeService`、`TaskService` 等。

```java
    private final ProcessEngine processEngine;

    public void test(){
      
        RepositoryService repositoryService = processEngine.getRepositoryService();
        TaskService taskService = processEngine.getTaskService();
        IdentityService identityService = processEngine.getIdentityService();
        FormService formService = processEngine.getFormService();
        HistoryService historyService = processEngine.getHistoryService();

      
        ManagementService managementService = processEngine.getManagementService();
        FilterService filterService = processEngine.getFilterService();
        ExternalTaskService externalTaskService = processEngine.getExternalTaskService();
        CaseService caseService = processEngine.getCaseService();
        DecisionService decisionService = processEngine.getDecisionService();
    }
```

### RepositoryService

`RepositoryService` 提供了管理流程定义的方法。您可以使用它来部署和检索流程定义、查询流程定义的信息、获取 BPMN XML 数据等。该服务还允许您对流程定义进行版本控制、挂起或激活流程定义等操作。

```java
// 查询流程定义列表
List<ProcessDefinition> processDefinitions = processEngine.getRepositoryService()
        .createProcessDefinitionQuery()
        .processDefinitionKey("myProcessKey")
        .list();

// 获取单个流程定义
ProcessDefinition processDefinition = processEngine.getRepositoryService()
        .createProcessDefinitionQuery()
        .processDefinitionKey("myProcessKey")
        .singleResult();

// 删除流程定义及关联的部署信
processEngine.getRepositoryService().deleteDeployment(processDefinition.getDeploymentId());
processEngine.getRepositoryService().deleteProcessDefinition(processDefinition.getId());

// 获取这流程定义模型元素
Collection<Task> tasks = processEngine.getRepositoryService()
        .getBpmnModelInstance(processDefinition.getId())
        .getModelElementsByType(Task.class);

Collection<SequenceFlow> SequenceFlows = processEngine.getRepositoryService()
        .getBpmnModelInstance(processDefinition.getId())
        .getModelElementsByType(SequenceFlow.class);
```

### RuntimeService

`RuntimeService` 用于管理运行时流程实例和执行的服务。通过 `RuntimeService`，您可以启动、暂停、恢复、终止、删除流程实例，发送信号，查询和设置流程变量，以及处理并发执行等。

- 启动流程

  ```java
  @PostMapping("/process/key/{key}/start")
  public void startProcess(@PathVariable String key, @RequestBody HashMap<String, Object> variables){
  
  
      // 通过definitionId启动，可以启动指定的流程版本
      List<ProcessDefinition> list = processEngine.getRepositoryService().createProcessDefinitionQuery().list();
    
      for (ProcessDefinition processDefinition : list) {
          log.info( "process info : id {}, name : {}, version: {}",
                  processDefinition.getId(),
                  processDefinition.getName(),
                  processDefinition.getVersionTag());
  
          // 根据流程defineId来启动
          processEngine.getRuntimeService().startProcessInstanceById(processDefinition.getId());
      }
  
      // 通过key来来启动，启动该流程的最新版本
      processEngine.getRuntimeService().startProcessInstanceByKey(key);
  
      // 可以传入一些流程实例参数 比如发起人starter
      processEngine.getRuntimeService().startProcessInstanceByKey(key, variables);
  }
  ```

- 删除流程

  ```java
      @DeleteMapping("/{id}")
      public void deleteProcess(@PathVariable String id) {
          
          processEngine.getRuntimeService().deleteProcessInstance(id, "ByeBye");
          
      }
  
    /**
      删除现有运行时流程实例。删除按照需要向上传播。
      参数:
      processInstanceId -要删除的流程实例id，不能为空。
      deleterreason -删除的原因，将被存储在历史记录中。可以为空。
      skipCustomListeners——如果为true，则只有内置的ExecutionListeners会被ExecutionListener通知。EVENTNAME_END事件。
      抛出:
      BadUserRequestException -当processInstanceId为空时。
      NotFoundException—当没有找到具有给定processInstanceId的流程实例时。
      AuthorizationException—如果用户没有权限。删除资源的权限。PROCESS_INSTANCE或no权限。对资源的DELETE_INSTANCE权限。
     */
    void deleteProcessInstance(String processInstanceId, String deleteReason, boolean skipCustomListeners);
  
  	void deleteProcessInstances(List<String> processInstanceIds, String deleteReason, boolean skipCustomListeners, boolean externallyTerminated);
  
    void deleteProcessInstancesIfExists(List<String> processInstanceIds, String deleteReason, boolean skipCustomListeners, boolean externallyTerminated,
        boolean skipSubprocesses);
  	
  	// 异步删除
    Batch deleteProcessInstancesAsync(List<String> processInstanceIds, ProcessInstanceQuery processInstanceQuery, String deleteReason, boolean skipCustomListeners, boolean skipSubprocesses);
  
  
  ```

### TaskService

`TaskService` 用于管理流程任务的服务。它允许您查询、创建、完成、分配和更新任务。您可以使用 `TaskService` 操作任务属性、设置任务的候选人和候选组、获取任务的变量等。

```java

    private final ProcessEngine processEngine;

    public void test(String taskId, HashMap<String, Object> variables) {

        TaskService taskService = processEngine.getTaskService();

        // 查询任务列表
        taskService.createTaskQuery()
                        .taskId(taskId)
                        .taskAssignee("admin")
                        .list();


        // 查询单个任务
        Task task = taskService.createTaskQuery().singleResult();

        // 将给定任务的分配人更改为给定userId。标识组件不检查用户是否已知。
        taskService.setAssignee(taskId, "admin");

        // 创建处理意见
        taskService.createComment(taskId, task.getProcessInstanceId(), "同意");
      
        // 对任务声明责任:指定用户被指定为该任务的受让人。
        // 与setAssignee(String, String)的不同之处在于，如果任务已经分配了用户，则执行检查。标识组件不检查用户是否已知。
        taskService.claim(taskId, "admin");

        // 将任务标记为已完成并继续执行流程。
        // 此方法通常由任务列表用户界面在受让人提交任务表单并提供所需的任务参数之后调用。
        taskService.complete(taskId, variables);

        // 将任务委托给另一个用户。这意味着设置了受让人，并将委托状态设置为DelegationState.PENDING。
        // 如果未在任务上设置所有者，则将所有者设置为任务的当前受让人。
        // 新的受让人必须使用resolveTask(String)向所有者报告。只有所有者才能完成任务
        taskService.delegateTask(taskId,"admin");

        //删除给定的任务，不删除与该任务相关的历史信息。
        taskService.deleteTask(taskId, "deleted");

        // 设置优先级
        taskService.setPriority(taskId,50);

        // 添加候选人 候选组
        taskService.addCandidateUser(taskId, "");
        taskService.addCandidateGroup(taskId, "");

    }

```

### IdentityService

`IdentityService` 用于管理用户、用户组和身份验证的服务。您可以使用 `IdentityService` 创建、查询、更新和删除用户和用户组，管理用户和用户组之间的关联关系，执行身份验证和授权等操作。

```
    private final ProcessEngine processEngine;

    public void test() {
        IdentityService identityService = processEngine.getIdentityService();

        // 验证
        identityService.checkPassword("admin", "jszq5768");

        identityService.createUserQuery().list();
        identityService.createUserQuery().singleResult();

        identityService.createGroupQuery().list();
        Group group = identityService.createGroupQuery().singleResult();

        User user = new UserEntity();
        user.setId("admin");
        user.setPassword("******");
        user.setFirstName("admin");
        user.setLastName("admin");

        // 保存
        identityService.saveUser(user);
        identityService.saveGroup(group);

        // 删除
        identityService.deleteUser(user.getId());
        identityService.deleteGroup(group.getId());

    }
```



### FormService

`FormService` 用于与任务表单和表单数据进行交互的服务。您可以使用 `FormService` 查询和提交任务表单，访问表单字段和值，并执行与表单相关的其他操作。

### HistoryService

`HistoryService` 用于访问和查询历史数据的服务。通过 `HistoryService`，您可以获取与已完成的流程实例、任务、变量等相关的历史数据。您可以查询已完成的流程实例、任务执行时间、变量历史等信息。

```java
// 历史流程实例
historyService.createHistoricProcessInstanceQuery()
        .processDefinitionId("")
        .orderByProcessInstanceStartTime().asc()
        .listPage(1,10);

// 历史任务
historyService.createHistoricTaskInstanceQuery()
        .processInstanceId("")
        .orderByHistoricActivityInstanceStartTime().asc()
        .listPage(1,10);
```

