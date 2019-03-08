---
layout: single
permalink: /fSQL/docs/create_table
title: CREATE TABLE
sidebar:
    nav: "fsql_side_bar"
---
<p>Defines a new table<p>

<pre><code class="sql">
    CREATE [TEMPORARY] TABLE [IF NOT EXISTS] tableName 
      [ ( column_name data_type [NOT NULL | NULL] [DEFAULT default_value | GENERATED [ALWAYS|BY DEFAULT] AS IDENTITY(identityValues)] [AUTO_INCREMENT] [UNIQUE [KEY]] [[PRIMARY] KEY] [ ,... ] )
        [CONSTRAINT constraintName] [KEY|INDEX|PRIMARY KEY|UNIQUE] (constraintColumn [ ,... ] ) [ ,... ] 
      ]
    | LIKE otherTable [[INCLUDING|EXCLUDING] IDENTITY] [INCLUDING|EXCLUDING] DEFAULTS]
</code></pre>

<p>CREATE TABLE creates a new table in the schema.</p>

<p>In the v1.3, all TEMPORARY files were stored in memory.  In the master branch, they are stored in temporary files.<p>

<p>Some examples:<p>

<pre><code class="sql">
    CREATE TABLE students (id INTEGER NOT NULL DEFAULT 1, firstName TEXT NOT NULL DEFAULT 'John', lastName TEXT NOT NULL DEFAULT 'Smith', zip INT NOT NULL DEFAULT 90210, gpa DOUBLE NOT NULL DEFAULT 4.0, uniform ENUM('S','M','L','XL') NOT NULL DEFAULT 'M')
</code></pre>

<pre><code class="sql">
    CREATE TABLE people (id INTEGER NOT NULL, firstName TEXT, lastName TEXT UNIQUE KEY, zip INT, gpa DOUBLE, uniform ENUM('S','M','L','XL'), PRIMARY KEY(id), UNIQUE(lastName)
</code></pre>

<p>In the master branch, IDENTITY columns were added to allow more powerful and flexible auto_increment behavior</p>

<pre><code class="sql">
    CREATE TABLE myTable (id INTEGER GENERATED ALWAYS AS IDENTITY(START WITH 7, INCREMENT BY 2, MINVALUE 3, MAXVALUE 10000, CYCLE) PRIMARY KEY, firstName TEXT NOT NULL, lastName TEXT NOT NULL)
</code></pre>

<p>According to the SQL Standard, identity columns doesn't support the "DEFAULT value" syntax.  For backwards compatibility with old versions if AUTO_INCREMENT is used without an identity definition,
it will use the same behavior as mySQL's auto_increment's default behavior:</p>

<pre><code class="sql">
    CREATE TABLE myTable (id INTEGER  GENERATED BY DEFAULT AS IDENTITY(START WITH 1, INCREMENT BY 1, MINVALUE 1, MAXVALUE mysqlIntMax, NO CYCLE) PRIMARY KEY, firstName TEXT NOT NULL, lastName TEXT NOT NULL)
</code></pre>

You can also use the LIKE clause to copy another table for the new one:

<pre><code class="sql">
    CREATE TABLE people LIKE students
</code></pre>

<p>In the master branch, you can choose to include or exclude the default values are added the new type..  You can always choose to include or exclude adding the identity column info to the new table.</p>

<pre><code class="sql">
    CREATE TABLE people LIKE students EXCLUDING IDENTITY INCLUDING DEFAULTS
</code></pre>