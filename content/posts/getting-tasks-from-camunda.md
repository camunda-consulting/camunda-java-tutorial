---

title: "Getting tasks from camunda"

date: 2017-08-17T10:53:47+02:00

draft: true

---


People often ask me "Niall, how can I get tasks from Camunda?" I usualy answer saying "with skill and persistance" but i've since been informed that this is a less than satisfactory answer. This tutorial aims to finally answer this question.

The first thing to know is where to start and that is with the `TaskService` this is the entry point for any kind of communication needed regarding user tasks. 

```Java
	processEngline.getTaskService()
    // Generally if you're looking to get some info 
    // from the engine it's going involve creating a query
	  .createTaskQuery()
      // In this instance we're only interested in tasks from our Fun Demo Process
	  .processDefinitionKey("FunDemoProcess")
	  .list();
```



Let's do something more interesting - lets say Niall is feeling a little overworked and wants to go get himself a beer. He's wants to offload his work to poor unsuspecting Darya...

```Java
	  List<Task> list = processEngine.getTaskService()
	  .createTaskQuery()
	  .processDefinitionKey("FunDemoProcess")
      //We've added some additional criteria to get only tasks Niall is doing
	  .taskAssignee("Niall")
	  .list();
	  
	  for (Task task : list) 
	  {
      	//Now it's just a matter of looping through the list of tasks
		processEngine.getTaskService()
		.setAssignee(task.getId(), "Darya");
	  }

```

Now Darya finally has some work to do - while Niall can relax and get himself a beer





