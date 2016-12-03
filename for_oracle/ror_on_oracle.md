# Ruby on Rails on Oracle: 

## Ruby Developer Center
[Ruby Developer Center](http://www.oracle.com/technetwork/database/database-technologies/scripting-languages/ruby/index.html)

## A Simple Tutorial

*   [A Simple Tutorial](http://www.oracle.com/technetwork/articles/dsl/haefel-oracle-ruby-089811.html)
   
   ### Set up the Oracle database:

     You wouldn't be reading this article if you were not interested in using Rails in combination with Oracle,
     so you'll need to have an installed instance of the Oracle database and create a schema for this sample 
     Rails application. The easiest way to do that is to use SQL*Plus as follows. (The assumption is that you 
     already have an Oracle database installed and know how to use it. Oracle Database 11g is used in this tutorial.)
    
     Using SQL*Plus, create a user that you can use for this application.
    
        ```sql
        SQL> GRANT CONNECT, RESOURCE TO ruby IDENTIFIED BY ruby;
        SQL> ALTER USER ruby DEFAULT TABLESPACE users TEMPORARY TABLESPACE temp;
        SQL> EXIT
        ```
      If you use a remote Oracle database, you need to install the Oracle Database Instant Client which allows you to run your applications without installing the standard Oracle client or having an ORACLE_HOME. To install it, do the following:
    
 ## Using Oracle and Ruby on Rails      
 * [Using Oracle and Ruby on Rails ](http://www.oracle.com/technetwork/testcontent/rubyrails-085107.html)     
 
 ## HR Schema on Rails
 * [HR Schema on Rails](http://www.oracle.com/technetwork/articles/saternos-rails-085771.html)