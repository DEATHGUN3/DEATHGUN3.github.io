# Activiti邮件配置方式

>在某个用户任务节点上想实现发送邮件的功能，只需在该用户任务上配置个任务监听器;
>流程结束发送给申请人的邮件,可以在结束节点配置一个执行监听器;
>根据邮件发送的场景会使用到不同的事件：

- 一种是create，这种方式是用来发送邮件提醒当前节点审批人有任务需要处理。 实现原理是当前任务创建好了，但是当前任务的审批人还没对这个任务执行任何其他操作(如签收，办理)，它就会触发配置好的Java类(监听类)，

- 一种是complete，这种方式是用来发送邮件告知申请人申请成功。实现原理是申请人或审批人对当前任务执行了办理(完成)这个动作后，它就会触发配置好的Java类(监听类)，
- 一种是end,这种方式是用来发送邮件告知申请人流程结束了。实现原理是流程结束后，它会触发配置好的Java类(监听类)。

### 场景一：开始节点后的第一个节点，申请表单

 - 配置方式：配置监听类
    com.gaiaworks.workflow.activiti.activiti.listener.taskListener.SendMail，实现complete事件
 - 参数选择：mailKey的值选择的是申请成功的模版key；receiveUser的值是apply，表示该邮件是发送给申请人的。

![这里写图片描述](http://img.blog.csdn.net/20171123170452379?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvamluemhlbmdndW8=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

![这里写图片描述](http://img.blog.csdn.net/20171123171354021?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvamluemhlbmdndW8=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

### 场景二：结束节点前的所有任务节点(除去第一个申请表单的任务节点)

 - 配置方式：配置监听类
   com.gaiaworks.workflow.activiti.activiti.listener.taskListener.SendMail，实现create事件
 - 参数选择：mailKey的值选择的是审批提醒的模版key；receiveUser的值是assignee，表示该邮件是发送给审批人的。

![这里写图片描述](http://img.blog.csdn.net/20171123171608573?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvamluemhlbmdndW8=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

![这里写图片描述](http://img.blog.csdn.net/20171123171625343?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvamluemhlbmdndW8=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

### 场景三：结束节点

 - 配置方式：配置监听类 listener.EndSendMail，实现end事件
 - 参数选择：mailKey的值选择的是结束的模版key；receiveUser的值是apply，表示该邮件是发送给申请人的
![这里写图片描述](http://img.blog.csdn.net/20171123172048092?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvamluemhlbmdndW8=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

![这里写图片描述](http://img.blog.csdn.net/20171123172105796?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvamluemhlbmdndW8=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
