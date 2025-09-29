# Alternatives to email-to-text services

Email-to-text services such as vtext, txt.att.net, and tmomail are increasingly blocking e-mails with high traffic volumes. In addition, AT&T recently announced that they would discontinue their email-to-text gateway service entirely, effective June 17, 2025.

As a result, these free services offered by cellular service providers are becoming increasingly unreliable. Because many Campbell Scientific customers rely on this functionality for alarm notifications using the EmailRelay() service, we have compiled this informational document to help you evaluate your options for future notifications.

**NOTE:** Campbell Scientific Inc. does not officially support all these proposed alternatives. Supported alternatives are denoted with an \*.

## EmailRelay() for Email alerts rather than email to text\*

Although cellular providers are limiting or discontinuing their email-to-text gateway services (e.g., vtext.com, txt.att.net, tmomail.net), the Campbell Scientific EmailRelay() service continues to function as it always has delivering email notifications to both computers and phones directly from a data logger using the EmailRelay() instruction.

If you're currently using text-to-email gateways and wish to switch to email notifications, simply update the recipient addresses in your data logger program to the appropriate email addresses. No other changes are needed. Campbell Scientific will continue to support email notifications through EmailRelay() for the foreseeable future.

## CELL2XX modem with SMSSend() instruction in CRBasic\*

An alternative to using EmailRelay() is the SMSSend() instruction. This requires SMS messaging to be enabled when provisioning your cellular service a feature not supported by Campbell Scientific s cellular plans. You ll also need a Campbell Scientific CELL2XX modem, either internal or connected to the data logger via PPP. With these requirements met, you can use SMSSend() to send messages to specified phone numbers. However, SMS support is not commonly enabled by default, so ensure your SIM card includes this feature when activated by your provider. This functionality is also available over PPP on older data loggers such as the CR800, CR1000, and CR3000. Contact Campbell Scientific for assistance if you're using one of these models.

## Notifyre

Notifyre is a cloud-based communication platform that enables businesses to send and receive SMS messages. There are upfront registration fees as well as ongoing monthly fees.

To send SMS messages via Notifyre, the data logger uses the HTTPPost() instruction to communicate with the Notifyre API, which then delivers the message. Campbell Scientific has tested this method and found it responsive, but it is not officially supported. Note that it requires TLS over HTTPS, and older dataloggers like the CR1000, CR800, and CR3000 may not be fast enough to complete the request before timing out.

URL: <https://notifyre.com/us/pricing>

