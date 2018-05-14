# Working with the AS4 Connector

[[overview]](#overview)   [[Operation details ]](#operation-details)   [[Sample configuration]](#sample-configuration)  [[Sample P-Mode configuration for the AS4 minimal client profile]](#sample-p-mode-configuration-for-the-as4-minimal-client-profile)

## overview
The following operations allow you to work with the AS4 connector. Click an operation name to see details on how to use it.

For a sample proxy service that illustrates how to work with the AS4 connector, see [Sample configuration](#sample-configuration).

For a sample P-Mode configuration that is compliant with the AS4 minimal client profile, see [Sample P-Mode Configuration for the AS4 Minimal Client Profile](#sample-p-mode-configuration-for-the-as4-minimal-client-profile).

## Operation details
   This section provides more information on the AS4 connector operations.
   
   **Sending an AS4 message to a messaging services handler**
   
   The send operation converts an incoming soap message to an AS4 compliant soap message by referring to a specified P-Mode, and then sends the AS4 message to a Messaging Services Handler (MSH).
   
   The incoming message can take any of the following forms:
   
   * A SOAP message with attachments.
   * A SOAP message that contains the payload in the body.
   Create a folder named pmodes under the <EI_HOME>/conf/ directory and place the default P-Mode configuration files.

**send**
```xml
<as4.send>
   <pmode>http://wso2.org/examples/agreement0</pmode>
</as4.send>
```
**Properties** 
* pmode: The P-Mode agreement that is referred.

**Receiving an AS4 message from a messaging services handler**

The receive operation accepts an AS4 message with the payload, saves the payload in the dataIn location specified in the configuration,  and then send back the receipt/error back to the sending MSH.

**receive**
```xml
<as4.receive>
   <dataIn>as4DataIn</dataIn>
</as4.receive>
```
**Properties** 
* dataIn: The location where the incoming AS4 messages and payloads are saved.

## Sample configuration
Following is a sample proxy service that illustrates how to connect to the AS4 connector and use the send operation to send an AS4 message. 

 
**Sample Proxy**
```xml
<proxy xmlns="http://ws.apache.org/ns/synapse"
       name="AS4SenderProxy"
       transports="http https"
       startOnLoad="true">
   <description/>
   <target>
      <inSequence>
         <log/>
         <as4.send>
            <pmode>http://wso2.org/examples/agreement0</pmode>
         </as4.send>
         <respond/>
      </inSequence>
      <outSequence>
         <log/>
      </outSequence>
   </target>
</proxy>
```
Following is a sample request message for the sample proxy given above:

**Sample request**
```
------=_Part_0_1401281349.1502438569305
Content-Type: application/soap+xml; charset=UTF-8
Content-Transfer-Encoding: 8bit
Content-ID: <rootpart@soapui.org>
 
<soap:Envelope xmlns:soap="http://www.w3.org/2003/05/soap-envelope">
   <soap:Header/>
   <soap:Body/>
</soap:Envelope>
------=_Part_0_1401281349.1502438569305
Content-Type: text/xml; charset=us-ascii; name=simple_document.xml
Content-Transfer-Encoding: 7bit
Content-ID: <simple_document.xml>
Content-Disposition: attachment; name="simple_document.xml"; filename="simple_document.xml"
 
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/">
<soapenv:Header/>
<soapenv:Body>
This is a simple document
<soapenv:Body/>
</soapenv:Envelope>
 
------=_Part_0_1401281349.1502438569305--
```

Following is a sample response message for the sample request that is sent:


**Sample response**
```
<?xml version='1.0' encoding='UTF-8'?>
<soapenv:Envelope xmlns:soapenv="http://www.w3.org/2003/05/soap-envelope">
    <soapenv:Header>
        <eb3:Messaging xmlns:eb3="http://docs.oasis-open.org/ebxml-msg/ebms/v3.0/ns/core/200704/" xmlns:ns2="http://www.w3.org/2003/05/soap-envelope" ns2:mustUnderstand="true">
            <eb3:SignalMessage>
                <eb3:MessageInfo>
                    <eb3:Timestamp>2017-08-11T13:32:50.780+05:30</eb3:Timestamp>
                    <eb3:MessageId>cf089329-0189-4b11-b7c4-465641fbd039</eb3:MessageId>
                    <eb3:RefToMessageId>c2043863-e245-429f-840d-4b05f8664e15</eb3:RefToMessageId>
                </eb3:MessageInfo>
           </eb3:SignalMessage>
        </eb3:Messaging>
    </soapenv:Header>
    <soapenv:Body/>
</soapenv:Envelope>
```
## Sample P-Mode configuration for the AS4 minimal client profile

Following is a sample P-Mode configuration that is compliant with the AS4 minimal client profile: Create a file named push.xml with this configuration and place it under the pmodes folder.

```
<PMode xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://wso2.org/schemas/2016/10/pmode" xsi:pmode="http://wso2.org/schemas/2016/10/pmode ../../repository/xsd/pmode.xsd">
      <ID>push</ID>
      <MEP>http://www.oasis-open.org/committees/ebxml-msg/one-way</MEP>
      <MEPbinding>http://www.oasis-open.org/committees/ebxml-msg/push</MEPbinding>
      <Initiator>
           <Party>org:wso2:example:company:A</Party>
           <Role>Sender</Role>
      </Initiator>
      <Responder>
           <Party>org:wso2:example:company:B</Party>
           <Role>Receiver</Role>
      </Responder>
      <Agreement>
            <Name>http://wso2.org/examples/agreement0</Name>
      </Agreement>
      <Protocol>
           <Address>http://localhost:8280/services/AS4ReceiverProxy</Address>
           <SOAPVersion>1.2</SOAPVersion>
      </Protocol>
      <BusinessInfo>
           <Service>Examples</Service>
           <Action>StoreMessage</Action>
      </BusinessInfo>
      <ErrorHandling>
           <Report>
                <AsResponse>true</AsResponse>
                <ProcessErrorNotifyProducer>true</ProcessErrorNotifyProducer>
           </Report>
      </ErrorHandling>
</PMode>
```
