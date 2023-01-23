![](/images/ahlsbanner.png)

# A-HLS Epic Patient Search Documentation 

## **Overview**

The Epic Patient Search accelerator enables customers to search for a patient in their Epic system from the Salesforce Health Cloud console. A user would enter the patient’s information to view results. The user has a choice of searching by Patient ID (SSN, FHIRID, or MRN), Name and Birthdate, or Name, Legal Sex, and Phone/Email. The user can then open the patient's record in Salesforce to view their detail. 

The accelerator itself does not require any middleware. After a few configuration steps, an organization is ready to start using the Patient Search component in their workflows.

It is assumed that an organization has enabled their Patient R4 FHIR API in their Epic system which is what supports the patient search capability.

![](/images/image1.png)

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

### **OmniScript (#)**

* EHR/EpicPatientSearch

### **Integration Procedures (2)**

* EHRConnect/getTokenSimple
* EpicFHIR/PatientSearch
* Patient/SearchCreate

### **Apex Classes (1)**

* getToken

### **Custom Settings (1)**

* Epic FHIR Endpoint

### **Custom Objects (1)**

* Epic Accesses

### KeyStore (1)

* FHIRDEMO.jks

* * *

## **Configuration Requirements**

### **Pre-Install Configuration Steps:**

1. **EHR Pre-Configuration Steps:**
    1. Identify your [fhir.epic.com](https://fhir.epic.com/) endpoint
    2. Confirm that your endpoint is configured such that the following APIs are active - for full reference, please refer to the [fhir.epic.com](http://fhir.epic.com/) API documentation.
        1. api/FHIR/R4/Patient
    3. Ensure your EHR system's network is configured to accept traffic from your Health Cloud org
    4. Install the Epic on FHIR App called “**Salesforce Health Cloud - Clinical Summary**” into your Epic organization.
        1. **Client ID:** 43b0500b-ea80-41d4-be83-21230c837c15
2. **Health Cloud Pre-Configuration Steps:**
    1. Add the [fhir.epic.com](https://fhir.epic.com/) endpoint to the Remote Site Settings in your Health Cloud org

#### Install the Accelerator 

1. Use IDX to install the files located in the GitHub repository here: [https://github.com/healthcare-and-life-sciences/epic-patient-search](https://github.com/healthcare-and-life-sciences/epic-fhir-clinical-flexcards)
    1. To access the IDX workbench, please navigate to this URL: https://workbench.developerforce.com/login.php
    2. For more information regarding IDX, please review this Trailhead: https://trailhead.salesforce.com/content/learn/modules/omnistudio-developer-tools
2. Enable Identity Provider in your Salesforce Org. Use a separate Self-Signed Certificate for the Identity Provider.
3. Import the FHIRDEMO.jks keystore from the GitHub repo as a new Certificate under Certificate and Keys Management. Use the password “salesforce1” for the keystore.

### **Post-Install Configuration Steps:**

1. **Add a new Epic FHIR Endpoint Custom Setting record for your organization’s Epic FHIR endpoint**
        1. Set the following attributes:
            1. Name = “Production”
            2. URL = your organization’s fhir endpoint URL domain. E.g. - 
                https://fhir.epic.com/interconnect-fhir-oauth
            3. client_id = the Client ID for the Salesforce Health Cloud - Clinical Summary FHIR App
                1. If you are testing with the Epic FHIR sandbox, please use this Client ID: fe0e3375-fffa-469b-8338-d5fe08f7d3b1
            4. jti = salesforce (or your label of choice)
            5. cert = fhirdemo_cert

1. **Grant Access to the Epic Access Object**
    1. Go to Settings > Profiles and open the Profile of your users
    2. Select “Object Settings” > Epic Access
    3. Edit the Settings and change the Tab Access from Tab Hidden to “Default On”
    4. ![](/images/image2.png)
    5. Ensure that the Access Token and ExpireEpoch fields are set to Read and Edit
    6. Click Save
2. **Create a single Epic Access record**
    1. Give the Epic Access record a Name/Title of “Epic”.
3. Click on **App Launcher →** Search for “**FlexCards**”
    1. Open the EpicPatientSearchResults Flexcards
    2. Deactivate and re-Activate the FlexCard
    3. Choose the following Publish Options:
        1. App Page
        2. Home Page
        3. Record Page
        4. ![](/images/image3.png)

1. Click on **App Launcher** → Search for “OmniScripts”
    1. Navigate to the recently installed OmniScript in the list view - EHR/EpicPatientSearch
        1. Deactivate the OmniScript
        2. Open up the first element of the OmniScript titled “SetEnv”
        3. Click on the “env” property and change the “Sandbox” value to the name of your Custom Setting above (e.g., “Production”)
2. ![](/images/image4.png)
    1. **Activate** the OmniScript. Be sure to activate the FlexCard in the previous step before re-activating the OmniScript.
    2. For more information regarding activating Omniscripts, please see this article: https://help.salesforce.com/s/articleView?id=sf.os_activating_omniscripts.htm&type=5

1. Click on **App Launcher** → Search for “DataRaptors” 
    1. Open the **DRCreatePersonAccount** DataRaptor
    2. Navigate to the **Formulas** tab
    3. Replace the value in the left hand pane to your org’s **Record Type ID** value for the **Person Account Record Type**

![](/images/image5.png)
1. Add the installed OmniScript to the App Home Page of your choosing. 
    1. Refer to this article for more information regarding adding OmniScripts to a Lightning Page: https://help.salesforce.com/s/articleView?id=sf.os_add_a_standard_omniscript_component_to_a_lighting_page_20263.htm&type=5

* * *

## **Assumptions**

1. A customer has licenses for Health Cloud, and the HINS Managed Package with OmniStudio. These solutions have all been installed and are functional.
2. A customer is assuming Salesforce Lightning Experience — not Classic.
3. Data Model elements that are part of the HINS (Vlocity) Managed package and Health Cloud are all available.
4. The Accelerator uses the Lightning Design System standards and look. Customers may want to apply their own branding which can be achieved.
5. A customer is live on Epic EHR and has the appropriate resources to enable the indicated APIs mentioned above.

* * *

## **Revision History**

* **Revision Short Description (Month Day, Year)**
    * Initial Draft (01/04/2023)
    * Updated docoumentation (1/23/2023)

