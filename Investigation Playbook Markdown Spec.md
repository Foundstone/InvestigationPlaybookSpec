# Investigation Playbook Markdown Specification

This specification shows how to use Markdown to capture investigation specific semantics.

The main goal of these extensions is to provide some structure to the investigation questions such that they can be reused, tagged and managed.

On the following sections we explain some of the extensions as well as the basic investigation playbook structure that we use. On this repository you will also find a coupe of example playbooks that use this spec.

## Table of content
1.  [Investigation playbook structure](#investigation-playbook-structure)
1.  [Markdown conventions for investigation playbooks](#markdown-conventions-for-investigation-playbooks)


## Investigation playbook structure

In order to reduce the effort of maintaining a collection of playbooks, we need to capture information about the intend and scope of a playbook. The following is a list of optional sections that help capture this information.

### Metadata

This section is a hidden YAML header that can be used to store metadata about the playbook as shown in the following example:

```
---
author: Jon
version: 1.0.0
tags: Malware
---
```

In order to comply with markdown rules this section needs to be at the beginning of the file.

### Title

The title is the first visible section. The goal should be to clearly communicate the purpose of the playbook. In particular it should include the threat being investigated, the type of evidence being used and the investigation purpose. For example: *Malware Alert triage*.

### Elevator pitch

Use this section as an extended title. The goal is to provide enough detail to capture the value of this playbook and help keep the investigation questions in line with such value.

### Alerts captured

Like in the elevator pitch section, this section adds more detail to the title and focus on the type of evidence (i.e. alerts) that will be used on the investigation questions as well as their sources (e.g. web gateway) and the way we expect to get them (e.g. from a SIEM). 

### Investigation questions

This is the main section of investigation playbook. All writing has to adhere strictly to the guidelines listed on "*[Markdown conventions for investigation playbooks](#markdown-conventions-for-investigation-playbooks)*". There should be no unstructured paragraphs on this section in order to simplify maintenance.

<!-- #### Remediation
WIP -->

## Markdown conventions for investigation playbooks
The following is a list of conventions that can be applied using markdown in order to capture some of the semantics needed for investigation playbooks. For a more general list of Markdown construct and syntax refer to [GitLab Flavored Markdown (GFM) guide](https://docs.gitlab.com/ee/user/markdown.html)

### Question importance and relationship

Use numbered lists to reflect the order in which questions should be considered. Use nesting to break complex questions into smaller questions. For example:

```
1. Question 1
	1. Question 1.1
	2. Question 1.2
```

### Question linking

Use the unique number assigned to each question as an anchor reference for linking inside a document and across documents. For example:

```
1. [Does the endpoint present performance degradation or instability](#1.3)
1. [Are there signs of pivoting actions](../NetworkCompromise/README.md#4.4)
```

Since not all the markdown interpreters generate the corresponding HTML anchor tags, add one manually to the target question as show here:

```
1. Question
	1. Question
	2. Question
	3. <a name="1.3"></a> Does the endpoint present performance degradation or instability?
```

<!-- In the future we may allow linking to a PB title instead of the file name .. 1. [Does the endpoint present performance degradation or instability](PB01#1.3) -->

### Tagging

Use `@` inside of inline comments to tag entire lines

```
1. Question 1 [](# "@MyTag")
	1. Question 2 [](# "@UseESM, @NeedsWork")
```

Sample tags types

*	Data source: What are the data sources that could be used to answer a question (e.g. @UseESM, @UseEndpointSnapshot)
*   EvidenceType: tags a question based on the type of evidence it is needed to answer it (e.g. @Email, @Endpoint, @Network, @DataLoss, @Attackers, @Users, @Malware)
*   InvestigationGoal: tag questions based on their goal (e.g. @Triage, @Scoping, @RootCause, @Attribution, @Remediation, @Hunting)
*	Stage: tags the stage of the attack lifecycle this question belongs to, using the MITRE ATT&CK chain (e.g. @Recon, @Weaponize, @Deliver, @Exploit, @Control, @Execute, @Maintain)
*	Asset: tags a question based on the type of asset at risk (e.g. @server, @workstation, @mobile, @all)
* 	Zone: tags a question based on the zone the assets at risk reside in (e.g. @lab, @dc, @dmz, @lan, @all)
*	Cost: tags a question based on the level of effort (time, data) required to answer it (@low, @medium, @high)
*	Fidelity?


<a name="implementation-details"></a> 

### Documenting implementation details

Use the HTML tag `<details>` and `Implementation` as the summary. Don't forget to delimit the paragraph using `<p>`. Markdown can (and should be used inside the paragraph). Tags can also be used to further annotate implementations.

Sample text

```
1. Question 1
	<details>
	<summary>Implementation</summary>
	
	* Use SIEM to get all the malware alerts generated by the endpoint during the investigation window [](# "@UseSIEM,@Manual")
	* Run this [Active Directory data dump script](tools/ADDumper.bat)
	
	</details>
```

<!-- Similarly we can link questions to findings by linking to the finding's .json file
	1.  [is_the_connection_to_a_site_linked_to_a_known_campaign.json](../hunter-questions/is_the_connection_to_a_site_linked_to_a_known_campaign.json) [](# "@UseESM")
 -->

Rendered result

1.  Question 1

    <details>
    <summary>Implementation</summary>

    *   Use SIEM to get all the malware alerts generated by the endpoint during the investigation window [](# "@UseSIEM,@Manual")
    *   Run this [Active Directory data dump script](tools/ADDumper.bat)

    </details>

<a name="implementation-references"></a> 

### Documenting references

Use HTML tag `<details>` and `References` as the summary. Don't forget to delimit the paragraph using `<p>`. Markdown can (and should be used inside the paragraph)

Sample text

```
1. Question 1
	<details>
	<summary>References</summary>
	
	[ATT&CK Matrix](https://attack.mitre.org/wiki/ATT%26CK_Matrix)
	
	</details>
```

Rendered result

1.  Question 1

    <details>
    <summary>References</summary>

    [ATT&CK Matrix](https://attack.mitre.org/wiki/ATT%26CK_Matrix)

    </details>

<a name="Comments"></a> 

### Commenting

There are two options for adding non visible comments to a playbook

```
<!-- HTML style multi-line comments -->

This is an in-line comment [](# "Comments here")
```
