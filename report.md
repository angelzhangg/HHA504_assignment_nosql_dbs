## Objective
The objective of this assignment is to introduce you to managed database services in Azure and Google Cloud Platform (GCP). You will learn how to start, stop, and monitor database-related services, including BigQuery and MySQL.

## Fake dataset to use: 

- data set url: [https://raw.githubusercontent.com/hantswilliams/HHA-504-2024/refs/heads/main/other/module8/module8_nosql_hw.csv](https://raw.githubusercontent.com/hantswilliams/HHA-504-2024/refs/heads/main/other/module8/module8_nosql_hw.csv)

```csv
PatientID,Name,Age,Gender,DiagnosisCode,VisitDate,Hospital,TreatmentPlan,FollowUpDate
1,John Doe,45,M,M54.5,2024-09-10,Stony Brook Hospital,Physical Therapy,2024-09-20
2,Jane Smith,29,F,E11.9,2024-08-15,Stony Brook Hospital,Insulin,2024-09-15
3,Bob Johnson,65,M,I10,2024-10-01,Long Island Clinic,Hypertension Medication,2024-11-01
4,Alice Williams,50,F,J45.909,2024-07-22,Southampton Hospital,Bronchodilators,2024-08-22
5,Michael Brown,37,M,G43.909,2024-06-12,Stony Brook Hospital,Triptans,
6,Susan Davis,54,F,I25.10,2024-05-10,Stony Brook Hospital,Statins,2024-06-10
```

- Dataset Explanation
  - *PatientID*: Unique identifier for each patient (integer).
  - *Name*: Patient's full name (string).
  - *Age*: Age of the patient (integer).
   - *Gender*: Gender of the patient (string: M or F).
  - *DiagnosisCode*: ICD-10 code representing the patient’s diagnosis (string).
  - *VisitDate*: Date of the hospital visit (YYYY-MM-DD format).
  - *Hospital*: Name of the hospital or clinic where the visit took place (string).
  - *TreatmentPlan*: The prescribed treatment plan for the patient (string).
  - *FollowUpDate*: Date for a follow-up visit, if applicable (YYYY-MM-DD format).

## Instructions

### 1. Start and Configure Databases
- **Google BigQuery (GCP):**
  - Navigate to BigQuery in the Google Cloud Console.
  - Use your student account project to create a new dataset in BigQuery.
  - Upload the provided healthcare dataset (CSV) into a table within your dataset.
  - Note the connection details and the query editor interface.
    
![bigQuery](https://github.com/user-attachments/assets/b83a5adb-0802-4a37-8e43-3c8952b40eb5)

- **MongoDB Atlas (Cloud):**
  - Go to [MongoDB Atlas](https://www.mongodb.com/cloud/atlas) and register for the free tier using your Stony Brook email.
  - Create a new database instance and configure it with basic settings.
  - Insert the provided healthcare dataset into a collection, ensuring each row is converted to a JSON document.
  - Document the steps and connection details.
  - **Hints**:
    - Each row in the CSV file corresponds to a document in MongoDB.
    - You’ll need to convert the CSV into JSON format, which could look like this for one patient:
      ```json
      {
        "PatientID": 1,
         "Name": "John Doe",
        "Age": 45,
        "Gender": "M",
        "DiagnosisCode": "M54.5",
        "VisitDate": "2024-09-10",
        "Hospital": "Stony Brook Hospital",
        "TreatmentPlan": "Physical Therapy",
        "FollowUpDate": "2024-09-20"
      }
      ```
    - You can use python to help convert it into json-like format: 
      - Example:
        ```python
        # Convert the DataFrame to a list of dictionaries (JSON-like)
        data = df.to_dict(orient='records')
        ```
      - Then using the `pymongo` library to insert the data into MongoDB.
        ```python
        # Insert the data into the MongoDB collection
        collection.insert_many(data)
        ```
![mangodb](https://github.com/user-attachments/assets/b6e29468-bd50-4e72-91ed-2f085726f763)
mongodb+srv://zhang:<db_password>@angela-mango.zr2eo.mongodb.net/



- **Redis Cloud:**
  - Go to [Redis Cloud](https://redis.io/cloud/) and sign up for a free tier account using your Stony Brook email.
  - Set up a new Redis database instance.
  - Use `PatientID` as the key and the rest of the patient data as the value (either as a serialized JSON string or separate fields).
   - **Hints**:
    - Redis is a key-value store, so you'll need to treat the `PatientID` as the unique key, with the patient data being the value.
    - Example in python, using the `redis` library: 
      ```python

      df = pd.read_csv('file.csv')

      # Connect to Redis
      r = redis.StrictRedis(
          host='your_redis_host',
          port='your_redis_port',
          password='your_redis_password',
          decode_responses=True
      )

      for _, row in df.iterrows():
          patient_data = row.to_dict()
          r.set(patient_data['PatientID'], json.dumps(patient_data))
        ```

### 2. Explore BigQuery (GCP)
- **BigQuery:**
  - In the Google Cloud Console, run a simple SQL query against the dataset you uploaded:

![bigQuery](https://github.com/user-attachments/assets/eb484617-b416-4c95-8ed3-ee4e18ddc197)
![BILLING](https://github.com/user-attachments/assets/87743f57-20b8-4219-8ed5-bc642d573f29)
  - WAS BILLED 0 BYTES
### 3. Modify and Explore the Data in MongoDB Atlas and Redis Cloud
- **MongoDB Atlas**:
  - Insert the dataset into MongoDB Atlas, converting each row into a JSON-like document.
  - Ensure fields like PatientID and VisitDate are treated appropriately (i.e., unique identifiers and date types).
  - Run a simple query to retrieve patient data based on a condition (e.g., `Age > 40`).

- **Redis Cloud**:
  - Insert key-value pairs where the PatientID is the key, and the rest of the patient data is the value.  
  - For example, retrieve the data for `PatientID=1`, then update the `TreatmentPlan` value.
  - Explore Redis's capabilities to update and query the dataset, e.g., retrieving all data for `PatientID`=1.


### 4. Describe Your Experience
- For each of the three services (BigQuery, MongoDB Atlas, Redis Cloud), document your experience creating and working with the healthcare dataset:
  - Describe the setup process and any configuration steps.
  - Share your reflections on the interface and usability of each platform.
    
All 3 services have their pros and cons. BigQuery offers a lot of other services so although BigQuery was pretty simple and easy to use, I think it was still more complicated then the other two services. MongoDB is pretty simple as well however I ran into a problem where I was unable to upload the data file so I was unable to query anything. Lastly, Redis Cloud is good for building real time data so Its another create tool that works similary as the other two. 
