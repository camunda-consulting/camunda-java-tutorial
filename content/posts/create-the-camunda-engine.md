---
title: "Startup the Camunda Engine"
date: 2017-08-17T10:53:47+02:00
draft: true
---

We'll you're here because you want to start chatting with the engine, well you'll probably fine the conversation a trifle one sidded if you there isn't an engine to talk to. So the first thing we're going to demonstrate is how to create an instace of the engine. 

You'll need to import the engine to your java class
`
import org.camunda.bpm.engine.ProcessEngine;
`
but once that's done it's all plane sailing because you can startup the engine 


```Java
	  ProcessEngine processEngine = ProcessEngineConfiguration
			    .createStandaloneProcessEngineConfiguration()
			    // We need a database for persistence, it's really easy to just use H2
			    .setJdbcUrl("jdbc:h2:./camunda-h2-dbs/process-engine;DB_CLOSE_DELAY=1000")
			    // We're also going to need to tell the engine to create it's tables 
			    .setDatabaseSchemaUpdate("true")
			    // Finally we should activate the job executor for async processing 
                //(more on that later)
			    .setJobExecutorActivate(true)
			    .buildProcessEngine();
```

Thats it! 
The egine is ready to rock!