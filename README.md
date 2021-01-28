## Sample app repo for Sinch Getting started 

To see a more detailed explanation of how the code will work, visit our [SMS getting started](https://developers.sinch.com/docs/sms-java) page.

# Prerequisites

- JDK 11 or later
- Gradle https://gradle.org/install/
- [Sinch account](https://dashboard.sinch.com) 

# How to run
This template repo allows you to quickly set up a Java Spring boot application to enable you to send and receive SMS via rest commands. It also spins up an [ngrok](https:ngrok.com) tunnel than you can use to configure your application to receive SMS in the [dashboard](https://dashboard.sinch.com). 

## Setup
Open [SmsJavaSampleApplication.java](https://github.com/sinch/sms-java-sample/blob/90df5881704950ee1e7de420f35adbaa3e4feea0/src/main/java/com/sinchdemos/smsjavasample/SmsJavaSampleApplication.java#L34) and fill in your values. You will find all the below values at https://dashboard.sinch.com/sms/api.

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

The method that handles sending SMS is located at src/main/main/java/com/sinchdemos/smsjavasample/SmsJavaSampleApplication.java and is named sendSMS. 

Make a post request with your favorite tool and don't forget to change *ToPhonenumber* to your own number including + and country code. 

Curl

```shell
curl --location --request POST 'https://localhost:8080/sms/send' \
--header 'Content-Type: application/json' \
--data-raw '{
    "ToPhonenumber": "+15612600684",
    "Body": "Hello"
}'
```

PowerShell

```powershell
$headers = New-Object "System.Collections.Generic.Dictionary[[String],[String]]"
$headers.Add("Content-Type", "application/json")
$body = "{
`n    `"ToPhonenumber`": `"+1Your phonenumber`",
`n    `"Body`": `"Hello`"
`n}"
$response = Invoke-RestMethod 'http://localhost:8080/sms/send' -Method 'POST' -Headers $headers -Body $body
$response | ConvertTo-Json
```

## Receiving SMS

To be able to handle replies to your sent SMS, you need to configure a callback url in the [Sinch dashboard](https://dashboard.sinch.com). When you start your Java application, an ngrok tunnel is set up that temporarily exposes a website to your application. Open a browser, go to http://localhost:4040/status and copy the URL that ends in ngrok.io. Head over to https://dashboard.sinch.com/sms/api and click the serviceplan id you use for this application. Then click edit where you see **Callback URL** and paste the url from the [ngrok status page](http://localhost:4040/status). Be sure to click **SAVE** when you are done. 

> **Note:**
> Every time you restart your application, you will get a new ngrok domain. If you want to set up ngrok manually and keep it running, you can turn off the automatic ngrok by removing compile('io.github.kilmajster:ngrok-spring-boot-starter:0.1') from your gradle file. 

