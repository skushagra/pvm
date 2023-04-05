# Documentation 

The Pvm Server enables users to automate printing by sending documents required to be printed to an email address after which a computer program automatically prints the required documents.

# Understanding the program

## Libraries used and there uses

The used python libraries and used are mentioned below :- 

1. ```imap_tools``` - imap_tools is used to access the IMAP server of the email address to which all the files to be printed are sent. Using this library we are able to read and download the emails and attachments.

2. ```subprocesses``` - subprocessed is used to access windows features using a powershell or command terminal for printing email content and files as and when required.

3. ```datetime``` - Python standard datetime library is used to keep tracks of accessing, downloading, printing and logging files and data. datetime library may be used as and when required for running the service and maintaining security.

4. ```smtplib``` - smtplib is used to send queue in, queue out and other relevent emails to users as and when required. SMTP and IMAP can be configured two different email accounts.

5. ```PyPDF2``` - 

6. ```sqlite3``` - sqlite3 libraryis used to access and store data and assets required for the functioning of the service.

7. ```cryptography``` - cryptography module is used to encrypt and decrypt user passwords for IMAP and SMTP servers.

8. ```textwrap``` -  

9. ```fpdf``` - 

```
from imap_tools import MailBox, AND
import subprocess
import datetime
import smtplib
import PyPDF2
import sqlite3 as db
from cryptography.fernet import Fernet
import textwrap
from fpdf import FPDF
```

After the imports the program directly calls the function ```principal```

## Functions created and used

1. ```principal``` - The principal function is the core funtion of the program. It handels all the requests and variables and all other functions assume a supporing role in this program. A detailed description is as follows.

The function opens with the `connection = connection()`. This line is a call to a supporting function called connection. This function returns the ```db.connect(database_address)``` and establishes a connection to the sqlite database.

Then, the `principal IMAP Server address` and `princiapl IMAP email address` value is fetched from the database in the second and third line.

The variable `epass` fetches the encrypted password from the db and then the `principal_imap_password` is calculated by decrypting the IMAP password saved in the db inthe fourth and fifth line.

Finally, the MailBox connecion is established by using the email and password.

The service can be configured enxtensively to match the needs of the user. `check_subject` condition is used to check weather or not the user wants to check the email for a specific subject and print mails containing only that subject. If `check_subject` is true then the email with the specific subjects are downloaded else all the emails are downloaded.

The user will have to options to store all files locally, this local storage address is stored in the `media_storage_directry`.

User can select wheather or not they want to recieve a queue in(when there files go for printing) and queue out(when printing is completed). This feature is maintained where bulk printing is in order and users might have to wait for some time to get their prints. The requirment of this feature is maintained by using the `send_queue_in_email` and `send_queue_out_email`.

Logs of printing commands is maintined in text and table format. Log data is mandatorly stored in the database but the user can creat a text log file if they wish using the `create_txt_log` variable.

`print_attachments` and `print_mail_content` variables is used to configure if the email attachments and mail content is to be printed or not respectivey.

After fetching all the configrations, `for msg in messages` loops through the downloaded email and handles each email indivisually for printing.

Firstly, it is checked who has sent the mail(`from_mail`) and what is the mail subject(`mail_sub`). The current time is noted using variable `ct` and the queue in mail is sent if required. The attachment count(`attl`) and file count(pdf_count) is set to 0.

If the user has configured to print attachments with the email then the conditional statement `if print_attachments == "YES"` block checks the email for attachments and sets print commands for them in the queue.

In the block `for att in msg.attachments` the `attl += 1` increments the attachments counter. Then we create a file in the `media storage directory` for the attachments, as a python object in the variable `f`. We write the attachments into the file. We then increment the counter for the PDF's, and load the print command in the queue.

We then check if the mail content is required to be printed. We change the input text into a pdf using a `text_to_pdf` function and then load the print command into the queue.

The end time of the processing is captured using the `et` variable. We use the `send_queue_out_email` is set to `YES` the program sends a queue out email. If the `create_txt_log` is set to `YES` then we create a text log of the process. At the end the `log_init` function loggs the printing data into in the database.

2. `text_to_pdf` - This function accepts two variables `text` and `filename`. What follows is a standard conversion of text to PDF using the FPDF function. We also use the `textwrap` library in the same function. At the end the function returns a PDF.

3. `decrypter` - This function accepts a connection to the db and a token and returns a decrypted password.

4. `connector` - This function returns a connection object to the SQLITE Database.

5. `logger` - The logger function executes a SQL command to log the printing data into the database.

6. `log_init` - Log init is a function that inatialises a connection to the sqlite db for the logger function.

7. `send_in_queue` - This function uses a the `smtplib` to send an email to the user when the users print mail enters the queue for printing.

8. `send_complete` - This function uses a the `smtplib` to send an email to the user when the users printing command is complete.

This was the complete description of the code in `printreader.py`. This description is for the version `dev-2022.01.02` and was last updated `Thursday, April 6 2022`. 
