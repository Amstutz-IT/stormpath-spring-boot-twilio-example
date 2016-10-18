# Identity Management in Spring Boot with Stormpath and Twilio

Use case: Demonstrate user authentication with Stormpath in combination with post login hook and SMS notification through Twilio 

# Requirements

1. Stormpath API Key to use for API access
2. Provision a (local) Twilio phone number that supports SMS
3. Verify in Twilio the phone numbers which are going to be notified

# Runtime Environment Variables

    twilio.account.sid = your account sid
    twilio.auth.token = your account token
    twilio.from.number = your twilio phone number

Also add the property `twilio.enabled = true` otherwise the environment 
variables will not get read and no post login messages get sent as only 
the defaultLoginPostHandler will be registered and executed. 

# Build and run from Terminal
 
     mvn clean package
     
     java -jar target/stormpath-sdk-examples-spring-boot-web-0.1.0-SNAPSHOT.jar -Dtwilio.account.sid=theSID -Dtwilio.auth.token=theTOKEN -Dtwilio.from.number=theNumber
     
     firefox http://localhost:8080/
     