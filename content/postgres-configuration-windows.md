# Postgres Configuration Windows

On Windows when you install postgres, you get choices about where files can be stored. This is a create feature, but if you come to 
do upgrades or configuration changes later it is important to know where files are and which ones actually make a difference.

## Find Data Directory 

Open a connection to the database (eg in PGAdmin) and then
`SHOW data_directory` 
this will display the path to where you database are stored

In this directory you will find your 
`pg_hba.conf` and `postgresql.conf` files.

If you want to listen on your local network you should 
[adjust to your network](https://www.postgresql.org/docs/current/runtime-config-connection.html#GUC-LISTEN-ADDRESSES)


  * '*' = All Interfaces
  * 192.168.1.14 = Your current IP Address

You can then refine which IPAddresses are allowed to connect to this via the `pg_hba.conf` file

for example to only allow a LAN machine to connect 
```
host all all 192.168.1.149/32 scram-sha-256
```

always use the highest of the possible options of the Method (last column)

After making these changes restart your postgres service. 
