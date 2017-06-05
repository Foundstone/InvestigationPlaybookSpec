---
author: Jon
version: 1.0.0
tags: Malware
---

# Playbook: Malware alert triage

## Elevator pitch

This is a general playbook that explores the presence of malware elements on the company network. In particular, it tries to answer basic questions like origin & spread of malware, evidence of activation, persistence mechanism, classification of purpose (C2, ransomware, etc).

## Alerts captured

Any alert signaling the presence of malware on an endpoint (for example AV alert, HIPS alert or Web proxy). 

## Investigation questions

<!-- The first level questions are typically hypotheses which we write as an assertion and in italics -->

1.  *There is no security threat, we cannot confirm malicious activity*  

    1.  Is there any evidence of suspicious outbound network connections?

        1.  Did the endpoint connect to IP or domains with bad reputation? 

            <details>
            <summary>Implementation</summary>

            *   Use SIEM to get all the network accessed blocked by Web Gateway from the endpoint during the investigation window [](# "@UseSIEM,@Manual")
            *   Collect the DNS cache of the endpoint [DNS cache using this tool](tools/ADDumper.bat) and check domains against VirusTotal [](# "@Manual")
            *   Collect memory snapshot and analyze for active network connections

            </details>

            1.  [Are there any red flags on the connection target?](../OutboundNetworkConnection/README.md#1.1) [](# "This is an example of question link")

        2.  Is the endpoint establishing network connections in abnormal times? 
        3.  [Does the endpoint present performance degradation or instability](#1.3)  [](# "This is an example of question link")
        4.  Is the network connection destination geo suspicious? [](# "TODO: SIEM has lists of corporate geos.. we could check against those")
        5.  Is the endpoint repeatedly trying to download the same resource from different URLs?
        6.  Is the endpoint exposing traffic going across nonstandard ports?
        7.  Is the endpoint performing unexpected DNS requests? [](# "@UseSIEM")
            1.  Entropy / DGAs
        8.  Is the endpoint generating abnormal web traffic?
            1.  Very fast sequence of sites accessed?
            2.  Abnormal user agents (e.g. one that doesn't match the software installed)?

    2.  <a name="1.2"></a>Is the endpoint listening on any unusual port?

        1.  Check for the evidence of a backdoor

    3.  <a name="1.3"></a>Does the endpoint contain evidence of malware persistence?

        1.  Autorun entries 
        2.  Scheduled tasks 
        3.  Hijacked DLLs 
        4.  Modifying registry keys 
        5.  File association hijack 
        6.  COM object hijacking 
        7.  Windows Management Instrumentation Event Subscription 

    4.  <a name="1.4"></a> Does the endpoint present performance degradation or instability? [](# "Many of these will have to come from an endpoint collection tool")
        1.  Is the endpoint performing high memory/CPU/disk consumption?
        2.  Is the endpoint having unusual system reboots?
        3.  Is the endpoint performing suspicious system updates?
        4.  Is the endpoint having frequent crashes? 
        5.  Is the endpoint having unusual messages/programs starting automatically?
        6.  Are the standard maintenance programs not working in the endpoint?
        7.  Is Windows Performance Monitor showing crash events, performance issues (Windows Event Log)
    5.  Is the endpoint presenting changes in the behavior of privileged users? // I don't understand this question
    6.  Is the endpoint presenting Log-in irregularities and/or failures? 
    7.  Are the involved users active employee with no red flags from HR? 

2.  *This is an opportunistic attack with common malware* 

    1.  Is it known malware? 
        1.  Why did AV not block it?
            1.  AV was outdated
            2.  AV was not running
                1.  did the user stop the AV?
                2.  when was the AV stopped?
    2.  Is it new (unknown) malware?
        1.  Am I the first one seeing this sample (hash)? 
        2.  Is it part of a well known family? 
    3.  What are the malware capabilities?
        1.  Is it a rootkit?
        2.  Does it inject into running processes?
        3.  Does it send spam?
        4.  Does it have keylogging capabilities?
        5.  Does it steal credentials?
        6.  Does it contain a backdoor?

3.  *This is an isolated infection and no additional systems are infected // What does "generalized"?*

    1.  Are there other machines in the company infected?
    2.  Are there any IT/SOC tickets where this malware gets mentioned?
    3.  Is the malware actively spreading over the network?

4.  *This is a targeted attack* // this will be redundant?
    1.  Am I the first one to observe the hash for this malware sample?
