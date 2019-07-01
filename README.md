# Selva's Blog : SFTP Processing framework using SSIS

## Introduction:
Our finance team receives many files through multiple SFTP servers and they manually download and copy the data to excel and generate reports. Similarly, our collections team extracts data from our internal database and exports the results as csv file and upload to SFTP server. This process was taking almost 4 hours on average per day.
We decided to automate this process by creating a framework using SQL Server Integration Services, this can be used for any SFTP upload/download process.
Over the weekend, I have come up with the framework which does exactly what the **user does** and it takes less than a minute for each process.

## GitHub Repository: 
You can download entire project from [GitHub](https://github.com/datalygence/sftp)

## Scope:
*SFTP (SSH File Transfer Protocol) file processing framework using SQL Server Integration Services (SSIS)*

## Requirements:
Framework should have below functionalities and also handle below listed scenarios
* Process single or multiple files to multiple SFTP servers.
* Support outwards (upload to SFTP server), inwards (download from SFTP) transfers.
* Multiple file processing per process.
* Option to enable and disable any number of processes or a file in a process.
* Process level, file level logging.
* Recording each stages of process in a table (Data Extraction, File Generation, SFTP upload/download, File Archive etc).
* Log number of records processed per file per process.
* Ability to store the data before exporting as a file.
* Re-Startability — restart from last failure point.
* Pre-validation of each process.
* Email notification on completion of execution (success/failure).

## Data Model:
Below data model supports this framework, it contains both control and history tables on Control schema
![Datalygence — SFTP File Processing Framework Data Model](https://miro.medium.com/max/1400/1*JLoSR2E7R64FroVzhfnPLA.jpeg)

## Database & SSIS Solution Design:
Contains both database (SFTP.DB) and SSIS (SFTP.SSIS) projects in single solution
![Datalygence — SFTP-Framework-Database-SSIS-Solution](https://miro.medium.com/max/710/1*u2W7qqxehs6MVqQMjoxQSQ.jpeg)

## Process Flow:
![Datalygence — SFTP-Framework-ProcessFlow](https://miro.medium.com/max/1400/1*28KDASuA-RP7FRVifFNBNg.jpeg)

## Acceptance Criteria:
* Successful execution copies files from (or) to SFTP server to or from File server. **what does this mean??**
* Start and End time of all stages of process stored in a table.
* Created file names should match file name pattern on control table.
* When process fails, restarting the package should start the process from where it failed in the previous execution.
* Email Notifications are sent out on completion (appropriate message for Success/Failure).
* If same files are available on SFTP, new file should overwrite.
* If same file exists on file server, existing file is renamed and new file should added.
