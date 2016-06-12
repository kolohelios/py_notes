Remember this command - any time you get connection errors talking about port 5432, run this again:

$ sudo service postgresql start
 * Starting PostgreSQL 9.3 database server
   ...done

$ sudo sudo -u postgres createuser ubuntu --interactive
Shall the new role be a superuser? (y/n) n
Shall the new role be allowed to create databases? (y/n) y
Shall the new role be allowed to create more new roles? (y/n) n

Adding the -u flag to sudo says "set user to the named user and do'. 
So the following command says become the superuser, then become the 
postgres user, and finally run the command:sudo service postgresql start
$ sudo sudo -u postgres run the command here

from command line:
createdb {{dbname}}

psql
\q - quit
\password
\d+ {{tablename}} (shows the table of cols)
psql -d {{dbname}} < schema.sql
psql -d {{dbname}} (pick the DB for the CLI)

inserting:
insert into snippets values ('insert', 'Add new rows to a table');
insert into snippets (message, keyword) values ('Add new rows to a table', 'insert');

query:
select * from snippets;
select message from snippets;
select keyword, message from snippets where keyword='insert';

update:
update snippets set message='Insert new rows into a table' where keyword='insert';

delete:
delete from snippets where keyword='insert';

install psycopg2:
sudo python3 -m pip install psycopg2

db maintenance:
dropdb {{name}} (deletes)
createdb {{name}} (creates)
(and then source the sql file)

pg_dump - dumps data to a SQL file