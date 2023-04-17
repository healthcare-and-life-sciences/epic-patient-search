![](/images/ahlsbanner.png)

# A-HLS Epic Patient Search Documentation 

## **Overview**

The Epic Patient Search accelerator enables customers to search for a patient in their Epic system from the Salesforce Health Cloud console. A user would enter the patient’s information to view results. The user has a choice of searching by Patient ID (SSN, FHIRID, or MRN), Name and Birthdate, or Name, Legal Sex, and Phone/Email. The user can then open the patient's record in Salesforce to view their detail. 

The accelerator is compatible with both MuleSoft or via a direct connection via FHIR APIs with Epic. After a few configuration steps, an organization is ready to start using the Patient Search component in their workflows.

It is assumed that an organization has enabled their Patient R4 FHIR API in their Epic system which is what supports the patient search capability.

![](/images/psimage1.png)
![](/images/psimage2.png)

## **Business Objective**

This accelerator aims to reduce the cost of IT by providing a free, lightweight, integration point with a customer's Epic instance. 
It will also improve a user's experience by providing a single user interface to search for, find, and open a patient's record without needing to switch systems.

## **Business Value and Benefits**

* Increased Efficiency
* Improved User Experience
* Decreased IT Costs
* Decreased Time to Value

* * *

## **Industry Focus and Workflow**

### **Primary Industry:**

* Provider
* Payer

### **Primary User Persona:**

* Call Center Representatives
* Care Coordinators
* Patient Services Representatives

### **User Workflow:**

* Choose a search method
* Enter the patient’s information
* Click Search 
* Click on the correct patient to open their Salesforce record

* * *

## **Package Includes:**

### **OmniScript (1)**

* EHR/EpicPatientSearch

### **Integration Procedures (2)**

* EHR/AuthAndSearch
* EpicFHIR/PatientSearch
* Patient/SearchCreate

### FlexCards (1)

* EpicFHIRPatientSearchResults

### **Custom Apex Class (1)**

* ClientCredentialsJWT.cls

* * *

## **Configuration Requirements**

### **Pre-Install Configuration Steps:**

1. **EHR Pre-Instllation Steps:**
    1. Identify your [fhir.epic.com](https://fhir.epic.com/) endpoint
    2. Confirm that your endpoint is configured such that the following APIs are active - for full reference, please refer to the [fhir.epic.com](http://fhir.epic.com/) API documentation.
        1. api/FHIR/R4/Patient
    3. Ensure your EHR system's network is configured to accept traffic from your Health Cloud org
    4. Install the Epic on FHIR App called “**Salesforce Health Cloud - Clinical Summary**” into your Epic organization.
        1. **Client ID:** 43b0500b-ea80-41d4-be83-21230c837c15
2. **Salesforce Pre-Installation Steps:**
    1. Ensure your Salesforce Health Cloud org has OmniStudio installed. 
        1. To verify installation, please navigate to Setup > Installed Packages > OmniStudio.
        2. If OmniStudio is not installed, please follow the instructions found here: https://help.salesforce.com/s/articleView?id=sf.os_omnistudio_release_summary_for_installation_and_upgrade.htm&type=5
    2. Create Custom Metadata for Authentication Provider:
        1. Setup > Custom Metadata Types
        2. New Metadata Type - ensure it is named **“ClientCredentialJWT”**
        3. Add the following Custom fields:
            1. aud
            2. callback url
            3. cert
            4. iss
            5. jti
            6. sub

![](/images/psimage3.png)

#### Install the Accelerator 

1. Use IDX to install the files located in the GitHub repository here: [https://github.com/healthcare-and-life-sciences/epic-patient-search](https://github.com/healthcare-and-life-sciences/epic-fhir-clinical-flexcards)
    1. To access the IDX workbench, please navigate to this URL: https://workbench.developerforce.com/login.php
    2. For more information regarding IDX, please review this Trailhead: https://trailhead.salesforce.com/content/learn/modules/omnistudio-developer-tools

### **Post-Install Configuration Steps (Direct Connection to Epic FHIR APIs):**

1. Create a new Authentication Provider
    1. Setup > Auth Providers
    2. Create a New Authentication Provider
        1. Provider Type: Custom
        2. Name: Epic_JWT_Auth
        3. Plugin: ClientCredentialJWT
        4. iss: 7a88a6d4-453f-4b34-a48e-e4a5c667285a
        5. sub: 7a88a6d4-453f-4b34-a48e-e4a5c667285a
        6. aud: set this to the API endpoint for authentication - either the MuleSoft API or Epic FHIR API - e.g., https://fhir.epic.com/interconnect-fhir-oauth/oauth2/token
        7. jti: salesforce
        8. cert: fhirdemo_cert
        9. callback url: [https://](https://%3Cyour/)<your salesforce org domain>/services/authcallback/Epic_JWT_Auth

![](/images/psimage4.png)
1. **Create a new Named Credential**
    1. Setup > Named Credential > New Legacy
        1. Give your Named Credential a name 
        2. Identity Type: Named Principal
        3. Authentication Protocol: OAuth 2.0
        4. Authentication Provider: the name of your Authentication Provider above
        5. “Run Authentication Flow on Save”: Checked

![](/images/psimage5.png)
1. Click on **App Launcher →** Search for “**FlexCards**”
    1. Open the EpicPatientSearchResults Flexcards
    2. Deactivate and re-Activate the FlexCard
    3. Choose the following Publish Options:
        1. App Page
        2. Home Page
        3. Record Page
![](/images/psimage6.png)
2. Click on **App Launcher** → Search for “OmniScripts”
    1. Navigate to the recently installed OmniScript in the list view - EHR/EpicPatientSearch
        1. Deactivate the OmniScript
        2. **Activate** the OmniScript. Be sure to activate the FlexCard in the previous step before re-activating the OmniScript.
    2. For more information regarding activating Omniscripts, please see this article: https://help.salesforce.com/s/articleView?id=sf.os_activating_omniscripts.htm&type=5
3. Click on **App Launcher** → Search for “DataRaptors” 
    1. Open the **DRCreatePersonAccount** DataRaptor
    2. Navigate to the **Formulas** tab
    3. Replace the value in the left hand pane to your org’s **Record Type ID** value for the **Person Account Record Type**

![](/images/psimage7.png)
1. Add the installed OmniScript to the App Home Page of your choosing. 
    1. Refer to this article for more information regarding adding OmniScripts to a Lightning Page: https://help.salesforce.com/s/articleView?id=sf.os_add_a_standard_omniscript_component_to_a_lighting_page_20263.htm&type=5

* * *

## **Assumptions**

1. A customer has licenses for Health Cloud withe OmniStudio, or the HINS Managed Package with OmniStudio. These solutions have all been installed and are functional. 
2. A customer is assuming Salesforce Lightning Experience — not Classic.
3. Data Model elements that are part of the HINS (Vlocity) Managed package or Health Cloud with OmniStudio are all available.
4. The Accelerator uses the Lightning Design System standards and look. Customers may want to apply their own branding which can be achieved.
5. A customer has an administrator/developer who is familiar with IDX and OmniStudio.

* * *

## **Revision History**

* **Revision Short Description (Month Day, Year)**
    * January 4, 2023 - Initial draft
    * January 23, 2023 - Updated documentation
    * April 7, 2023 - Updated documentation




