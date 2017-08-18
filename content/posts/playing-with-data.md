---
title: "Playing with complex data"
date: 2017-08-17T10:53:47+02:00
draft: true
---

Data comes in many shapes and sizes. primitive types are not too complicated but things can get a little more interesting if you'd like to use more complext types

first we need a model of course - something like this looks good

![XOR Gateway model](/playingWithData.PNG)


The goal here is to create and invoice object that has an `isValid` flag of some kind. The condition on the gateway will read `#{invoice.isValid()}` so our just now is to create this object and put it into the engine contenxt.

Luckily we have "Validate Invoice", a nice little service task in which we can write some Java which gives is this data

```Java

	@Override
	public void execute(DelegateExecution execution) throws Exception {
		// First we're going to get ourselves a varible that will help us
        // Decide about if we should approve the invoice or not
		String badAccountName = (String)execution.getVariable("badAccountName");
		
        // The invoice object is just a simple POJO which i'll get
		Invoice invoice = getDemoInvoice();
		
        // Now we'll check to see if this invoice is from our bad account name
		if(invoice.accountName.equals(badAccountName)) {
        	// if it is - then this invoice is not valid
			invoice.setValid(false);
		}else {
			invoice.setValid(true);
		}
		
        
        // Now we get to the important part.
        // We need to creating a serialized version of our POJO
        // I'm using Camunda Spin to do it
		ObjectValue invoiceSerialized =
				Variables.objectValue(invoice)
                // I've decided to use JSON 
				.serializationDataFormat("application/json")
				.create();
		
        // then we can set the variable as normal
		execution.setVariable("invoice", invoiceSerialized);
	
		
	}

```

An important thing to rememeber is to make sure your object is serializable.
```Java
public class Invoice implements Serializable{
	private static final long serialVersionUID = 1L;
    
	....  
}

```
