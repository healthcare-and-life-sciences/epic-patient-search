![](/images/ahlsbanner.png)

# A-HLS Epic Patient Search Documentation 

## **Overview**

The Epic Patient Search accelerator enables customers to search for a patient in their Epic system from the Salesforce Health Cloud console. A user would enter the patient’s information to view results. The user has a choice of searching by Patient ID (SSN, FHIRID, or MRN), Name and Birthdate, or Name, Legal Sex, and Phone/Email. The user can then open the patient's record in Salesforce to view their detail. 

The accelerator is compatible with both MuleSoft or via a direct connection via FHIR APIs with Epic. After a few configuration steps, an organization is ready to start using the Patient Search component in their workflows.

The setup instructions below detail how to connect the Accelerator directly to your Epic instance. Please contact your MuleSoft AE for support in connecting the Accelerator to MuleSoft.

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

### **Integration Procedures (3)**

* EHR/AuthAndSearch
* EpicFHIR/PatientSearch
* Patient/SearchCreate

### DataRaptors (4)

* EpicPatientSearchTransform
* EpicPatientSearchTransform2
* DRFindPersonAccount
* DRCreatePersonAccount

### Documents (Logos) (4)

* Epic logo transparent.png
* nurse.png
* clinical_fe.png
* phone.png

### FlexCards (1)

* EpicFHIRPatientSearchResults

### **Custom Apex Class (1)**

* ClientCredentialsJWT.cls

* * *

## **Configuration Requirements**

### **Pre-Install Configuration Steps:**

