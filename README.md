# Data Analysis project- Using SQL to Clean and Analyse Data.

Through this data analysis project, I used SQL to clean and analyze call center data from ‘Real World Fake Data’, and leveraged Power BI to create impactful visualizations, aiming to enhance performance and elevate customer experience.


## 
 
![calls6](https://github.com/IhebBA/Call-Center/assets/72503027/e2563239-2c54-49c9-a754-978189356ef7)  
photo by [**Lexica.art**](https://lexica.art/prompt/30fe6c22-34c9-4fde-b3f4-ff4222c59f8c)

Today, we'll walk through the process of importing CSV files into a PostgreSQL database, cleaning the data, analyzing it, and finally visualizing it using Power BI. Let's begin!


## Step 1: Import our data.

So, the call-center dataset comprises over 32,900 records, encompassing calls made to various call centers. It includes details such as call ID, call duration in minutes, caller names, satisfaction scores, sentiment, reason, response time, and other attributes that will become clearer as we delve deeper.

Anyway, I've downloaded our dataset, and now it's time for me to import it into a PostgreSQL database.

Link Dataset : [**_Call Center Dataset_**](https://data.world/markbradbourne/rwfd-real-world-fake-data/workspace/file?filename=Call+Center.csv)

### Create Database : 
To begin, I set up a database where I'll import our data. I opted to create a new one rather than using an existing database.

Here's a snapshot of the CSV file I'm working with:

![dataset in csv format](https://github.com/IhebBA/Call-Center/assets/72503027/b96f7212-addc-4354-ab89-4e08d07546e8)

Here's a snapshot of the file in Excel :

![dataset in exel format ](https://github.com/IhebBA/Call-Center/assets/72503027/56eb2384-315e-42b6-bb1b-abe676d07f29)

### Create Table 'calls':

Next, I created a table named 'calls' to accommodate our data. This table is designed to match the structure of our dataset. Each column in the CSV file corresponds to a column in the 'calls' table, ensuring compatibility with the data types.

Here's a visual representation of the table:

![create_table](https://github.com/IhebBA/Call-Center/assets/72503027/eba65cdd-e08f-4cbd-a052-48a7c4ac7113)

### Import CSV File: 

Moving forward, within the PostgreSQL database interface, I navigated to the database in use and located the 'calls' table that I just created. With a right-click on the table, I selected 'Import/Export' to open a window where I could browse and select the CSV file for import.

![import csv to table](https://github.com/IhebBA/Call-Center/assets/72503027/8f5d4f59-5244-40fc-9a7b-7045f013894f)

After selecting the CSV file, the next step involves specifying the import settings. I ensured that the encoding was set to UTF-8 for proper character representation. Additionally, I selected 'Yes' for the header option to recognize the first row as column headers. For the delimiter, I chose ',' as the separator to delineate fields properly. Finally, after configuring these settings, I pressed 'OK' to proceed with the import.

![import csv to table 2](https://github.com/IhebBA/Call-Center/assets/72503027/271de050-5a9f-4be9-9165-aee18cc01281)

### Reviewing the Table 

To ensure a seamless process, let's review the table to confirm that everything is in order.

![table etat initial](https://github.com/IhebBA/Call-Center/assets/72503027/b9d35a53-f658-4827-ab8e-69e321b80507)

## Step 2: Clean the data.

- When I created the table, you might have wondered, "Why is the call_timestamp, originally a date, using a char datatype?" Let me explain the process: Initially, the call timestamp in the CSV file was in the format: mm-dd-YYYY. This translates to two digits for the month, two digits for the day, and four digits for the year. However, PostgreSQL's default format is yyyy-MM-dd, which meant it needed adjustment. To address this, I converted it into a string and later rectified it in PostgreSQL. First, I had to transform the timestamp into the yyyy-MM-dd format, and then convert the column to a date type instead of a string.

- I have noticed that all the empty values in the original CSV dataset were present in the csat_score column (ah, I forgot to mention, it stands for customer satisfaction score). I chose to retain these null values for the time being. However, in the future, I might consider updating them either by the median or by assigning a value of 1. It's worth noting that 1 is selected as the minimum value in this context.

1- SQL query image for the first issue : 

![1ere clean call_timestamp](https://github.com/IhebBA/Call-Center/assets/72503027/a0368f30-e762-4c7b-a8f6-d4bc659f25e0)

I finally managed to convert the call_timestamp to date format.

The table result after cleaning :

![tout le table ba3ed clean 1](https://github.com/IhebBA/Call-Center/assets/72503027/27617ede-88f5-4b68-b92e-8aa591c0b36b)
![tout le table ba3ed clean 1 image 2](https://github.com/IhebBA/Call-Center/assets/72503027/b0261106-3175-49d7-9764-fb4f222edbe6)

## Step 3: Exploratory Data Analysis (EDA).

The first image shows the number of rows in the 'calls' table

![eda 1](https://github.com/IhebBA/Call-Center/assets/72503027/a9fd0376-f313-4060-a599-516b771fc870)

The second image displays the number of columns in the 'calls' table

![eda 2](https://github.com/IhebBA/Call-Center/assets/72503027/e7f77022-a7b1-4bf7-ae9c-8a345d1c403c)

In the following query, I've displayed the call centers : 

![eda 4](https://github.com/IhebBA/Call-Center/assets/72503027/4ecdfc6b-63c5-49b0-ac6a-e31d47377e54)

In the following query, I displayed the reasons from the call center calls : 

![eda 5](https://github.com/IhebBA/Call-Center/assets/72503027/b9224834-d78a-4f88-bef1-6274c9f3385a)

Now : customer sentiments from the calls : 

![eda 3](https://github.com/IhebBA/Call-Center/assets/72503027/9ea2755b-f2ec-4205-b7cb-896fc65b6d3b)

Next, I displayed the 'channel' and 'response_time' : 

![eda 6](https://github.com/IhebBA/Call-Center/assets/72503027/a9a67885-24d0-4b08-ba93-a89206d8a901)

![eda 7](https://github.com/IhebBA/Call-Center/assets/72503027/c15dbebf-2a9b-49da-a93f-fa66b37c7d82)

Sentiment Distribution : Now for the next query: it collects sentiment data from the 'calls' table, counts the occurrences of each sentiment, and calculates the percentage corresponding to each sentiment.

![eda 9 count and pourcentage sentiment](https://github.com/IhebBA/Call-Center/assets/72503027/67e83ff7-68c0-44b9-8620-dbf4ea7e8b08)

For the following queries, a similar process is followed : 

Call Center Distribution : 

![eda 10 count and pourcentage call_center](https://github.com/IhebBA/Call-Center/assets/72503027/a2658b13-6554-4524-9c62-cc4fae5ff625)

Reason Distribution  :  

![eda 8 count and pourcentage reason](https://github.com/IhebBA/Call-Center/assets/72503027/cca13ae2-7ed8-41b1-abdb-785098ade188)

Response_time Distribution : 

![eda 11 count and porcentage response_time](https://github.com/IhebBA/Call-Center/assets/72503027/0bda893c-f3c0-4026-8f5b-c89d5d963935)

Now, The day with the highest number of calls : 

![eda 12 day have most call](https://github.com/IhebBA/Call-Center/assets/72503027/15a55486-6b2d-4a10-b998-bd53854c52ee)

The state with the highest number of calls : 

![eda 15](https://github.com/IhebBA/Call-Center/assets/72503027/c9350228-0c1e-48d8-8c91-ccaff0f6a7dc)

Calculate the minimum, maximum, and average customer satisfaction scores (csat_score) : 
![eda 13](https://github.com/IhebBA/Call-Center/assets/72503027/24eca1fd-6877-4b99-845e-de9cd110aa64)

Calculate the minimum, maximum, and average call duration in minutes : 
![eda 14](https://github.com/IhebBA/Call-Center/assets/72503027/d859d04b-e108-453d-802b-31b0a0316429)

Relationship between Sentiment and State : 
![eda 16](https://github.com/IhebBA/Call-Center/assets/72503027/867a671f-5a0a-46f1-a113-f92c7b8e89be)

Relationship between Reason and State : 
![eda 17](https://github.com/IhebBA/Call-Center/assets/72503027/31bf9e4c-2640-40ed-aa88-3d16fe8c2942)

#### That's all for the SQL queries. Now, here's the dashboard I've created using Power BI :  [**_Link_**]()

![dashbord](https://github.com/IhebBA/Call-Center/assets/72503027/dbe22623-a3c8-4133-80d6-39e276e97244)
