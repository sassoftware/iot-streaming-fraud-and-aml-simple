# Streaming Fraud and AML simple example

## Overview

This is a very small streaming demo built to showcase pattern matching, temporal sliding windows and data aggregation for simple AML / fraud alerts on transactions in a streaming banking use case.  There is no advanced analytics in here, it is all simple data comparison and analysis with rules, predominently in sliding, temporal windows.

> Retention is required for much of the example, so please be aware that the concepts shown very simply here may need to scale with support of a  fast in-memory database solution in support, as the retention data could grow large. 

If you want to take advantage of the SMTP notifications then use a third-party SMTP service, there are several free ones, I used (sendinblue.com  and my personal gmail email account)

>****************************************************************************************************************************************************
>Of course the data is all fake, no intention to reference real people or situations, but please ensure you have checked it and if needs be change the names prior to use in your demo for sensitivity of any similar names to real people.
>****************************************************************************************************************************************************

## Prerequisites


1. load the project xml file into ESP Studio
**** Edit the project name to suit your needs and edit and change the SMTP server, user, email addresses and passwords accordingly ****

2. load the prebuilt streamviewer dashboard into streamviewer (optional)
**** Check the project name for ESP is the same, you can skip this part as you do not have to use streamviewer, you can use esp-connect or 3rd party charting tools ****

## Running 

To perform the demo, start the project running, (I used test mode in ESP Studio), then inject the data using the Publish button:

- For everything to work correctly always load the restrictive_list.csv data into the restriced_list window first

- Load the input_transactions.csv into the transactions_streaming_in window, I would give this a second or even two of 'Maximum event rate to be maintained' 

- Make sure you select the right date format which is :- %d/%m/%Y %H:%M:%S (at the time of writing this, it is the 3rd date format from the top on the publish data from File, date drop down list, in ESP Studio test screen)

 
There are 3 simple rules that fire in the demo, (remember that the data takes place over 5 days, so the rules will fire based on transaction times not on ESP data ingestion times)

- Rule 1 - Consecutive cash-out transactions for an account
o	Transactions 0000001 to 0000004
o	There are 4 cash-out transaction done by the same MSISDN (phone number of the customer) in less than 10 minutes
o	An aggregation of transaction done in a 10 minutes window must be implemented
o	If more 4 or more transactions are done in this windows ESP should issue a “decline message” (you can store such results in a DB or issue a final message to simulate the answer to the core system)

- Rule 2 - Many-to-One fund transfers (per various pointers like subscriber’s name or MSISDN);
o	Transactions 0000005 to 0000009
o	There are 5 transfer between accounts where the same MSISDN is the beneficiary.
o	An aggregation counting how many distinct customers are sending transactions to the same beneficiary must be added
o	If 3 or more distinct customers are sending money to the same beneficiary in a windows of 1 day ESP should issue a “decline message”

- Rule 3 - Transactions originating or terminating from high-risk locations – For this one we also will add transfer to Risk-beneficiary (meaning in a Restrictive list)
o	Transaction 0000010
o	The beneficiary of this transaction is on the restriction list for a match code 85% of sensitivity (there are 2 small typos on the name)
o	There is no aggregation needed, all transaction should go under the check of beneficiary names against restrictive lists.
o	If a name has a match ESP should issue a “on hold message” warning that a investigation is needed (here we might want to come into play VI investigations.)

<p>*** The below is not built into the example, but you could add it as the data is there to support it ****</p>
- Rule 4 - Sudden increase in account/customer transaction amount by transaction type compared with historic averages
o	Transactions 0000011 to 0000016
o	transactions 0000011 to 0000015 build the customer behaviour of 5 days, then transaction 0000016 is to high considering customer behaviour of previous 5 days.
o	An aggregation of 5 day windows must be created by customer and transaction time accumulating the average of transactions.
o	If a news incoming transaction for the same customer and same type is greater than or equal average * 4 then ESP should issue a “decline message”


## Contributing

> We welcome your contributions! Please read [CONTRIBUTING.md](CONTRIBUTING.md) for details on how to submit contributions to this project. 

## License

> This project is licensed under the [Apache 2.0 License](LICENSE).

