1,Created the needed **resources**
     * Azure Data Lake Gen2
     * Azure SQL DB ( to store EMR data)
        Data was coming from two different data bases
     * Azure Databricks workspace ( for audit table processing )

2, Created **Generic Linked Service** for Azure SQL ( because multiple databases are present ), ADLS , Azure Databricks.
      A Linked Service defines secure connection details in ADF, and by parameterizing it, we can create a generic SQL linked service that      dynamically connects to different databases using **@linkedService().db_name** and added db_name in parameters.
<img width="1314" height="176" alt="image" src="https://github.com/user-attachments/assets/6aaa601a-d5ca-4faf-9cb9-814abcdbd054" />

3, Created **Generic Datasets** for uploading csv ,parquet files to ADLS GEN2 , dataset for azure sql and dataset for azure databricks
<img width="1092" height="394" alt="image" src="https://github.com/user-attachments/assets/169c979a-5848-4f25-a340-1ca19db56311" />


      
      
