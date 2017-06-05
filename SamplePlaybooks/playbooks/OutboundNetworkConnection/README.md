---
author: Jon
version: 1.0.0
tags: Network
---

# Playbook: Outbound network alert triage

## Elevator pitch

This is a general playbook that understands the basics of all outbound network alerts (IPs involved, devices, users). Its goal is to bring initial context and enable pivoting. Users can pivot into an endpoint to look for the root cause of the alert.

## Alerts captured

Any alert involving an outbound connection. In particular we expand from the originating device, the target domain, the user involved and the action taken by the network element reporting the alert.

## Investigation questions

<!-- The first level questions are typically hypotheses which we write as an assertion and in italics -->

1.  *There is no security threat, this is a legitimate connection* 
    1.  <a name="1.1"></a> Are there any red flags on the connection target?
        1.  <a name="1.1.1"></a> Is the destination a clean domain & IP? 
            1.  Does VT flag it as known with good reputation? [](# "@UseVT")
        2.  Does it have global high prevalence? 
    2.  Is the user the likely user to be using the endpoint? 
    3.  Is this event expected?
    4.  Is the endpoint in good shape?
        1.  [Does the endpoint contain evidence of malware persistence?](../MalwareAlertTriage/README.md#1.2)
        2.  Are there an abnormal number of malware alerts in the recent past?
    5.  Is the process opening the socket expected to do so? 
        1.  Is the process in good shape (no injection, abnormal behavior)?
2.  *This network request is driven by malware* 
    1.  Where did it come from? (i.e.)
        1.  Is it a web downloaded? 
            1.  Check web default download folder (with an endpoint snapshot: expensive) 
            2.  Query web gateway (can be done through ePO/SIEM?) 
        2.  Was it attached in an email? 
            1.  Query email gateway (can be done through ePO/SIEM?) 
    2.  [Does the endpoint contain evidence of malware persistence?](../MalwareAlertTriage/README.md#1.2)
    3.  What is the impact in the organization?
        1.  Is the malware running in any other device? 
    4.  Are there any high value assets involved on this case?
        1.  Is there a server or critical infrastructure involved?
3.  *This is a C&C scenario* 
    1.  Did the connection take place at normal times? 
    2.  Was the endpoint located on the expected places?
    3.  [Is the destination a clean domain & IP?](#1.1.1) 
    4.  Does the destination have global high prevalence? 