See [Example Code for Notifyre](#example-code-for-notifyre) for sample code for your data logger

## EMAG (Verizon Only)

Another alternative is EMAG, a Verizon Wireless service that supports sending text messages to all cellular carriers, including via email to SMS. A Verizon Wireless cellular account is required to use this service. For more information and current pricing contact Verizon Wireless or visit their EMAG webpage here:<https://ess.emag.vzw.com/emag/>.

Pricing at the time this documentation was created was by bulk quantity per month:

- 15,000 messages is $50 (that's .0033/msg)

- 100,000 messages is $200 (.002/msg)

- \* Verizon offers pricing up to 5 million messages a month.

## Create Rules in Microsoft Outlook

If your provider still supports their email-to-text gateway service (e.g., vtext.com, txt.att.net, tmomail.net) but your messages are being blocked due to security filters, you may be able to work around this by using Microsoft Outlook rules. Outlook allows you to create rules that automate actions based on incoming messages. For example, you can create a rule to forward messages from the KonectGDS EmailRelay service to an email-to-text address.

To set this up, right-click the incoming email, then choose **Rules** > **Create Rule**.

From here, you can select many options for alerting, including certain sounds to play or moving the item to a specific folder. For forwarding rules, we will choose the**Advanced Options**button.

You can then choose what the trigger will be for this rule.

Then, choose to forward the message to a certain e-mail address and click on the blue link created to specify what e-mail address you would like to forward the message to.

You then need to specify the address to forward the message to, which will be the e-mail to text service of your choosing.

Finally, you can choose exceptions to the rule. We will not create any exceptions in this walkthrough. Name the rule and click**Finish**.

If you experience issues with the e-mail bouncing and not going through, there may be some domain permissions with your e-mail that restrict creating rules that affect external addresses. You will need to speak to your IT department or consider some of the other options in this document. You could also choose to enact some of the other Outlook rules at your disposal.

## Microsoft Power Automate

An alternative method to [Create Rules in Microsoft Outlook](#create-rules-in-microsoft-outlook) is to use Power Automate. You can choose to create a flow that is triggered by an e-mail send in Microsoft s Power Automate cloud-based software. You can then do many things based on receiving that e-mail such as sending another e-mail out to an e-mail-to-text service. Please make sure to consult with your IT department before exploring this route.

As you can see, this is very similar to rule creation in Outlook.

## Other Text Messaging Services

Many other text messaging services are available. Campbell Scientific doesn t specifically recommend any of them, but they may be able to offer the functionality you are looking for. Be sure to do your own research and evaluate each of them based on their available features and cost.

Many of these services operate similarly to Notifyre. These text message providers include:

- ClickSend

- Trumpia

- Textedly

- Clickatell

## Example Code for [Notifyre](#notifyre)

```crbasic
Public PTemp, Batt_volt
Public textsendtest As Boolean
Public ContentsString As String *500
Public HTTPResponse As String *200
Public NotifyreURL As String *100
Public campaignname As String *100
Public HTTPHeader As String *200
Public ContentsString1 As String *200
Public ContentsString2 As String *240
Public ContentsString3 As String *200

Public fromnumber As String *15
Public recipient1 As String *15
Public recipient2 As String *15
Public recipient3 As String *15
Public recipient4 As String *15

Public recipientstring1 As String *70
Public recipientstring2 As String *60
Public recipientstring3 As String *60
Public recipientstrings As String *200

Public RecipientNum As Long

Public MessageBody As String *250
Public APIKey As String *100

'Define Data Tables
DataTable (Test,1,-1) 'Set table size to # of records, or -1 to autoallocate.
DataInterval (0,15,Sec,10)
Minimum (1,Batt_volt,FP2,False,False)
Sample (1,PTemp,FP2)
EndTable

'Main Program
BeginProg
'This is the correct URL. If you make changes ensure your code matches the cURL example from Notifyre instead of the .Net, Node.js, or PHP.

'Notifyre API Documentation available here: https://docs.notifyre.com/api/sms-send

NotifyreURL = "https://api.notifyre.com/sms/send"

'Your from number needs +1 appended to the front of it

fromnumber = "+1XXXXXXXXXX"

'Enter the number of recipients here. The remaining fields will be ignored

RecipientNum = 4

'Your recipients numbers need +1 appended to the front of them. Multiple recipients need to be individually enclosed in quotes and separated by a comma.

recipient1 = "+1XXXXXXXXXX"
recipient2 = "+1XXXXXXXXXX"
recipient3 = "+1XXXXXXXXXX"
recipient4 = "+1XXXXXXXXXX"

MessageBody = "Test Text Message! Be sure to check the battery voltage level to ensure the station is operational after dark."

APIKey = "API Key from Notifyre goes Here"

campaignname = "Enter your Campaign Name you have entered in Notifyre Here"

Scan (1,Sec,0,0)
PanelTemp (PTemp,15000)
Battery (Batt_volt)
CallTable Test
NextScan

SlowSequence
Scan (5,Sec,3,0)

If textsendtest = -1 Then
recipientstring1=recipient1
recipientstrings=recipientstring1
If RecipientNum = 2 Then
recipientstring1=recipient1 &CHR(34)&CHR(13)&CHR(10)& "}" &CHR(44) &CHR(13)&CHR(10)&"{" &CHR(13)&CHR(10)&CHR(34)& "type"&CHR(34)&":"&CHR(34)& "mobile_number"&CHR(34)&CHR(44)&CHR(13)&CHR(10)& CHR(34)& "value"&CHR(34)&":"&CHR(34)& recipient2
recipientstrings=recipientstring1
EndIf
If RecipientNum= 3 Then
recipientstring1=recipient1 &CHR(34)&CHR(13)&CHR(10)& "}" &CHR(44) &CHR(13)&CHR(10)&"{" &CHR(13)&CHR(10)&CHR(34)& "type"&CHR(34)&":"&CHR(34)& "mobile_number"&CHR(34)&CHR(44)&CHR(13)&CHR(10)& CHR(34)& "value"&CHR(34)&":"&CHR(34)& recipient2
recipientstring2=CHR(34)&CHR(13)&CHR(10)& "}" &CHR(44) &CHR(13)&CHR(10)&"{" &CHR(13)&CHR(10)&CHR(34)& "type"&CHR(34)&":"&CHR(34)& "mobile_number"&CHR(34)&CHR(44)&CHR(13)&CHR(10)& CHR(34)& "value"&CHR(34)&":"&CHR(34)& recipient3
recipientstrings=recipientstring1 & recipientstring2
EndIf
If RecipientNum = 4 Then
recipientstring1=recipient1 &CHR(34)&CHR(13)&CHR(10)& "}" &CHR(44) &CHR(13)&CHR(10)&"{" &CHR(13)&CHR(10)&CHR(34)& "type"&CHR(34)&":"&CHR(34)& "mobile_number"&CHR(34)&CHR(44)&CHR(13)&CHR(10)& CHR(34)& "value"&CHR(34)&":"&CHR(34)& recipient2
recipientstring2=CHR(34)&CHR(13)&CHR(10)& "}" &CHR(44) &CHR(13)&CHR(10)&"{" &CHR(13)&CHR(10)&CHR(34)& "type"&CHR(34)&":"&CHR(34)& "mobile_number"&CHR(34)&CHR(44)&CHR(13)&CHR(10)& CHR(34)& "value"&CHR(34)&":"&CHR(34)& recipient3
recipientstring3=CHR(34)&CHR(13)&CHR(10)& "}" &CHR(44) &CHR(13)&CHR(10)&"{" &CHR(13)&CHR(10)&CHR(34)& "type"&CHR(34)&":"&CHR(34)& "mobile_number"&CHR(34)&CHR(44)&CHR(13)&CHR(10)& CHR(34)& "value"&CHR(34)&":"&CHR(34)& recipient4
recipientstrings=recipientstring1 & recipientstring2 & recipientstring3
EndIf
ContentsString1 = "{" &CHR(13)&CHR(10)&CHR(34)& "Body"&CHR(34)&":"&CHR(34)& MessageBody &CHR(34)&CHR(44)&CHR(13)&CHR(10)&CHR(34)& "Recipients"&CHR(34)&":["&CHR(13)&CHR(10)&"{" &CHR(13)&CHR(10)&CHR(34)& "type"&CHR(34)&":"&CHR(34)& "mobile_number"&CHR(34)&CHR(44)&CHR(13)&CHR(10)& CHR(34)& "value"&CHR(34)&":"&CHR(34)
ContentsString2 = recipientstrings &CHR(34)&CHR(13)&CHR(10)& "}" &CHR(13)&CHR(10)&"]"&CHR(44)&CHR(13)&CHR(10)&CHR(34)&"From"&CHR(34)&":" &CHR(34)& fromnumber &CHR(34)&CHR(44)&CHR(13)&CHR(10)&CHR(34)& "AddUnsubscribeLink"&CHR(34)&":"&"false"&CHR(44)&CHR(13)&CHR(10)
ContentsString3 = CHR(34)&"CampaignName"&CHR(34)& ":" &CHR(34)& campaignname &CHR(34) &CHR(13)&CHR(10)& "}"
ContentsString = ContentsString1 & ContentsString2 & ContentsString3
HTTPHeader = "x-api-token: " & APIKey &CHR(13)&CHR(10)& "Content-Type: application/json"
HTTPPost (NotifyreURL,ContentsString,HTTPResponse,HTTPHeader)
textsendtest = 0
EndIf
NextScan
EndProg
```
