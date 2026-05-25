# Greek ERGANI XML Data Validation Tool

## Summary:
A Python **data validation** tool designed to audit standardized XML files generated for the Greek Ministry of Labor’s registry system (ERGANI) and return any lines with missing key data.

This specific Python tool focuses entirely on XML files used to upload the daily work schedule of the employees to the ERGANI system.

By default, it uses the file 'Work Schedule 22-23.05.2026.xml' included in this folder. This file includes changes for 100 employees, for two days (22 & 23.05.2026).

## The Case:
With the implementation of the Digital Work Card, companies are required to upload the work schedule of their employees to the Greek Ministry of Labor’s registry system (ERGANI). In case of changes in the initially declared work schedule, they must proceed with a "Daily Work Schedule" submission.

For large employers or for firms who offer payroll outsourcing services to clients, this is a high-volume daily procedure. These companies often genarate XML files to automatically declare changes for many employees simultaneously. To successfully pass the ERGANI portal checks and be uploaded to the system, these files must contain non-empty tags for:
1. AFM (Tax Identification Number),
2. Name and Surname,
3. Date of change,
4. Type of work ('ΕΡΓ' refers to work from the company premises, 'ΤΗΛ' refers to remote work and 'ΑΝ' refers to rest day)
5. Start and End Time of the new work schedule

## The Problem:
Data generation programs frequently output faulty XML files, usually containing missing key fields. When such errors occur, the submission fails entirely. Due to technical limitations, the official ERGANI portal cannot display a complete list of all errors within a file. Manualy parsing hundreds or even thousands of lines of XML code to locate empty tags can be time-consuming and therefore lead to:
* Increased workload,
* Missed submission deadlines,
* Compliance implications for the company (e.g. fines).

## The Solution:
By scanning the entire XML file, this Python tool logs every single missing data element alongside its exact physical line number. This significantly reduces the time needed to repair the XML, since it instantly provide a list of every empty tag in the file.

## Technical Features:
Different tools were utilised during the making of this Python program, including:

* **urllib (Network & Data Retrieval):** Used to establish a connection with remote servers and stream the raw XML data directly into the program via an HTTP request. This eliminates the need to download the file manually beforehand.

* **Regular Expressions:** Utilized to dynamically add an attribute with the line number in all the key tags that will be after checked for missing data. It is then used to retrieve the line of the error.

* **Lists:** Combined with the Regular Expressions to convert the raw web file into a sequential Python list of plain text strings (`.splitlines()`). This allowed the modification of the file (addind the line number attributes). After the modification, the `.join()` structure was used to reassemble the list back into a single text block so it could be successfully parsed into the final XML tree.

* **ElementTree(Data Parsing & Extraction):** Used to parse the modified XML text into an in-memory element tree. It handles looking up specific data structures (like `.findall()`) to isolate tags and contribute to error checking.

* **Python Dictionaries:** Utilized to convert XML tags (e.g. 'f_onoma') to user friendly terms (e.g. 'name') for improved output display purposes.

* **Loops and Conditional Statements:** Implemented to scan the dataset and flag missing or invalid data errors.
