---
title: "Playing with process history"
date: 2017-08-17T10:53:47+02:00
draft: true
---

Camunda has a really good memory. Assuming you haven't changed the default history settings everthing that has happend to each process instance and even each process varialbe is being kept in the Camunda's history tables. To get your hands on this information you just need to askt the``HistoryService``. 


A common enough requirment for the ``HistoryService`` is simply trying to find out about a bunch of processes they might have recently started.

```Java
	//We start by geting the history service to give us a list of processes
  	List<HistoricProcessInstance> list = processEngine.getHistoryService()
    	.createHistoricProcessInstanceQuery()
        // We're looking for a specific process so we add the Key 
    	.processDefinitionKey("FunDemoProcess")
        // Finally, we're only interested in processes that have started since 	yesterday
    	.startedAfter(yesterday)
    	.list();
    	
```

It's really important to know that in the case above we'll of course be getting any processes that both started and ended since yesterday but we'll also be getting any processes that are still currently running since yesterday. Since the history tables actually contains all processes that have finished and those that are currently running. 

There's of course more to history than this we can also some damage. 

```Java

 	processEngine.getHistoryService()
    	.deleteHistoricDecisionInstanceByDefinitionId("FunDemoProcess");

```

The above code will purge your history tables of all ``FunDemoProcess`` instances. Luckily while this will kill all the history data of your currently running processes this is just a copy. Your running processes are safe and sound on the Runtime table.

