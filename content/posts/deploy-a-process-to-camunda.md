---
title: "Deploy a process to camunda"
date: 2017-08-17T10:53:47+02:00
draft: true
---

If you've got yourself a process engine, the logicial thing to do is to build yourself a process and deploy it!
You can build a model using [Camunda's modeler](https://camunda.org/download/modeler/) if you like... go ahead - i'll wait...

So - assuming you've built yourself a process that looks something like this:

![BPMN image](/enterSomeDataModel.png)

which from a BPMN XML perspective looks something like this


```XML
<?xml version="1.0" encoding="UTF-8"?>
<bpmn:definitions xmlns:bpmn="http://www.omg.org/spec/BPMN/20100524/MODEL" xmlns:bpmndi="http://www.omg.org/spec/BPMN/20100524/DI" xmlns:di="http://www.omg.org/spec/DD/20100524/DI" xmlns:dc="http://www.omg.org/spec/DD/20100524/DC" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" id="Definitions_1" targetNamespace="http://bpmn.io/schema/bpmn" exporter="Camunda Modeler" exporterVersion="1.9.0">
  <bpmn:process id="enterSomeDataModel" name="enterSomeDataModel" isExecutable="true">
    <bpmn:startEvent id="StartEvent_1">
      <bpmn:outgoing>SequenceFlow_0iyr4nx</bpmn:outgoing>
    </bpmn:startEvent>
    <bpmn:sequenceFlow id="SequenceFlow_0iyr4nx" sourceRef="StartEvent_1" targetRef="Task_01jvgf6" />
    <bpmn:userTask id="Task_01jvgf6" name="Enter some Data">
      <bpmn:incoming>SequenceFlow_0iyr4nx</bpmn:incoming>
      <bpmn:outgoing>SequenceFlow_1y51gf9</bpmn:outgoing>
    </bpmn:userTask>
    <bpmn:endEvent id="EndEvent_05h86ud">
      <bpmn:incoming>SequenceFlow_1y51gf9</bpmn:incoming>
    </bpmn:endEvent>
    <bpmn:sequenceFlow id="SequenceFlow_1y51gf9" sourceRef="Task_01jvgf6" targetRef="EndEvent_05h86ud" />
  </bpmn:process>
</bpmn:definitions>
```

The only thing left to give is gift this beautiful creation to the process engine

```Java

// The engine has a bunch of services and when you want to deploy - you're gonna call the Respository Service
processEngine.getRepositoryService()
    .createDeployment()
    //Now it's simply a matter of pointing the engine in the direction of your process file.
    .addClasspathResource("vacation-request.bpmn")
    .deploy();

```


Congrats! you've just give a process engine the greatest gift of all... 