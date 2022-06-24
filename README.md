# mongodb-aws-deployment
This Repo Is Regarding Step By Step Deployment Of Mongodb In Aws

------------
### How to install MongoDB In Ubuntu Linux

------------
Import the public key used by the package management system.


```
wget -qO - https://www.mongodb.org/static/pgp/server-5.0.asc | sudo apt-key add -
```

Create a list file for MongoDB.

```
touch /etc/apt/sources.list.d/mongodb-org-5.0.list
```


```
echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/5.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-5.0.list
```


- *Reload The Local Package Database.*

```
sudo apt-get update
```

- *Install The Mongodb Packages.*

```
sudo apt-get install -y mongodb-org
```

- *Start Mongodb.*

```
sudo systemctl start mongod
```


- *Restart Mongodb Deamon.*

```
sudo systemctl daemon-reload
```

- *If Mongodb Fail To Restart .*

That Shall Be Fault Due To User Permissions In .Sock File, You May Have To Change The Owner To Monogdb User.

```
sudo chown -R mongodb:mongodb /var/lib/mongodb
```

```
sudo chown mongodb:mongodb /tmp/mongodb-27017.sock
```


------------
### Create Mongodb Users For Authentication And Authorization
------------



- *Enable Authorization*

Edit mongod.conf file

```
security:
authorization: "enabled"
```

Add Above Lines In Below File

```
nano /etc/mongod.conf
```

- Open Mongosh Shell

```
mongosh
```

- Create User Admin With Root Role

```
db.createUser(
	{ 
    	user: "admin", 
    	pwd: "admin@123", 
    	roles: [ { role: "root", db: "admin" } ] 
  	} 
)
```

- Authenticate Admit To Use Database


If inside mongosh shell or after the connection

```
db.auth("admin", "admin@123")
```

If outside mongosh shell or before the connection

```
mongosh -u admin -p admin@123 admin
```


- Create User With Multiple Roles For Dbs

```
db.createUser(
	{
		user: "users",
		pwd:  "users@123",
		roles: [    
			{ role: "readWrite", db: "production" },
			{ role: "read", db: "finance" }
		] 
	}
)
```


- Authenticate Admin With User

If Inside Mongosh Shell Or After The Connection

```
db.auth("users", "users@123") 
```

If Outside Mongosh Shell Or Before The Connection

```
mongosh -u users -p users@123 production
```

OR

```
mongosh -u users -p users@123 finance
```

- Show Users 

```
db.getUsers()
```

To Return Information About The Current Connection, Specifically The State Of Authenticated Users And Their Available Permissions.

```
db.runCommand( { connectionStatus: 1, showPrivileges: true } )
```

------------
###  *How To Uninstall Mongodb From Ubuntu Linux*

------------
#### Uninstall steps
------------
###### Type The Following Commands One By One To Uninstall Mongodb:

 1) Stop MongoDB process:

```
sudo service mongod stop
```

2) Completely remove the installed MongoDB packages:

```
sudo apt-get purge mongodb-org*
```



3) Remove the data directories, MongoDB database(s), and log files:

```
sudo rm -r /var/log/mongodb /var/lib/mongodb
```

###### To check if MongoDB is successfully uninstalled, type:

```
service mongod status
```

#### Thank You 🙏
