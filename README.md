# A-HLS Epic Patient Search Documentation 

## **Overview**

The Epic Patient Search accelerator enables customers to search for a patient in their Epic system from the Salesforce Health Cloud console. A user would enter the patient's First Name, Last Name, Date of Birth, and Email (optional) to see the matching results. The user can then open the patient's record in Salesforce to view their detail. 

The accelerator itself does not require any middleware. After a few configuration steps, an organization is ready to start using the Patient Search component in their workflows.

It is assumed that an organization has enabled their Patient R4 FHIR API in their Epic system which is what supports the patient search capability.



------

## **Business Objective**

This accelerator aims to reduce the cost of IT by providing a free, lightweight, integration point with a customer's Epic instance. 
It will also improve a user's experience by providing a single user interface to search for, find, and open a patient's record without needing to switch systems.

## **Business Value and Benefits**

- Increased Efficiency
- Improved User Experience
- Decreased IT Costs
- Decreased Time to Value

------

## **Industry Focus and Workflow**

### **Primary Industry:**

- Provider
- Payer

### **Primary User Persona:**

- Call Center Representatives
- Care Coordinators
- Patient Services Representatives

### **User Workflow:**

- Enter the first and last name of the desired patient
- Enter the date of birth and optionally the email of the patient
- Click Search and then view the results or open the patient record

------

## **Package Includes:**

### **OmniScript (#)**

- EHR/EpicPatientSearch

### **Integration Procedures (2)**

- EHRConnect/getToken
- EpicFHIR/PatientSearch
- Patient/SearchCreate

### **Apex Classes (1)**

- getToken

### **Custom Settings (1)**

- Epic FHIR Endpoint

### **Custom Objects (1)**

- Epic Accesses

------

## **Configuration Requirements**

### **Pre-Install Configuration Steps:**

1. **EHR Pre-Configuration Steps:**

2. 1. Identify your [fhir.epic.com](https://fhir.epic.com/) endpoint

   2. Confirm that your endpoint is configured such that the following APIs are active - for full reference, please refer to the [fhir.epic.com](http://fhir.epic.com) API documentation.

   3. 1. api/FHIR/R4/Patient

   4. Ensure your EHR system's network is configured to accept traffic from your Health Cloud org

3. **Health Cloud Pre-Configuration Steps:**

4. 1. Add the [fhir.epic.com](https://fhir.epic.com/) endpoint to the Remote Site Settings in your Health Cloud org

#### Install the Accelerator 

1. Use IDX to install the files located in the GitHub repository here: [https://github.com/healthcare-and-life-sciences/epic-patient-search](https://github.com/healthcare-and-life-sciences/epic-fhir-clinical-flexcards)

2. 1. To access the IDX workbench, please navigate to this URL: https://workbench.developerforce.com/login.php
   2. For more information regarding IDX, please review this Trailhead: https://trailhead.salesforce.com/content/learn/modules/omnistudio-developer-tools

3. Install the Epic on FHIR App called “**Salesforce Health Cloud - Clinical Summary**” into your Epic organization.

4. 1. **Client ID:** 43b0500b-ea80-41d4-be83-21230c837c15

### **Post-Install Configuration Steps:**

1. **Add Custom Setting for your endpoint**

2. 1. Set the following attributes:

   2. 1. Name = “Production”
      2. URL = your organization’s fhir endpoint URL domain. E.g. - 
         https://fhir.epic.com/interconnect-fhir-oauth
      3. client_id = the Client ID for the Salesforce Health Cloud - Clinical Summary FHIR App
      4. jti = salesforce (or your label of choice)
      5. cert = epicfhirnewcert



1. **Grant Access to the Epic Access Object**

2. 1. Go to Settings > Profiles and open the Profile of your Administrator
   2. Select “Object Settings” > Epic Access
   3. Edit the Settings and change the Tab Access from Tab Hidden to “Default On”
   4.
   5. Ensure that the Access Token and ExpireEpoch fields are set to Read and Edit
   6. Click Save

3. **Create a single Epic Access record** 

4. 1. Give the Epic Access record a Name/Title of “Epic”.

1. Click on **App Launcher** → Search for “OmniScripts”

2. 1. Navigate to the recently installed OmniScript in the list view
   2. Click on the dropdown at the right of the OmniScript and select **Activate**.
   3. For more information regarding activating Omniscripts, please see this article: https://help.salesforce.com/s/articleView?id=sf.os_activating_omniscripts.htm&type=5

3. Add the installed OmniScript to the lightning page layout of your choosing. 

4. 1. Refer to this article for more information regarding adding OmniScripts to a Lightning Page: https://help.salesforce.com/s/articleView?id=sf.os_add_a_standard_omniscript_component_to_a_lighting_page_20263.htm&type=5
   2. Refer to this article for more information regarding adding OmniScripts to an Experience Cloud Page: https://help.salesforce.com/s/articleView?id=sf.os_add_a_standard_omniscript_component_to_an_experience_page_20341.htm&type=5

------

## **Assumptions**

1. A customer has licenses for Health Cloud, and the HINS Managed Package with OmniStudio. These solutions have all been installed and are functional.
2. A customer is assuming Salesforce Lightning Experience — not Classic.
3. Data Model elements that are part of the HINS (Vlocity) Managed package and Health Cloud are all available.
4. The Accelerator uses the Lightning Design System standards and look. Customers may want to apply their own branding which can be achieved.
5. A customer is live on Epic EHR and has the appropriate resources to enable the indicated APIs mentioned above.

------

## **Revision History**

- **Revision Short Description (Month Day, Year)**

- - Initial Draft (01/04/2023)
