---
title: "Create a BPMN model with Java"
date: 2017-08-17T10:53:47+02:00
draft: true
---

While camunda done have a perfectly good [modeler avaialbe](https://camunda.org/download/modeler/) in order to build processes - it's possible that you're the kind of person who enjoys the more authentic method of creation - via the Java API

Camunda has very kindly supplied a lib which can help you live out the dream of building a model without a modeler.
`
import org.camunda.bpm.model.bpmn.Bpmn;
`
And once you've added it - things get pretty easy, in order to create this model:

![Simple BPMN model](/enterSomeDataModel.png)

You just need to using the following:


```Java
	 Bpmn.createProcess()
	  .startEvent()
      //We've already got a start event and next up is our user task
	  .userTask()
	    .id("importantWork")
	    .name("Enter some Data")
        //as an added bonus i'm going to add myself as the assigne
	    .camundaAssignee("Niall")
	  .endEvent()
	  .done();
```

We could then make things more interesting by throwing in a gateway

```Java

	  Bpmn.createProcess()
	  .startEvent()
	  .userTask()
	    .id("UserTask_importantWork")
	    .name("Enter some Data")
	    .camundaAssignee("Niall")
        // So we'll throw in a Paraelle Gateway
	   .parallelGateway()
       // first we'll stick a service task in there
	   .serviceTask()
	   	.id("ServiceTask_sendEmail")
	   	.name("Send an Email")
        // going to add a Topic - so that it can be done by some external worker (maybe)
	   	.camundaTopic("SendEmail")
        // then we'll end this flow by added an end event
       .endEvent()
       // Continuing the other flow requires us to go back to our gateway
	   .moveToLastGateway()
       // once we're back we can continue the other flow by added a User Task
	   .userTask()
	   	.id("UserTask_checkData")
	   	.name("Check Data")
        // Then just drop in one more end event and we're good to go!
	  .endEvent()
	  .done();

```

With these new additions - our model looks like this



![Final Model](/enterSomeDataModelParaelle.png)