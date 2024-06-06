# MyProjects

# Data Mover App
Infrastructure: Lambda, Cognito, Salesforce connected app
Technologies/frameworks/programming language: python, reactjs, amplify


Main Responsibilities:
Customer wants to use DataLoader (process of importing/exporting data) in a more effective way.

To do so :
Need to have a Salesforce account.
Once you are logged in - it will transfer you for the first time to register in Amplify (put sf credentials - use Amplify to authenticate users, once signed up, it will know you).

# First case : 
Create a new table (insert a query with a cron expression - scheduled job)
After authenticating, Lambda will verify you as a user - with the credentials (org.id received from Salesforce).
Lambda will get triggered by the event of creating a new table - it receives the event with the job_id (each job id consists has an instance).
From that moment Lambda will download the job_id and wrap it in a .zip file - to S3 bucket.
S3 bucket will have .zip files each with different job_id.
Lambda will see that the job is scheduled, and it will trigger an event from Event Bridge.
Lambda receives the scheduled event, and it will create an EC2 instance for the actual job required.
The EC2 instance will get the configuration job_id . zip file .
It will use the data loader to query SF and create the CVS.
Finally, the files can be transformed with Transform-script from EC2 to desired protocol storage server : FTP, SFTP, FTPS, S3.

# Second case
taking in consideration all is set up, the user has a table with his/her jobs scheduled
once the user insert something in that table a proper request is sent to lambda
could it be a request for changing a row or changing the time configuration
a job configuration zip is written to a bucket and a new schedule is written in eventbridge
when it comes time a new ec2 gets started, downloads job configuration, performs the extraction and store it into a bucket
it is possible also to send results into ftp/ftps/sftp hosts
I avoided the authentication/setup part, cognito
