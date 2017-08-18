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


There we have it!  our process has started