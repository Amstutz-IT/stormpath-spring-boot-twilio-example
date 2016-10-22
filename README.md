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

# Setup HTTPS with Boxfuse 

    keytool -genkey -alias tomcat -keyalg RSA -keysize 2048 -keystore keystore.jks -validity 3650
    
    mv keystore.jks src/main/resources/

Add boxfuse specific properties in `application-boxfuse.properties`

    server.port=443
    server.ssl.enabled=true
    server.ssl.key-store=classpath:keystore.jks
    server.ssl.key-store-password=myS3cr3tPwd

# Deploy to AWS

    mvn clean package
    
    boxfuse create stormpath-twilio -logs.type=cloudwatch-logs
    
    boxfuse run -envvars.STORMPATH_API_KEY_ID=1234abc... \
        -envvars.STORMPATH_API_KEY_SECRET=efg.... \
        -envvars.STORMPATH_CUSTOMDATA_LOGINIPS_IDENTIFIER=loggedLoginIPs \
        -envvars.TWILIO_ACCOUNT_SID=ABCD... \
        -envvars.TWILIO_AUTH_TOKEN=2a2b... \
        -envvars.TWILIO_FROM_NUMBER=+1...
    