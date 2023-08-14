## Python Interview Test - Updraft

### Preface

Updraft is a fin-tech startup headquartered in London with a mission to help everyone in the world to get out of the bad debt cycle. With an average interest rate (APR) of 40% of any unsecured loan such as credit cards in the UK, millions of people struggle to fulfill their financial goals and live a stress free life. Instead, these pocket heavy loans make sure that the consumers always stay in the debt and pay interests.

We, at Updraft, have come up with a revolutionary lending product that helps users get rid of their bad debt the next day and help them get out of all debts by providing a much lower and reasonable interest rate loan. With over £200 million funding, 400,000+ users and the best brains working together, we think we are in the position to solve this problem not just in the UK but globally.

* Pick one of the two tasks described below. (You are free to attempt and submit both)
* Output should be a folder with your full name in snake_case. Email it to div@updraft.com

### Task 1 - Monitoring System (1-2 hours)

An essential part of a lending company and product is **collection**. When we lend some money to a user, we need to make sure that the user is paying their monthly repayments as per the agreement. The best way to ensure the smooth running of collections is to automate the actual transaction of deducting the amount from the user's bank account and transferring it to Updraft’s single account responsible for collection. In the UK, this automation is enabled by a concept called Direct Debit using which Updraft can charge the user as and when required (as per the loan agreement). 

Updraft has partnered with a company called Modulr and using their API we have built an automated collections system. On behalf of us, Modulr system, for all our users with an active loan, collects the monthly repayment amount (every month) and sends it to Updraft's account. Your task is to make a simple monitoring system that alerts when this automated system doesn’t behave the way it should. Assume that this monitoring system runs weekly and evaluates any issues for the last 7 days before sending alerts accordingly. Attached are two CSVs that will help you build and test this system - 

1. modulr_transactions_010123_070123.csv  
2. updraft_db_010123_070123.csv. 

The first csv is the record of all successful collections done by Modulr and the second csv is an extract of one of our database tables called Payment. This table keeps track of all collection activities and has following statuses for each collection - 

* Pending - The collection was requested but is still pending on Modulr’s end.
* Processed - The collection was successfully done by Modulr and Updraft has that amount in its collection account.
* Failed - The collection that was requested failed because of one of many reasons such as insufficient funds in the user's bank account, technical issues etc.

The monitoring system should be capable of detecting all the different types of discrepancies in the two datasets and print their count with the respective payment details. Hint - One type of discrepancy is some loan payments are missing in the updraft db csv but are present in the modulr csv. This is an issue because modulr csv has a list of all successfully collected payments, so updraft db should have the record of all rows in modulr csv with the status set to processed. 

##### Instructions

1. Create a (python) directory called monitoring_system.
2. Create a folder named csv_files and put the two CSVs in there.
3. Create a file called monitor.py that contains a class Monitor and takes in the two datasets as lists (of dict or of tuple, up to you) called - modulr_data, updraft_data. 
4. Write different functions for (at least 4) different types of discrepancies. Each function inside this class should return the count of payments getting triggered by that check and should return relevant details of those payments.
5. Create a pytest.py file that tests the different functions of the class created above.
6. Create a main.py file that imports the two CSVs and produces (prints) the relevant output.

##### Points to note 

- Make sure we can run main.py and see the output as instructed.
- Focus on code quality, tests and edge cases.

### Task 2 - API Integration (1-2 hours)

Imagine Updraft has only 400 users (and not 400,000+ users) for simplicty. You are asked to build a collection system for our product Updraft Credit using a third-party api integration -  GoCardless API. This collection system will have two components - 

1. A component that schedules a collection/ payment for a specific monthly repayment amount of an active loan.
2. A component that runs periodically and updates the status of all pending collections.

Note that this system runs everyday and the first component only needs to run for the loans that are due a collection today (using date_of_collection). For eg -  If a date_of_collection is 20 and today is 20-08-2023, then only we will schedule a collection, provided a loan is active. Definition of an active loan is provided in the instructions below. 

For both the above components, we will be using these endpoints: Create a payment, Get a payment respectively. Attached are two CSVs are useful for building and testing the system - 

- loans.csv - Use this for the first component
- payments.csv - Use this for the second component

##### Instructions

1. Create a (python) directory called collection_system.
2. Put the loans.csv and payments.csv inside the directory.
3. Create a file called collection.py that contains a class Collection 
4. Write a function in this class that takes in a loan (row as a dict or tuple, up to you), the start_date and number_of _months of this loan to check if it is active or not. If for eg. a loan started 3 years ago and had only 12 months in the agreement, then that loan is not active. 
5. Write another function that takes in a loan, monthly_amount and date_of_collection to check if a schedule is required today and then schedules a payment using the Gocardless API. meta_data field can be left as an empty dict in the request to modulr. Use loan_id as the reference. Usually, an output of this function will be a unique payment_id returned by GoCardless that will be stored in our database. To avoid any DB interaction in this task, we will skip that part.
6. Write another function that takes in a payment (row as a dict or tuple, up to you). Using payment_id reach out to this endpoint and get the latest status. Usually, output of this function will be a database entry with an updated status for the corresponding payment_ids (let’s skip it)
7. Create a pytest.py file that tests the 3 functions and their different scenarios (if any).
8. Create a main.py file that imports the two CSVs and runs the two components subsequently (one by one) for each row in the corresponding CSVs.

##### Points to note 

- Focus on code quality, tests, exception handling and edge cases
- Focus on api integration bits and make sure the api integration is tested using mock/ patch/ etc
