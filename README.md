# Inform–UNICARE Interoperability Solution for Bosnia and Herzegovina (BiH) and Türkiye

This solution enables automated, secure data exchange between **Inform** (UNICEF’s data collection platform built on Ona) and **UNICARE** (an implementation of the Primero platform used for grievance, feedback, and case management). The workflow supports seamless interoperability to strengthen Complaints, Feedback, and Response Mechanisms (CFRMs) for UNICEF and its implementing partners in **Bosnia and Herzegovina (BiH)** and **Türkiye**.

The integration ensures that feedback and grievances submitted through Inform are automatically processed, validated, and registered as cases in UNICARE. This improves accountability and responsiveness by enabling real-time follow-up through a centralized case management system. The solution is adaptable for use in other countries and contexts using similar tools and workflows.

The same solution has been adapted for use in Türkiye with minor configuration differences, enabling seamless feedback processing across both countries.

## 1. Project Overview

UNICARE is UNICEF’s corporate digital solution for managing CFRMs. Developed in collaboration with EMOPS, ICTD, DAPM, Safeguarding, and CSS teams, the platform enables field offices and partners to track and manage complaints and feedback in a structured and transparent way.

This integration connects Inform with UNICARE to automate the grievance intake and case creation process. Once a feedback form is submitted in Inform, the case is created in UNICARE in near real-time, enabling streamlined follow-up by trained case workers.

## 2. Interoperability Workflows

This solution supports a one-way data flow from Inform to UNICARE. A grievance or feedback form submitted through Inform automatically triggers a webhook to OpenFn, which processes the data and creates a new case in UNICARE.

The implemented workflow in Bosnia and Herzegovina is designed to be adaptable to other UNICEF offices with similar CFRM processes.

**Workflow diagram**

_Refer to the diagram below for an overview of the data flow from Inform to OpenFn to UNICARE._
<img width="2122" height="827" alt="UNICARE BIH Turkey - Workflow" src="https://github.com/user-attachments/assets/1d60f7be-965e-48b4-8d96-7dc717c7451f" />

## 3. System Integrations

To enable automated case creation, a webhook-based integration has been configured on the [OpenFn platform](https://openfn.org). This connects the Inform API with Primero v2, which powers the UNICARE system.

**APIs used**

- **Inform (Ona)**: Inform submissions are shared with OpenFn using a webhook trigger  
  [Inform API documentation](https://data.inform.unicef.org/static/docs/data.html)

- **Primero (UNICARE)**: OpenFn creates cases using the Primero v2 API  
  [Primero API reference](https://github.com/primeroIMS/primero/blob/main/doc/api/attachments/post.md)

**OpenFn adaptors**

The integration uses the following adaptors:

- `common@2.3.1` for generic data handling and formatting  
- `primero@3.0.10` for case creation and API interaction with UNICARE

## 4. Execution Triggers

The integration is triggered automatically via a webhook configured in the Inform platform. When a user submits a feedback form, Inform immediately sends the submission payload to OpenFn via HTTP POST.

### Bosnia and Herzegovina (BiH)
**Trigger type**: Webhook  
**Source**: Inform form [BiH Face-to-Face Feedback Training Form](https://web.inform.unicef.org/x/KblafkQ1)  
**Destination**: OpenFn job webhook URL  
**Payload**: JSON-formatted form submission  
**Timing**: Triggered instantly upon form submission

### Türkiye
**Trigger type**: Webhook  
**Source**: Inform form [BiH Face-to-Face Feedback Training Form]([https://web.inform.unicef.org/x/KblafkQ1](https://web.inform.unicef.org/x/WJsD7Pi8))  
**Destination**: OpenFn job webhook URL  
**Payload**: JSON-formatted form submission  
**Timing**: Triggered instantly upon form submission

OpenFn receives the data, validates and maps the fields, and sends a case creation request to UNICARE.

## 5. Data Mappings and Protocols

Submission data from Inform is transformed to match Primero's required case structure. Field mappings include location, issue category, reporter details, and feedback description. Mappings are based on the Primero configuration in BiH and Turkey.

**OpenFn Project**:
- Project workspace and job logic can be found in the OpenFn implementation:  [OpenFn Project Workspace](https://app.openfn.org/projects/75b5159b-0102-4005-9fb7-2afa1693912e/w/020fd1c0-46dd-4f4f-ae3c-224fcca1fd0b?a=6fab331e-4f6a-4167-9219-c7ce28bbcf2d&s=46fa271d-2124-4a95-8b79-233392bd0a5a)

**Source Forms**:
- **BiH**: [BiH Face-to-Face Feedback Training Form](https://web.inform.unicef.org/x/KblafkQ1)
- **Türkiye**: [Face-to-face complaints and feedback form](https://web.inform.unicef.org/x/WJsD7Pi8)

**Mapping Sheets**:
- [BiH Mapping Specification](https://docs.google.com/spreadsheets/d/1d0IXdaI3yrvOUHdr0a6hGmUX6dhLLyK6pr8STrgcsDA)
- [Türkiye Mapping Specification](https://docs.google.com/spreadsheets/d/1p9JXzE0HDu5adol_knRa_mGPe_2xrkHXkNE61cmm4Y4)

**Static metadata mappings**

The following values are hardcoded when creating cases in UNICARE:
```
feedback_channel_eba5764: 'mobile_teams__field_staff__monitors_afdfc51',
module_id: 'primeromodule-cp'
```

These values are based on the current UNICARE configuration. Any changes to metadata in Primero must be reflected in the OpenFn configuration.

## 6. Solution Assumptions

This solution is based on the following assumptions:

- Submissions must include required fields such as location and issue category
- Personally Identifiable Information (PII) is only included when the respondent provides consent during submission
- Some metadata values are hardcoded and must align with Primero configuration
- Data flows only from Inform to UNICARE and not in the reverse direction

These assumptions should be reviewed if system configurations or forms are updated in future deployments.

## 7. Security & Data Protection
To ensure safe and responsible data handling, this integration follows UNICEF’s internal guidelines and security best practices. The checklist outlines key measures related to transport encryption, access control, authentication, PII handling, and system configuration management.

View the full Security Checklist here: [Security Checklist for Inform–UNICARE Integration](https://docs.google.com/document/d/1sx_QqQg0yPHGRI5ZVbO_SjlWiPLGoYmL/edit?usp=sharing&ouid=100028693760577730296&rtpof=true&sd=true)

## 8. Change Management Guidelines

Administrators should review the integration configuration when changes are made to the Inform form or the Primero instance. Common changes that require updates include:

- Adding, removing, or renaming form fields in Inform
- Changing location or case type options
- Modifying static metadata values in Primero such as `module_id` or feedback channels
- Updating the Inform webhook or OpenFn job structure

All job logic and field mappings are maintained in the OpenFn project. For complex updates or reconfiguration, the support of a developer or OpenFn administrator may be required.

[Access the OpenFn Project](https://app.openfn.org/projects/75b5159b-0102-4005-9fb7-2afa1693912e/w/020fd1c0-46dd-4f4f-ae3c-224fcca1fd0b?a=6fab331e-4f6a-4167-9219-c7ce28bbcf2d&s=46fa271d-2124-4a95-8b79-233392bd0a5a)
