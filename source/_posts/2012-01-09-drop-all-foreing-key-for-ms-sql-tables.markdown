---
layout: post
title: "Drop all foreing key for MS SQL tables"
date: 2012-01-09 18:55
comments: true
alias: /2012/01/drop-all-foreing-key-for-ms-sql-tables.html
categories: [MSSQL, T-SQL]
---
My typical database development process usually looks like this of sql scripts for creating table, populating with test data and dropping tables. Creating tables usually goes pretty smoothly. I just run all script files to create schema and populate it with test data. When want to drop all tables to revert schema to previous version I have to execute scripts that drops tables, columns etc. Looks pretty simple, but using foreign keys force you to drop table in concrete order to resolve all foreign keys dependencies. I'm not exiting to have db script were I should be over careful and remember order how can I drop my tables.

For MySQL solution was to disable foreign key check for build operation and enable it after<br />
For MS SQL I've create simple stored procedure that drops all foreign keys point to this table. After it table<br />
can be deleted without any errors<

Here is stored procedure
{% codeblock lang:psql %}
CREATE PROCEDURE drop_foreign_keys
@table nvarchar(50)
AS
    DECLARE tables_cursor CURSOR
    FOR
      SELECT
        'ALTER TABLE ' + OBJECT_NAME(parent_object_id) +
        ' DROP CONSTRAINT ' + name as dropkey
      FROM sys.foreign_keys
    WHERE referenced_object_id = object_id(@table)
    OPEN tables_cursor;
 
    DECLARE @dropkey sysname;
    FETCH NEXT FROM tables_cursor INTO @dropkey;
    WHILE (@@FETCH_STATUS &lt;&gt; -1)
    BEGIN;
      EXECUTE (@dropkey);
      FETCH NEXT FROM tables_cursor INTO @dropkey;
    END;
 
    CLOSE tables_cursor;
    DEALLOCATE tables_cursor;
GO
{% endcodeblock %}
And how to use it
{% codeblock lang:psql %}
EXECUTE drop_foreign_keys N'TableТame'
IF EXISTS(SELECT 1 FROM sys.objects WHERE OBJECT_ID = OBJECT_ID(N'TableТame') AND type = (N'U'))
DROP TABLE TableName
{% endcodeblock %}


