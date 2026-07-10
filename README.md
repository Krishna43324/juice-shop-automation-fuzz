## Student Information
- Name: Krishna Chinta
- Email: kchinta1@umbc.edu

## Project Title
Juice-Shop-Automation-Fuzz
## Project Overview

## Goal of this Project
The goal of this project is to provide people a way to find vulnerabilities involving lack of permission enforcement and lack of user-based validation or JWT validation

## Technologies Used
1. Kali Linux
2. The juice-shop application
3. OWASP ZAP
4. Python
5. API of OWASP ZAP
6. BurpSuite
7. semgrep

## Installation of Juice-Shop:
1. Install [node.js](https://github.com/juice-shop/juice-shop#nodejs-version-compatibility)
2. Run `git clone https://github.com/juice-shop/juice-shop.git --depth 1` (or clone [your own fork](https://github.com/juice-shop/juice-shop/fork) of the repository)
3. Go into the cloned folder with `cd juice-shop`
4. Run `npm install` (only has to be done before first start or when you change the source code)
5. Run `npm start`
6. Browse to [http://localhost:3000](http://localhost:3000)


## Installation of Semgrep:
In order to understand the different vulnerabilities within the juice-shop application, I decided to use the semgrep SAST tool to scan for vulnerabilities within the code of the juice-shop application. 

1. sudo apt update && sudo apt install python3-pip-y
2. pip install semgrep --break-system-packages

## Execution of Semgrep
1. semgrep -config=p/typescript ~/Desktop/juice-shop/routes

Note: ~/Desktop/juice-shop is the directory that the juice-shop application was installed within
Note: I chose to just scan the code within the /routes directory to find some vulnerabilities initially. /routes contain a lot of files of code that take user input, so I decided to run a scan against those. I configured the scanner as 'typescript' because all the code within /routes was written in typescript. This allows me to only load the typescript rules in the SAST scanner, reducing the time it takes to run the scan.

## Findings from running the semgrep scan












## Semgrep Results

```
┌─────────────────┐                                                                                                                                                                                               
│ 8 Code Findings │                                                                                                                                                                                               
└─────────────────┘                                                                                                                                                                                               
                                                                                                                                                                                                                  
    /home/kali/Desktop/juice-shop/routes/fileServer.ts                   ❯❱ javascript.express.security.audit.express-res-sendfile.express-res-sendfile                                                 ❰❰ Blocking ❱❱                                                   The application processes user-input, this is passed to res.sendFile which can allow an attacker to arbitrarily read files on the system through path traversal. It is recommended to perform input validation in addition to canonicalizing the path. This allows you to validate the path against the intended directory it should be accessing.                                                                     Details: https://sg.run/7DJk                                         33┆ res.sendFile(path.resolve('ftp/', file))                                                                                       /home/kali/Desktop/juice-shop/routes/keyServer.ts                    ❯❱ javascript.express.security.audit.express-res-sendfile.express-res-sendfile                                                                  ❰❰ Blocking ❱❱                                                      The application processes user-input, this is passed to res.sendFile which can allow an attacker to arbitrarily read files on the system through path traversal. It is recommended to perform input validation in addition to canonicalizing the path. This allows you to validate the path against the intended directory it should be accessing.                                                         
          Details: https://sg.run/7DJk                                     14┆ res.sendFile(path.resolve('encryptionkeys/', file))
                                                                        
    /home/kali/Desktop/juice-shop/routes/logfileServer.ts
    ❯❱ javascript.express.security.audit.express-res-sendfile.express-res-sendfile
          ❰❰ Blocking ❱❱
          The application processes user-input, this is passed to res.sendFile which can allow an attacker to
          arbitrarily read files on the system through path traversal. It is recommended to perform input    
          validation in addition to canonicalizing the path. This allows you to validate the path against the
          intended directory it should be accessing.                                                         
          Details: https://sg.run/7DJk                                         14┆ res.sendFile(path.resolve('logs/', file))
                                                                
    /home/kali/Desktop/juice-shop/routes/login.ts
   ❯❯❱ javascript.sequelize.security.audit.sequelize-injection-express.express-sequelize-injection
          ❰❰ Blocking ❱❱
          Detected a sequelize statement that is tainted by user-input. This could lead to SQL injection if   
          the variable is user-controlled and is not properly sanitized. In order to prevent SQL injection, it
          is recommended to use parameterized queries or prepared statements.                                 
          Details: https://sg.run/gjoe                                         34┆ models.sequelize.query(`SELECT * FROM Users WHERE email = '${req.body.email || ''}' AND
               password = '${security.hash(req.body.password || '')}' AND deletedAt IS NULL`, { model:
               UserModel, plain: true }) // vuln-code-snippet vuln-line loginAdminChallenge           
               loginBenderChallenge loginJimChallenge                                                 
                                                                           
    /home/kali/Desktop/juice-shop/routes/quarantineServer.ts
    ❯❱ javascript.express.security.audit.express-res-sendfile.express-res-sendfile
          ❰❰ Blocking ❱❱
          The application processes user-input, this is passed to res.sendFile which can allow an attacker to
          arbitrarily read files on the system through path traversal. It is recommended to perform input    
          validation in addition to canonicalizing the path. This allows you to validate the path against the
          intended directory it should be accessing.                                                         
          Details: https://sg.run/7DJk                                                                       
                                                                                                             
           14┆ res.sendFile(path.resolve('ftp/quarantine/', file))
                                                                   
    /home/kali/Desktop/juice-shop/routes/redirect.ts
    ❯❱ javascript.express.security.audit.express-open-redirect.express-open-redirect
          ❰❰ Blocking ❱❱
          The application redirects to a URL specified by user-supplied input `query` that is not validated. 
          This could redirect users to malicious locations. Consider using an allow-list approach to validate
          URLs, or warn users they are being redirected to a third-party website.                            
          Details: https://sg.run/EpoP                                         19┆ res.redirect(toUrl)
                                                                 
    /home/kali/Desktop/juice-shop/routes/search.ts
   ❯❯❱ javascript.sequelize.security.audit.sequelize-injection-express.express-sequelize-injection
          ❰❰ Blocking ❱❱
          Detected a sequelize statement that is tainted by user-input. This could lead to SQL injection if   
          the variable is user-controlled and is not properly sanitized. In order to prevent SQL injection, it
          is recommended to use parameterized queries or prepared statements.                                 
          Details: https://sg.run/gjoe                                                                       
           23┆ models.sequelize.query(`SELECT * FROM Products WHERE ((name LIKE '%${criteria}%' OR   
               description LIKE '%${criteria}%') AND deletedAt IS NULL) ORDER BY name`) // vuln-code-
               snippet vuln-line unionSqlInjectionChallenge dbSchemaChallenge 
```









