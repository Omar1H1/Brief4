
### Create the database :

```sql
	 CREATE DATABASE miniature;
```

![](https://i.ibb.co/FhwYwVR/create-database.png)

### Access database :

```sql
   \c miniature
```

### Create the Users table :

```sql
   CREATE TABLE Users (
   id SERIAL PRIMARY KEY,
   first_name VARCHAR(10),
   last_name VARCHAR(10),
   email VARCHAR(150) UNIQUE,
   password VARCHAR(150),
   followers_count INT,
   following_count INT
   );
```

### Create Posts Table :

```sql
CREATE TABLE Posts (
id SERIAL PRIMARY KEY,
author_id INT REFERENCES Users(id),
parent_id INT REFERENCES Posts(id),
post_date timestamp,
content TEXT,
image VARCHAR(50),
document VARCHAR(50),
likes INT,
comments INT,
shares INT
);
```
### Create Follows Table:

```sql
	CREATE TABLE Follows (
	follower_id INT REFERENCES Users(id),
	following_id INT REFERENCES Users(id),
	date_of_follow timestamp
	);
```

### Create Groups Table :

First I need to create an Enum because PostgreSQL doesn't support it directly 

```sql
 CREATE TYPE type AS ENUM ('Public', 'Private');
```

```sql
	CREATE TABLE Groups (
	id SERIAL PRIMARY KEY,
	creator_id INT REFERENCES Users(id),
	name VARCHAR(10),
	description TEXT,
	type type
	);
```

### Create GroupMembers Table:
same thing as above for the enum
```sql 
CREATE TYPE Roles AS ENUM ('Super user','Administrator','Moderator','Editor','Visitor');
```

```sql
  CREATE TABLE GroupMembers (
  group_id INT REFERENCES Groups(id),
  user_id INT REFERENCES Users(id),
  role Roles
  );
```

### Create Session Table:

```sql
 CREATE TABLE Session (
   id SERIAL PRIMARY KEY,
   user_id INT REFERENCES Users(id),
   login_date timestamp,
   logout_date timestamp
 );
```

### Create post_log Table:

```sql 
 CREATE TABLE Post_log (
   id SERIAL PRIMARY KEY,
   post_id INT REFERENCES Posts(id),
   content_before TEXT,
   log_date timestamp
 );
```

### Create Sub_session:

```sql
 CREATE TYPE session_type AS ENUM ('trending', 'newest');
```

```sql
 CREATE TABLE Sub_session (
  id SERIAL PRIMARY KEY,
  session_id INT REFERENCES Session(id),
  type session_type,
  login_date timestamp,
  logout_date timestamp
 );
```


All tables are up and running locally

![photo](https://i.ibb.co/pyz7j6Y/Screenshot-from-2024-03-12-13-43-12.png)

