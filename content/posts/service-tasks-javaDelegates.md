---
title: "Run your own java task"
date: 2017-08-17T10:53:47+02:00
draft: true
---

If you've been following some of the other tutorials then you already know what lovely java camunda has to ofer. But of course we all know that you've got your own cool code that you'd like run so lets have a little chat about that.

The first thing we need is a service task that points to your java class

![service task](/service-task-with-java.PNG)


You can't just attach any old java class to a service task, you're going to need some context and for that we need a `JavaDelegate`


```Java
import org.camunda.bpm.engine.delegate.DelegateExecution;
import org.camunda.bpm.engine.delegate.JavaDelegate;
import org.camunda.bpm.engine.variable.value.LongValue;

//The class needs to implement the JavaDelegate
public class MyCoolJavaClass implements JavaDelegate
{

	@Override
    // When this is called by the engine it will run the execute method
	public void execute(DelegateExecution execution) throws Exception {

		// This method also gives us a DelegateExecution variable
        // this is how we know the current contenxt and we can do stuff like...
        // get variables!
		String userName = (String)execution.getVariable("userName");
		Long priority = new Long(40);
		
		// We alo have access to the process engine and all the lovely
        // API contained within. 
		long taskAmount = execution.getProcessEngineServices()
		.getTaskService()
		.createTaskQuery()
		.taskAssignee(userName)
		.count();
		
		if(taskAmount > 10) {
			priority = new Long(20);
		}
		// Importantly we can also give variables back to the process
		execution.setVariable("taskPriority", priority);

	}

}

```


