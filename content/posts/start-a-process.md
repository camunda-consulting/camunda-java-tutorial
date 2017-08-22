---
title: "Start a process instance"
date: 2017-08-17T10:53:47+02:00
draft: true
---

With a process deployed to Camunda you gain the amazing power of being able to create and instance do that process. It's also very easy to start a process. So lets assume we've deployed the process below


![Simple BPMN model](/FunDemoProcess.PNG)



You just need to using the following:


```Java
	  ProcessInstance processInstance = processEngine
      .getRuntimeService()
      //once we get the runtime service we kick things off with the key of the process. 
	  .startProcessInstanceByKey("FunDemoProcess");

```
FUN FACT:  you can also use the `.startProcessInstanceById("SomeFancyISD")` method to start a process, but it doesn't actually take the ID of the BPMN process! It actually starts a specific VERION of a deployed process. 

Obviously when starting a process you might want to pass in some process data - so lets give it a shot.

```Java
   //We need to setup a nice little map for our variables
   Map<String, Object> variables = new HashMap<String, Object>();
   variables.put("userName", "Niall");
   variables.put("greatHackDaysProject", true);

	ProcessInstance processInstance = rule.getRuntimeService()
	.startProcessInstanceByKey("FunDemoProcess", variables);
```


There we have it!  our process has started.. 
It was almost too easy in fact and i still have lots of space on this page so lets try something more complicated. Lets imagine I want to start the process from the ``Check Data`` task instead of the start event. In fact it would be even better if started from both the ``Check Data`` and the ``Send Email`` tasks. 
Sounds quite complicated but of course it's not

```Java
	
    // If we're going to start this process in the middel somewhere it's
    //usually the case that we'll need some data to make up for the missing tasks
	Map<String, Object> variables = new HashMap<String, Object>();
    variables.put("userName", "Niall");
    variables.put("greatHackDaysProject", true);
    
    // Once again we need the runtime server
    processEngine.getRuntimeService()
    	// But instad of startProcess... we're using the createProcess.. method
	    .createProcessInstanceByKey("FunDemoProcess")
        // We're creating a token before the check data task by giving it the id
	    .startBeforeActivity("checkDataTaskID")
        // and then doing the same with the email task
	    .startBeforeActivity("sendEmailTaskID")
        // finally we give the process the list of variables and maybe a business key for luck
	    .setVariables(variables)
	    .businessKey("MyKey")
        // The just execute
	    .execute();

```



