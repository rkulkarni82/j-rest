<b>JRest</b>

  JRest is a Meta Programming Language that builds REST services on a webserver automatically, using JSON as definition language. 
JRest is built using Java, Jersey and JSON Simple, and is deploy-able on any web servers.

We have put a Google group in place for all of you to interact with us on JRest. 
Feel free to let us know about your thoughts, opinions, corrections, requests etc. over the group forum https://groups.google.com/forum/#!forum/jrest

<b>In a Jiffy!</b>

  JRest helps you write and publish RESTFul services faster than you can even imagine. For, example, if you have a table called User in your database and would like to implement GET URI service for it, the JRest code translates to something like

    {
  
          "Users" : {
                  "Query" : "Select username, name, password From User;",
                  "Type" : "GET"
  
          }
    }

If you know typing, it should get over under a minute, now compare this with the time you would spend in making your Java based REST code, you will get the idea on what JRest can do for you.

<b>5 Minutes Guide</b>

<para>
1. Download the source code <br>
2. Stop your web server now, if you are running one! we will start it back in 5 minutes <br>
3. To compile the source, you must have Maven already installed along with Java <br>
4. To generate the war file from source code, go to the j-rest folder and execute mvn install. This should generate the war file which you need to place it under the webapps folder of your webserver. <br>
5. Set an environment variable by the name <b>JREST_DEFINITION_PATH</b> to any of your favorite path. Depending on your platform you may need to reopen/restart the shell/command prompt. <br>
6. To work with Oracle and Sql Server you need to have their jdbc drivers installed and accessible on <b>CLASSPATH</b>. <br>
7. Make sure you have/create a table called User on your database, with username and password columns present in them. <br>
8. Now move into JREST_DEFINITION_PATH, and open a new file jrest.json in edit mode, and fill in the details given below (replace the values accordingly). <br>

    {
        "AUTH":{
              "Query":"Select -3022 From User Where username = ? and password = PASSWORD(?);"
        }
    }
    !
    {
        
        "JDBC":{
                "Host":"<hostname>",
                "Port":"<database port>",
                "User":"<username>",
                "Pass":"<password>",
                "Db"  :"<database/schema name>",
                "Type":"MySql/PostgreSql/SQLServer/Oracle"
        }
    }

<para>
9. Now open another file users.json in edit mode in JREST_DEFINITION_PATH and put the contents given below <br>

    {
  
          "Users" : {
                  "Query" : "Select username, name, password From User;",
                  "Type" : "GET"
  
          }
    }

    
<para> 
10. Now start your web server or execute mvn jetty:run on the shell prompt (you must be inside the j-rest directory where you have uncompressed the JRest source) <br>
11. Observe the output of web server; your definition files must have loaded successfully. Your output should be something similar to following


      2013-02-10 16:23:00,705 [Thread-6] DEBUG org.aprilis.jrest.compile.Compile - Default role value [[-3022]] added to JRest Key [UA4] <br>
      2013-02-10 16:23:00,707 [Thread-6] INFO  org.aprilis.jrest.compile.Compile - Trimmed JSON string is[{"AUTH":{"Query":"Select -3022 From Darwin.User Where username = ? and password = PASSWORD(?);"}}] <br>
      2013-02-10 16:23:00,708 [Thread-6] DEBUG org.aprilis.jrest.store.Store - Definition SQL Query is [Select -3022 From Darwin.User Where username = ? and password = PASSWORD(?);] <br>
      2013-02-10 16:23:00,709 [Thread-6] INFO  org.aprilis.jrest.compile.Compile - Trimmed JSON string is[{"JDBC":{"Host":"localhost","Port":"3306","User":"root","Pass":"xmc4vhcf","Db":"Darwin","Type":"MySql"}}] <br>
      2013-02-10 16:23:00,732 [Thread-6] INFO  org.aprilis.jrest.compile.Compile - Trimmed JSON string is[{"Users":{"Query":"Select username, name, password From Darwin.User;","Type":"GET"}}] <br>
      2013-02-10 16:23:00,733 [Thread-6] DEBUG org.aprilis.jrest.compile.Compile - Default role value [[-3022]] added to JRest Key [Users] <br>
      Feb 10, 2013 4:23:01 PM com.sun.jersey.api.core.PackagesResourceConfig init <br>
      INFO: Scanning for root resource and provider classes in the packages: <br>
      org.aprilis.jrest <br>
      Feb 10, 2013 4:23:02 PM com.sun.jersey.api.core.ScanningResourceConfig logClasses <br>
      INFO: Root resource classes found:  <br>
        class org.aprilis.jrest.pull.Pull <br>
        class org.aprilis.jrest.push.Push <br>
        class org.aprilis.jrest.auth.Authentication <br>

<para>
12. Make sure you have Postman plugin for Google Chrome or REST Client extension for Firefox; this is needed to test the REST service. <br>
13. Create a HTTP POST request with the URL http://localhost:8080/jrest/login and pass the authentication details in Header params with JSON_DATA as the key and {"1":"d", "2":"d"} as the value. <br>
14. Pay attention to the header parameters; we have placed {"1":"d", "2":"d"} as JSON_DATA for the call. 1 and 2 in the actual JSON data represents the positions on the Query given as part of AUTH string ("Select -3022 From Darwin.User Where username = ? and password = PASSWORD(?);". The value "d" of key "1" is supplemented to username and "d" (another 'd') of key "2" is supplemented password of the SQL statement. <br>
15. You should also get a session key of length 32 bytes as a reply to login; we need that key for every other call that we are going to make for JRest, keep it copied to some place.  <br>
16. There you go! You have successfully interacted with your webserver using j-rest.  <br>
17. On the same lines, execute other definitions. The information needed in case of definitions other than AUTH is that the HTTP request should contain the following information in the Header params <br>


      JREST_KEY : Definition key
      SESSION_KEY : Session key received from login HTTP request
      JSON_DATA : Optional data to supply actual values to the query corresponding to the JREST_KEY as shown in the login example. (Refer Pt. 15) 

