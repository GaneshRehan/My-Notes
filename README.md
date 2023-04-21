# My-Notes
This is the notes of the skills i practice , so I don't need to go through youtube tutorials again if I forget it.
# This is written in Python3.

import sqlite3 as sql

# sqlite3 is not a case sensitive script. However, for thesake of readability all sqlite keywords will be capatailized.

# To connect to a database or to create a new one if it doesnt exist.
conn=sql.connect('Customer.db')

# To do something in database we need a cursor. It executes our commands in database.
# To create a cursor.
csr=conn.cursor()

# To create a table and insert columns.
# csr.execute('''CREATE TABLE customers(
#     first_name text, 
#     last_name text,
#     email text
# )''')
# In above case first_name, last_name, email are column names and text is data type. We only have 5 datatypes in sqlite3 i.e., null, integer, real, text, blob.

# To insert data into out tables. we can insert 1 record at a time
# csr.execute("INSERT INTO customers VALUES('Alex','Ambrose','')")

# To insert many records at once, we have to create a list containing touples or lists as items. Each touple or list will be added independently into the table.
# many_customer=[
#     ['WEs','Brown','Wes.brown@abc.qwe'],
#     ('Dan','Pas','dan,pas@qwe.rty'),
#     ('Steph','Kneave','steph.kneve2qwe.asd')
#     ]
# csr.executemany('INSERT INTO customers VALUES(?,?,?)',many_customer)


# QUERY and FETCHHALL
# To see whats inside the database, we need to query the database.
# csr.execute('SELECT * FROM customers') # The following 3 functions will not return anything if this command is not executed.
# SELECT clause is used to select required columns from the table
# It will fetch the first item in the table.
# csr.fetchone()
# To fetch as many as we want. We have to pass how many we want.It will return the first n records. If the no. of records are less than the passed number of records, it will return all the records.
# csr.fetchmany()
# It will return everything.
# csr.fetchall()
# The above 3 commands donot show any output. To get the output we need to print it.
# The above 3 commands return lists. We can store returnes lists in variables.
# print(csr.fetchall()) #it donot take arguements
# print(csr.fetchone()) #it donot take arguements
# print(csr.fetchmany(20))
# Since the returned ones are tuple, we can access a particular element by its index.
# print(csr.fetchone()[0])
# Here we will get (j)[i] i indexed record in table if i<=j else it will show indexerror:list out of bounds error .
# print(csr.fetchmany(1)[2])
# It will show [i] i'th indexed record if it exixts else it shows indexerror:list out of bounds error .
# print(csr.fetchall()[15])
# After we store the returned lists from these 3 functions, we can loop through the accordingly a we wish. An example is givemn below.
# items= csr.fetchall()
# for item in items:
#     print(f'First Name: {item[0]}   Last Name: {item[1]}    Email: {item[2]}')

# csr.execute('SELECT *,rowid FROM customers') # If it is (rowid,*) rowid will be first then other columns will come. So it is place sensitive. In (a,b) 1st a will come and then b will come. Here rowid can be considered as a primary key. 
# ***A primary key is unique for each record.***
# items=csr.fetchall()
# for item in items:
#     print(item) 

# To get specific records.
# csr.execute('SELECT * FROM customers WHERE last_name="Pas"') # If we write last_name='Pa%' , it will return all records whole name start with Pa and can have anything after that. If it is '%Ps' , same but beginning can be anything.
# WHERE clause is used to give a specific condition to all records based on a particular column.
# items=csr.fetchall()
# for item in items:
#     print(item)

# Update Records
# csr.execute('''
#             UPDATE customers SET first_name="Danny" WHERE last_name="Pas"
#             ''')          # UPDATE clause will update the records of the specified table for specified columns when the condition of WHERE clause is satisfied.
# csr.execute('SELECT * FROM customers')
# items=csr.fetchall()
# for item in items:
#     print(item)

# csr.execute('''
#             UPDATE customers SET first_name="Dannie" WHERE last_name="Pas" AND rowid=6
#             ''')          # WHERE clause can have multiple conditions seperated by AND OR clauses and ordered by ().
# csr.execute('SELECT rowid,* FROM customers')
# items=csr.fetchall()
# for item in items:
#     print(item)

# Delete records. It is done with DELETE clause.
# csr.execute('DELETE FROM customers WHERE rowid=5 or rowid=6 or rowid=7')          # If we delete by rowid then that rowid wont be displayed. For example if rowid is upto 15, then here we will get rowid's of 1 to 4 and 8 to 15 but not 5,6,7
# csr.execute('SELECT rowid,* from customers')
# items=csr.fetchall()
# for i in items:
#     print(i)

# Order the data. It is done by ORDER BY clause. In general, the records are ordered in ascending order.
# csr.execute('SELECT rowid,* FROM customers ORDER BY email')         # We can order our records by any column. If wwe add DESC clause after coolumn, the records will be ordered in descending order according to the key. We can even explicitly add ASC in place of dESC for ascending order, but it is not required because it is the default order.
# items=csr.fetchall()
# for i in items:
#     print(i)

# AND, OR clauses. They are used to add more conditions to our WHERE clause.
# csr.execute('SELECT * FROM customers')
# csr.execute('''
#           UPDATE customers SET first_name="Dannie" WHERE last_name="Pas" AND rowid=6
#              ''')         # WHERE clause can have multiple conditions seperated by AND OR clauses and ordered by ().
# We can use LIKE clause to check with strings as follows.
# csr.execute('SELECT * FROM customers')
# csr.execute('''
#             UPDATE customers SET first_name="Wes" WHERE last_name LIKE "Br%"
#                           ''') 
# csr.execute('SELECT rowid,* FROM customers')
# items=csr.fetchall()
# for item in items:
#     print(item)

# Limiting results. It is done to limit the number no. of recorde in output. It is done by LIMIT clause as LIMIT <number of records> as follows
csr.execute('SELECT rowid,* FROM customers ORDER BY rowid DESC LIMIT 5 ')           # LIMIT clause always comes at end of statement. Here the ORDER BY clause is optional, used only for showing syntax of the qurey.
items=csr.fetchall()
for item in items:
    print(item)

print('Command executed successfully')

# To commit our actions to our database.
conn.commit()

# To delete a table. It is done by DROP clause.
# d1=sql.connect('db1.db')
# c1=d1.cursor()
# c1.execute('''CREATE TABLE table_d1(
#     asd text,
#     zxc text)''')
# c1.execute("INSERT INTO table_d1 VALUES('asdf','qwer')")
# c1.execute('SELECT * FROM table_d1')
# items=c1.fetchall()
# for i in items:
#        print(i)
# c1.execute('DROP TABLE table_d1')
# db1.commit()
# db1.close()

# To close the connection.
conn.close()
