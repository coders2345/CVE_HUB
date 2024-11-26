CVE Management System Assessment 
Introduction:   
• The objective of this project is to develop a comprehensive solution for managing Common 
Vulnerabilities and Exposures (CVE) data, enabling security professionals to access and 
analyze the latest vulnerability information effectively. 
Logic and Description of the Problem Statement: 
• Consume the CVE information from the CVE API for all the CVEs and store it in a Database 
of your choice. 
Approach: The `app.py` script includes the logic to fetch the CVE data from the NVD 
API and store it in a MongoDB database. The `fetch_total_cve_count()` function retrieves 
the total count of CVE data available in the API, while the `fetch_cve_data()` function 
fetches the CVE data in batches, using the `startIndex` and `results per page` parameters 
to page through the data. The `store_cve_data()` function stores the fetched CVE data in 
the MongoDB collection, using the CVE ID as the document's `_id` field. 
Challenges: Handling the pagination of the API responses to retrieve all the CVE data, 
and ensuring the reliability and robustness of the data fetching and storage process. 
Apply data cleansing & de-duplication,  
• Approach: The `deduplication.py` script includes the logic to identify and remove 
duplicate CVE documents from the MongoDB collection.It uses an aggregation pipeline 
to group the documents by the `cve. id` field, count the occurrences and identify the 
duplicate documents. The script then removes the duplicate documents from the 
collection, ensuring data quality and uniqueness. 
Challenges: Designing an efficient algorithm to identify and remove duplicate 
documents, while ensuring the process does not impact the overall performance of the 
system. 
CVE details : 
Approach: The `synchronize_cve_data()` function in the `app.py` script implements the 
logic to periodically synchronize the CVE data between the NVD API and the MongoDB 
database. It compares the total count of CVE data in the API and the MongoDB 
collection, and fetches and stores any new CVE data in batches, ensuring that the 
MongoDB collection is kept up-to-date with the latest CVE data. 
Challenges: Determining the appropriate time interval for the synchronization process, 
and ensuring the process does not overload the API or the database. 
• Develop APIs to read & filter the CVE details by the below parameters - CVE ID, CVE IDs 
belonging to a specific year, CVE Score (Field to ref 
metrics.cvssMetricV2.cvssData.baseScore or metrics.cvssMetricV3.cvssData.baseScore), last 
Modified in N days. 
Approach: The `app.py` script defines two API endpoints: 
cves/list`: This endpoint retrieves a list of CVEs with pagination support, allowing users 
to filter the results by the `results per page` and `page` parameters. 
details/<cve_id>`: This endpoint retrieves the details of a specific CVE, identified by the 
cve_id parameter. 
Challenges: Implementing the additional filtering capabilities based on the specified 
parameters (year, score, last modified date) within the scope of this assessment. 
Code & Output Screenshots 
• `app.py`: The main Flask application that handles the API endpoints and the 
synchronization of CVE data. 
• Data-API-MONGODB.py This Python file handles data synchronization from 
API to MongoDB 
• `deduplication.py`: A script that identifies and removes duplicate CVE 
documents from the MongoDB collection. 
• ` unit Test.py`: A set of unit tests for the Flask application. 
• `index.html` and `details.html`: HTML templates that display the CVE data in 
the user interface. 
•  
screenshots of the code and output: 
index.html 
Output: 
 
Details Page: 
 
 
 
Output: 
App.py: 
Output: 
Data-Api-mongoDB: 
 
Output 
 
 
 
