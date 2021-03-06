Lutung - Java Mandrill API
======

Lutung - a Java interface to the [Mandrill](http://www.mandrill.com/) API. 
All API calls are implemented thus far. Check out Mandrill's API [Documentation]
(https://mandrillapp.com/api/docs/) to see all the possible magic.

Features:

*  all public API calls are implemented.
*  easy library set up; just provide your api 
   key that you got from Mandrill.
*  all API calls are exposed through one simple interface: 
   the [MandrillApi](src/main/java/com/microtripit/mandrillapp/lutung/MandrillApi.java) 
   class.
*  easy, intuitive naming scheme. All function-names are derived from the 
   Mandrill API calls: if there is a call with the address 
   '/messages/send.json', then we have a function for that 
   called 'MandrillApi.messages().send(...)'.
*  API request errors are exposed to the user (you!) as a 
   [MandrillApiError](src/main/java/com/microtripit/mandrillapp/lutung/model/MandrillApiError.java). 

Examples
--------
**The 'whoami' of Mandrill:**
```java
MandrillApi mandrillApi = new MandrillApi("<put ur Mandrill API key here>");

MandrillUserInfo user = mandrillApi.users().info();

// pretty-print w/ gson
Gson gson = new GsonBuilder().setPrettyPrinting().create();
System.out.println( gson.toJson(user) );
```


**Send a 'Hello World!' email**
```java
MandrillApi mandrillApi = new MandrillApi("<put ur Mandrill API key here>");

// create your message
MandrillMessage message = new MandrillMessage();
message.setSubject("Hello World!");
message.setHtml("<h1>Hi pal!</h2><br />Really, I'm just saying hi!");
message.setAutoText(true);
message.setFromEmail("kitty@yourdomain.com");
message.setFromName("Kitty Katz");
// add recipients
ArrayList<Recipient> recipients = new ArrayList<Recipient>();
Recipient recipient = new Recipient();
recipient.setEmail("claireannette@someotherdomain.com");
recipient.setName("Claire Annette");
recipients.add(recipient);
recipient = new Recipient();
recipient.setEmail("terrybull@yetanotherdomain.com");
recipients.add(recipient);
message.setTo(recipients);
message.setPreserveRecipients(true);
ArrayList<String> tags = new ArrayList<String>();
tags.add("test");
tags.add("helloworld");
message.setTags(tags);
// ... add more message details if you want to!
// then ... send
MandrillMessageStatus[] messageStatusReports = mandrillApi
		.messages().send(message, false);
```


**Error handling for Mandrill API errors**
```java
MandrillApi mandrillApi = new MandrillApi("<put ur Mandrill API key here>");

try {
	MandrillUserInfo user = mandrillApi.users().info();
} catch(final MandrillApiError e) {
	log.error(e.getMandrillErrorAsJson(), e);
}
```


**Create a new template**
```java
MandrillApi mandrillApi = new MandrillApi("<put ur Mandrill API key here>");

MandrillTemplate newTemplate = mandrillApi.templates().add(
		"test_template_001", 
		"<html><body><h1>Hello World!</h1></body></html>",
		false);
```

Dependencies
------------
If you use Maven, the [pom file](pom.xml) should help you with this. If you are not 
using Maven, make sure to put the following libraries (and their dependencies) 
on your classpath:

* [google-gson](https://code.google.com/p/google-gson/)
* [Apache Http Components](http://hc.apache.org/index.html)
* [Apache Commons IO](http://commons.apache.org/proper/commons-io/)
* [Apache Commons Logging](http://commons.apache.org/proper/commons-logging/)

Known Issues
------------
*  The metadata returned by the mandrill api on 
   [/messages/search.json](https://mandrillapp.com/api/docs/messages.html#method=search)
   does not get mapped to a member of [MandrillMessageInfo](src/main/java/com/microtripit/mandrillapp/lutung/view/MandrillMessageInfo.java)
   
*  So far, I failed to successfully use Mandrills [/messages/send-raw.json](https://mandrillapp.com/api/docs/messages.html#method=send-raw)
   call. I'm not sure if I fail to create valid MIME contents, but lemme know if 
   you make any experience with this call.

*  Also, I have no inbound-emailing set up with Mandrill. Would be great if anyone 
   out there could test the implemented 'inbound' functionalities.

Contribute
-----------
Sure! Just shoot me an email and let me know about your ideas.

Lutung? Huh?
------------
**A monkey!!!** The [Javan Lutung](http://en.wikipedia.org/wiki/Javan_lutung) is the name giver 
for this project; hat tip to [MailChimp's](http://mailchimp.com/) naming scheme.

License
-------
This library is released under the GNU Lesser General Public 
License [http://www.gnu.org/licenses/lgpl.html](http://www.gnu.org/licenses/lgpl.html). 