1. **EHR Pre-Instllation Steps:**
    1. If you are connecting to Epic directly, identify your [fhir.epic.com](https://fhir.epic.com/) endpoint. If you are connecting to Epic via MuleSoft, you can skip this step.
    2. Confirm that your endpoint is configured such that the following APIs are active - for full reference, please refer to the [fhir.epic.com](http://fhir.epic.com/) API documentation.
        1. api/FHIR/R4/Patient
    3. If you are connecting to Epic directly, ensure your EHR system's network is configured to accept traffic from your Health Cloud org. If you are connecting to Epic via MuleSoft, you can skip this step.
    4. If you are connecting to Epic directly, install the Epic on FHIR App called “**Salesforce Health Cloud - Clinical Summary**” into your Epic organization. If you are connecting to Epic via MuleSoft, you can skip this step.
        1. **Client ID:** 43b0500b-ea80-41d4-be83-21230c837c15
2. **Salesforce Pre-Installation Steps:**
    1. Ensure your Salesforce Health Cloud org has OmniStudio installed - either the Vlocity HINS package or Core OmniStudio. 
        1. To verify installation, please navigate to Setup > Installed Packages > OmniStudio.
    2. Enable Identity Provider according to these steps: https://help.salesforce.com/s/articleView?id=sf.identity_provider_enable.htm&type=5
    3. If you are connecting to Epic directly, create Custom Metadata for Authentication Provider. If you are connecting to Epic via MuleSoft, you can skip this step.
        1. Setup > Custom Metadata Types
        2. New Metadata Type - ensure it is named **“ClientCredentialsJWT”**
        3. Add the following Custom fields:
            1. aud
            2. callback uri
            3. cert
            4. iss
            5. jti
            6. sub

![](/images/psimage3.png)

#### Install the Accelerator 

1. Follow the download steps in the "Download Now" flow presented on the HLS Accelerators website for this Accelerator which downloads the following GitHub repository on your machine: https://github.com/healthcare-and-life-sciences/epic-fhir-clinical-flexcards
2. Unzip the resulting .zip file which is downloaded to your machine. 
3. Open the “OmniStudio” folder
    1. If you have the Vlocity_ins package installed in your org, open the folder titled “Vlocity Version”.
    2. If you have Core OmniStudio installed in your org, or if you have converted over from Vlocity_ins to Core, open the folder titled “Core OmniStudio Version”.
    3. Install the appropriate DataPack into your org. 
        1. Click on App Launcher → Search for 'OmniStudio DataPacks' and click on it.
        2. Click on 'Installed' > Import > From File
        3. When the window opens, select the json file identified in the previous step. Click 'Open' then click 'Next' 3 times.
        4. If prompted to Active, choose Activate Later. 
4. If you are connecting to Epic directly, open the “salesforce-sfdx” folder. Use IDX or sfdx to install the files under the “salesforce-sfdx” folder. If you are connecting to Epic via MuleSoft, you can skip this step.
    1. To access the IDX workbench, please navigate to this URL: https://workbench.developerforce.com/login.php
    2. For more information regarding IDX, please review this Trailhead: https://trailhead.salesforce.com/content/learn/modules/omnistudio-developer-tools
5. If you are connecting to Epic directly, import the keystore FHIRDEMOKEYSTORE.jks from .zip file. If you are connecting to Epic via MuleSoft, you can skip this step.
    1. Setup > Certificates and Key Management > Import from Keystore
    2. Password: salesforce1

### **Post-Install Configuration Steps - Direct Connection to Epic FHIR APIs:**

1. Create a new Authentication Provider. If you are connecting to Epic via MuleSoft, you can skip this step.
    1. Setup > Auth Providers
    2. Create a New Authentication Provider
        1. Provider Type: Custom
        2. Name: Epic_JWT_Auth
        3. Plugin: ClientCredentialJWT
        4. iss: 43b0500b-ea80-41d4-be83-21230c837c15
        5. sub: 43b0500b-ea80-41d4-be83-21230c837c15
        6. aud: set this to the API endpoint for authentication - either the MuleSoft API or Epic FHIR API - e.g., https://fhir.epic.com/interconnect-fhir-oauth/oauth2/token
        7. jti: salesforce
        8. cert: fhirdemo_cert
        9. callback uri: [https://](https://%3Cyour/)<your salesforce org domain>/services/authcallback/Epic_JWT_Auth
2. Add your API endpoint to Remote Site Settings. If you are connecting to Epic via MuleSoft, you can skip this step.
    1. Setup > Remote Site Settings
    2. New
    3. Enter a name and then the URL of your API - e.g., https://fhir.epic.com/interconnect-fhir-oauth/ 
    4. Save



![](/images/psimage4.png)
3. **Create a new Named Credential - Direct Connection to Epic FHIR APIs:**
    1. Setup > Named Credential > New Legacy. If you are connecting to Epic via MuleSoft, you can skip this step.
        1. Name: Must be Epic Auth JWT
        2. URL: the URL of the endpoint you are going to connect to. For example, https://fhir.epic.com/interconnect-fhir-oauth/ 
        3. Identity Type: Named Principal
        4. Authentication Protocol: OAuth 2.0
        5. Authentication Provider: the name of your Authentication Provider above
        6. “Run Authentication Flow on Save”: Checked

4. If you are using MuleSoft to connect to Epic, follow these steps to configure the Integration Procedure to call MuleSoft APIs:
   1. Follow the Epic Administration System API Setup Guide, if it has not already been completed as part of your MuleSoft implementation. https://anypoint.mulesoft.com/exchange/org.mule.examples/hc-accelerator-epic-us-core-administration-sys-api/minor/1.0/pages/edh-nhj/Setup%20Guide/
   2. Create new Remote Site (Setup > Remote Site Settings > New Remote Site) with the URL of the newly-created MuleSoft app.
   3. Create a Named Credential for your MuleSoft app.
   4. Update each HTTP Action element of the Integration Procedure titled EpicFHIRPatientSearch with the following:
      1. Path = endpoint of your MuleSoft app without the domain. For example: /api/R4/Patient.
      2. Named Credential = the name of your MuleSoft Named Credential from step 3 above.
      3. Use the Preview pane to test the updates to the Integration Procedure.
      4. Activate the Integration Procedure.


![](/images/psimage5.png)
5. Click on **App Launcher →** Search for “**FlexCards**”
    1. Open the EpicPatientSearchResults Flexcards
    2. Deactivate and re-Activate the FlexCard
    3. Choose the following Publish Options:
        1. App Page
        2. Home Page
        3. Record Page

![](/images/psimage6.png)

6. Click on **App Launcher** >> Search for "Integration Procedures"
    1. Open the EHR/AuthAndSearch Integration Procedure. 
    2. Click "Activate" at the bottom of the Procedure Configuration screen.
    3. Close the Integration Procedure.
    4. Repeat the steps above for the Integration Procedures titled "EpicFHIR/PatientSearch" and "Patient/SearchCreate".

7. Click on **App Launcher** → Search for “OmniScripts”
    1. Navigate to the recently installed OmniScript in the list view - EHR/EpicPatientSearch
        1. Deactivate the OmniScript
        2. Open up the Step 1 Element and click on the selection boxes which hold the three search options. 
        3. Øn the Properties pane, click on the hyperlink name of one of the option titles, and then click the Save button. 
        4. **Activate** the OmniScript. Be sure to activate the FlexCard in the previous step before re-activating the OmniScript.
        5. If you are using Core OmniStudio on Health Cloud, click on the drop-down arrow next to "Active" and select "Deploy Standard Run Time Compatible LWC".
    2. For more information regarding activating Omniscripts, please see this article: https://help.salesforce.com/s/articleView?id=sf.os_activating_omniscripts.htm&type=5

8. Click on **App Launcher** → Search for “DataRaptors” 
    1. Open the **DRCreatePersonAccount** DataRaptor
    2. Navigate to the **Formulas** tab
    3. Replace the value in the left hand pane to your org’s **Record Type ID** value for the **Person Account Record Type**

![](/images/psimage7.png)

9. Add the installed OmniScript to the App Home Page of your choosing. 
    1. Refer to this article for more information regarding adding OmniScripts to a Lightning Page: https://help.salesforce.com/s/articleView?id=sf.os_add_a_standard_omniscript_component_to_a_lighting_page_20263.htm&type=5

* * *

## **Assumptions**

1. A customer has licenses for Health Cloud withe OmniStudio, or the HINS Managed Package with OmniStudio. These solutions have all been installed and are functional. 
2. A customer is assuming Salesforce Lightning Experience — not Classic.
3. Data Model elements that are part of the HINS (Vlocity) Managed package or Health Cloud with OmniStudio are all available.
4. The Accelerator uses the Lightning Design System standards and look. Customers may want to apply their own branding which can be achieved.
5. A customer has an administrator/developer who is familiar with IDX and OmniStudio.
6. A customer is live with Epic EHR and has their FHIR R4 server active.
7. Epic is a trademark of Epic Systems Corporation.

* * *

## **Revision History**

* **Revision Short Description (Month Day, Year)**
    * January 4, 2023 - Initial draft
    * January 23, 2023 - Updated documentation
    * April 7, 2023 - Updated documentation
    * May 4, 2023 - updated with configuration steps for MuleSoft customers






