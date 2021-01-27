## Sample app repo for Sinch Getting started. 

For a more detailed explanation on whats going on the code see [SMS getting started](https://developers.sinch.com/docs/sms)

# Prerequisites

- JDK 11 or later
- Gradle https://gradle.org/install/
- [Sinch account](https://dashboard.sinch.com) 

# How to run
This template repo allows you to quickly set up an Java Spring boot appllicaiton to enable you to send adn recive SMS via rest commands. It also spins up a [ngrok](https:ngrok.com) tunnel than you can use to configure your application to recive sms in the dashboard. 

## Setup
Open [SmsJavaSampleApplication.java](src/main/main/java/com/sinchdemos/smsjavasample/SmsJavaSampleApplication.java) and fill in your values. You can find all the blow values at https://dashboard.sinch.com/sms/api

```java
    static String SERVICE_PLAN_ID = "";
	static String TOKEN = "";
	private static String SENDER = ""; 
```
and run 
'''
.\gradlew bootRun
'''

## Sending SMS 

The method that handles Sending SMS is located 
src/main/main/java/com/sinchdemos/smsjavasample/SmsJavaSampleApplication.java
and calld sendSMS. 

Make a post request with your favorite tool, dont forget to change *ToPhonenumber* to your own number including + and country code. 
'''shell
curl --location --request POST 'https://localhost:8080/sms/send' \
--header 'Content-Type: application/json' \
--data-raw '{
    "ToPhonenumber": "+15612600684",
    "Body": "Hello"
}'
'''
PowerShell
```
$headers = New-Object "System.Collections.Generic.Dictionary[[String],[String]]"
$headers.Add("Content-Type", "application/json")
$body = "{
`n    `"ToPhonenumber`": `"+1Your phonenumber`",
`n    `"Body`": `"Hello`"
`n}"
$response = Invoke-RestMethod 'http://localhost:8080/sms/send' -Method 'POST' -Headers $headers -Body $body
$response | ConvertTo-Json
```



## Recieving SMS

To be able to handle replies to your send sms you need to configure a callback url in the sinch dashboard. When you start your java application an ngrok tunnel is set up that temporarly exposes a website to your application. Open a browswer and go to http://localhost:4040/status and copy the URL that ends in ngrok.io. Head over to 
https://dashboard.sinch.com/sms/api and click on the serviceplan id you use for this application. Paste the url from the [ngrok status page](http://localhost:4040/status). 

> Be aware  that every time you restart your application you will get a new ngrok domain. If you want to set up ngrok manually and keep it running, you can turn of the automatic ngrok by removing compile('io.github.kilmajster:ngrok-spring-boot-starter:0.1') from your gradle file. 

REply to your previous message and 

