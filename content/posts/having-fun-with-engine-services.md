---

title: "Having fun with camunda's API"

date: 2017-08-17T10:53:47+02:00

draft: true

---


While discussing how user tasks worked I demonstraited some functions of the `TaskService` but of course we have so many other fun services so since you're here who not take a look at some of them, so you have an idea what fancy little APIs we have:

1. `RuntimeService`
1. `RepositoryService`
1. `IdentityService`
1. `ManagementService`


The RuntimeService is used for messing around with or creating process instances. We can send messages, set variables and even suspened processes

```Java
	  // We can suspend a procsess so that no new instances can start
	  processEngine.getRuntimeService()
	  .suspendProcessInstanceByProcessDefinitionKey("FunDemoProcess");
      // We could also try to send a message to a waiting process
      processEngine.getRuntimeService()
	  .createMessageCorrelation("HelloMessage");

```
The RepositoryService is used when you want to find out things about the deployed process definitions. As well as creating new ones

```Java
	  // It's pretty easy to add a new Resource 
	  processEngine.getRepositoryService()
	  .createDeployment()
	  .addClasspathResource("SomeClassPath")
	  .deployWithResult();
      
      
	  // You can also query for definintions 
	  processEngine.getRepositoryService()
	  .createProcessDefinitionQuery()
	  .processDefinitionKey("FunDemoProcess")
	  .list();
```	  

Stalking people on social media is hard but not on Camunda! With the Identity service you can find out anything you want to know about who is using Camunda.

```Java
	  // As you might expect you can do simple stuff like checking a password
      processEngine.getIdentityService()
	  .checkPassword("Niall", "dryingTeeth");
	  // But you can also do some fun user queries
      
      
	  processEngine.getIdentityService()
	  .createGroupQuery()
      // Including over multipul tenants. 
	  .memberOfTenant("tenantOfDoom")
	  .groupIdIn("DoomBringers")
	  .list();

```
Finally the managment service - this little guy is great if you want to check out jobs and messages. For instance if there is a timer in your process and you'd quite like to move it along this is where to start

```Java
	  //Our old friend the create query but in this case we're using it to find
      // some jobs
	  List<Job> highPriorityJobs = processEngine.getManagementService()
	  .createJobQuery()
      //but we only want jobs that have a high priority
	  .priorityHigherThanOrEquals(60)
	  .list();
	  
	  // now we can "execute" the job meaning that we move the process on
	  for (Job job : highPriorityJobs) {
		processEngine.getManagementService()
		.executeJob(job.getId());
	  }

```

And thats a fun little intro into some of the API calls available in camunda. 


