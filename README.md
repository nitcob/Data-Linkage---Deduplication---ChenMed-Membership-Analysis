![](RackMultipart20200822-4-tianhc_html_4b393c0cf0d5d759.png)

# **Membership Analysis**

In order to take care of patients at ChenMed, we must be able to track the patients we take care of and the associated Health Plan coverage. Recently, we started to notice that there were duplicates our key tables.

- T\_PATIENT: List of patients who have been seen by a ChenMed provider

- T\_ELIGIBLITY: For patients in the T\_PATIENT table, this table documents the Health Plan the patient is associated with for healthcare coverage. If a patient does not have an associated record in this table, this means the patient does not have current Health Plan coverage, meaning the patient should not be seen by ChenMed.

This should not happen. Here are some examples of duplicates that were found:

T\_PATIENT

| FIRST\_NAME | LAST\_NAME | ACTIVE | DATE\_OF\_BIRTH | ADDRESS | CITY | STATE | ZIP | GENDER |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [DYLHU | PFHOOLRWW | 1 | 03/05/99 | 56:4#UXWODQG#VW | RSD#ORFND | IO | 66387 | M |
| [DYLHU | PFHOOLRWW | 1 | 03/05/99 | 56:4#UXWODQG#VW | RSD#ORFND | IO | 66387 | M |

T\_ELIGIBILITY

| PATIENT\_ID | ELIGIBILITY\_ID | INSURANCE\_CARRIER | FIRST\_NAME | LAST\_NAME | DATE\_OF\_BIRTH | POLICY\_NUMBER | GROUP\_ID |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 34662 | 6071020 | 4 | VDPXHO | OHZLV | 07/06/33 | 536\&lt;35:-34 | 333 |
| 34662 | 6163743 | 4 | VDPXHO | OHZLV | 07/06/33 | 536\&lt;35:-34 | UR-DEG97 |

We suspect two processes that may create these duplicates:

- T\_PATIENT: When a patient shows up in the clinic, if we are not able to find them in the T\_PATIENT table, the front office adds a new record into the T\_PATIENT table.

- T\_ELIGILIBITY: When a patient is added to the T\_PATIENT table, coverage is confirmed, then a record is added to the T\_ELGIIBILITY table using the new PATIENT\_ID. This is done through an automated process. Additionally, each month, we get a membership file from each health plan. These files will include patients who we have seen and patients who have _ **never** _ seen. When we get these files monthly, we have a process to check the patients listed in the membership file against T\_PATIENT and T\_ELIGILIBITY to avoid creating duplicate records. Below are the criteria to determine a match:

                1. Policy Number
                2. First Name + Last Name +DOB
                3. DOB + Phone +Gender
                4. Last Name(first 3 letter) + First Name(first 2 letter) + DOB
                5. Last Name(first 3 letter) + First Name(first 2 letter) + Phone + Gender

As a result, we want you to review the data in the two tables above to clean up the duplicates and develop a process to ensure that new duplicates are not created.

For the interview, we would like you analyze the data to do the following:

- Provide a count of the duplicates and show some specific example of the duplicates that were found
  - Provide the criteria you used to determine which records were considered duplicates
- Provide a set of clean tables (T\_PATIENT &amp; T\_ELIGIBILITY) where the duplicates have been removed
- Provide a recommendation for how we can avoid duplicates in the future
  - What criteria should we use to identify duplicates?

_ **NOTE** _:

For the purposes of this exercise, many of the fields have been encrypted. The data in the fields still represent values that can be queried for duplicates. The same values in a column across 2 records can be considered a duplicate.

Also, social security number cannot be used as a unique identifier because the Health Plans do not provide it to us. So we do not store this value and have to rely on other demographic information to determine if a record is unique.

_ **Data Dictionary:** _

T\_PATIENT

_ **Primary Key** __:_ **PATIENT\_ID**

|     Field                  	|     Description                                                                                                                                                                                              	|
|----------------------------	|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------	|
|     PATIENT_ID             	|     Unique   Identifier for each patient.    Automatically created when a new record is added                                                                                                                	|
|     OFFICE_ID              	|     Office   within ChenMed that a patient is assigned to                                                                                                                                                    	|
|     ACTIVE                 	|     Indicates   whether a record is concerned active.    1 = ACTIVE: 0 = INACTIVE.  When   a duplicate is found in T_PATIENT, one of the records should have ACTIVE = 0.  The record is not deleted          	|
|     FIRST_NAME             	|     First   name of the patient                                                                                                                                                                              	|
|     LAST_NAME              	|     Last   Name of the patient                                                                                                                                                                               	|
|     DATE_OF_BIRTH          	|     Date   the patient was born                                                                                                                                                                              	|
|     ADDRESS                	|     Street   Address for the patient                                                                                                                                                                         	|
|     CITY                   	|     City   the patient lives in                                                                                                                                                                              	|
|     STATE                  	|     State   the patient lives in                                                                                                                                                                             	|
|     ZIP                    	|     Zip   code of the patient                                                                                                                                                                                	|
|     GENDER                 	|     Patient   Gender                                                                                                                                                                                         	|

T\_ELIGIBLIITY

_ **Primary Key** __:_ **ELIGIBILITY\_ID**

**Notes:** A PATIENT\_ID can have multiple records in this table if they have different INSURANCE\_ID values. This means that the patient has two health plans.

|     Field                           	|     Description                                                                                             	|
|-------------------------------------	|-------------------------------------------------------------------------------------------------------------	|
|     ELIGILIBILITY_ID                	|     Unique   Identifier for each eligibility record.    Automatically created when a new record is added    	|
|     PATIENT_ID                      	|     Unique   Identifier for each patient.                                                                   	|
|     INSURANCE_ID                    	|     ID for   insurance / Health Plan associated with the patient                                            	|
|     FIRST_NAME                      	|     First   name of the patient                                                                             	|
|     LAST_NAME                       	|     Last   Name of the patient                                                                              	|
|     DATE_OF_BIRTH                   	|     Date   the patient was born                                                                             	|
|     ADDRESS                         	|     Street   Address for the patient                                                                        	|
|     CITY                            	|     City   the patient lives in                                                                             	|
|     STATE                           	|     State   the patient lives in                                                                            	|
|     ZIP                             	|     Zip   code of the patient                                                                               	|
|     PHONE                           	|     Phone   Number for the patient                                                                          	|
|     POLICY_NUMBER                   	|     Policy   number associated with the patient based on the insurance / Health Plan                        	|
|     GROUP_ID                        	|     Group   ID associated with the patient based on the insurance / Health Plan                             	|
|     GENDER                          	|     Patient   Gender                                                                                        	|
|     OFFICE_ID                       	|     Office   within ChenMed that a patient is assigned to                                                   	|