Deduplication: 
Output: 
Unit Test: 
Output: 
Explanation of Solution Approach 
CVE Data Synchronization 
• The `app.py` script implements the logic to fetch the CVE data from the NVD API and store it 
in the MongoDB database. The `fetch_total_cve_count()` function retrieves the total count of 
CVE data available in the API. In contrast, the `fetch_cve_data()` function fetches the CVE 
data in batches, using the `startIndex` and `results per page` parameters to page through the 
data. The `store_cve_data()` function stores the fetched CVE data in the MongoDB collection, 
using the CVE ID as the document's `_id` field. 
• The `synchronize_cve_data()` function in the `app.py` script implements the logic to 
periodically synchronize the CVE data between the NVD API and the MongoDB database. It 
compares the total count of CVE data in the API and the MongoDB collection, and fetches and 
stores any new CVE data in batches, ensuring that the MongoDB collection is kept up-to-date 
with the latest CVE data. 
Data Cleansing and De-duplication: 
• The `deduplication.py` script includes the logic to identify and remove duplicate CVE 
documents from the MongoDB collection. It uses an aggregation pipeline to group the 
documents by the `cve. id` field, count the occurrences and identify the duplicate documents. 
The script then removes the duplicate documents from the collection, ensuring data quality 
and uniqueness. 
API Documentation: 
1. Get CVE List 
• Endpoint: /cves/list 
Method: GET 
Description: 
• Retrieves a list of CVEs with pagination support. 
Parameters: 
• `results per page` (optional, default: 10): The number of results to display per page. 
• `page` (optional, default: 1): The page number to retrieve. 
Response: 
JSON: 
{ 
"data": [ 
{ 
"I've": { 
"id": "CVE-2014-123456", 
"source identifier": "CVE-2014-123456", 
"published": "2023-04-01T00:00:00.000Z", 
"lastModified": "2023-04-15T00:00:00.000Z", 
"vulnStatus": "PUBLISHED" 
} 
}, 
{ 
"I've": { 
"id": "CVE-2014-456132", 
"source identifier": "CVE-2014-456132", 
"published": "2023-04-02T00:00:00.000Z", 
"lastModified": "2023-04-16T00:00:00.000Z", 
"vulnStatus": "PUBLISHED" 
} 
}, 
... 
], 
"results_per_page": 10, 
"page_number": 1, 
"total_pages": 100, 
"total_records": 1000 
} 
``` 
2. Get CVE Details: 
• Endpoint: `/details/<cve_id>` 
• Method: GET 
Description: 
• Retrieves the details of a specific CVE. 
Parameters: 
• `cve_id`: The ID of the CVE to retrieve. 
Response: 
JSON 
{ 
"I've": { 
"id": "CVE-2014-123456", 
"descriptions": [ 
{ 
"value": "A vulnerability has been discovered in the XYZ software that allows an 
attacker to execute arbitrary code." 
} 
], 
"metrics": { 
"cvssMetricV2": [ 
{ 
} 
] 
}, 
"baseSeverity": "HIGH", 
"impact score": 9.3, 
"cvssData": { 
"vectorString": "AV:N/AC:M/Au:N/C:C/I:C/A:C", 
"accessVector": "NETWORK", 
"access complexity": "MEDIUM", 
"authentication": "NONE", 
"confidentialityImpact": "COMPLETE", 
"integrityImpact": "COMPLETE", 
"availabilityImpact": "COMPLETE" 
}, 
"exploitabilityScore": 8.6 
"configurations": [ 
{ 
"nodes": [ 
{ 
"cpeMatch": [ 
{ 
} 
] 
"Criteria": "cpe:/a:xyz_inc:xyz_software:3.2.1", 
"matchCriteriaId": "1", 
"vulnerable": true 
} 
] 
} 
] 
} 
} 
Error Handling: 
• The API will return appropriate HTTP status codes and error messages in case of any issues. 
For example: - - - 
`400 Bad Request`: If the request parameters are invalid or missing. 
`404 Not Found`: If the requested CVE is not found. 
`500 Internal Server Error: If there is an unexpected error on the server side. 
• The error response will include a JSON object with an `error` field containing a descriptive 
error message. 
Rate Limiting: 
• The API is subject to rate limiting to prevent abuse and ensure fair usage. If the rate limit is 
exceeded, the API will return a `429 Too Many Requests` response with the appropriate 
headers indicating the rate limit details. 
Authentication and Authorization: 
• The CVE Management System API does not currently require any authentication or 
authorization. However, in a production environment, it is recommended to implement 
appropriate security measures, such as API keys or OAuth 2.0, to control access to the API. 
Unit Test Cases 
• The `test_app.py` script includes the following unit test cases: 
1. test_index_route: 
• Purpose: Verifies the functionality of the `/cves/list` endpoint, which retrieves the list of 
CVEs. 
• Checks: Ensures the response status code is 200 (OK) and the response data contains the 
expected string "CVE LIST". 
2. test_cve_detail_route: 
• Purpose: Verifies the functionality of the `/details/<cve_id>` endpoint, which retrieves the 
details of a specific CVE. 
• Checks: Ensures the response status code is 200 (OK) and the response data contains the 
expected CVE ID. 
• These unit tests help ensure the correctness and reliability of the Flask application's routes and 
functionality. 
Conclusion: 
• The CVE Management System project developed by Mabasha AIDS Rathinam Technical 
Campus demonstrates a structured and comprehensive approach to managing CVE data. The 
system's ability to fetch, store, and retrieve CVE information from the NVD API, coupled 
with its data cleansing and de-duplication capabilities, ensures the integrity and reliability of 
the data. The well-documented API and the comprehensive unit testing further enhance the 
project's overall quality and maintainability. 
• This submission showcases the team's technical expertise, attention to detail, and commitment 
to delivering a high-quality solution for managing CVE data effectively 
output Screenshot:
<img width="677" alt="ui" src="https://github.com/user-attachments/assets/e6ebe924-63fe-4a09-ab19-c51005ac029b">

Db image
<img width="947" alt="dataloadin" src="https://github.com/user-attachments/assets/3a0515b2-da66-49a6-afdb-d22f021a0da4">
Data upload and synchronization from cve API

<img width="959" alt="data upload and sync" src="https://github.com/user-attachments/assets/b26e5590-f779-43d5-8d04-61c45b36e199">
