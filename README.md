# Social Network Prototype Database - Development Plan

## Database Preparation:
- Prepare the database for the full-stack developer.

## User Account:
- End users need to create an account.

## User Profile:
- Users creating a profile must provide necessary information for their page.

## Data Retrieval:
- Create a query to fetch user profile data using either name or ID (preferably ID for data integrity).

## Guest Access:
- Users can access the site without being logged in.

## Third-Party Authentication:
- Users can log in using Google or Discord (implement a query for authentication).

## Post Creation:
- End users can create posts with photos, videos, and documents. Posts can also be created without any attachments.

## Interaction with Posts:
- Other users can like, comment, and share on posts.

## Displaying Posts:
- Use a query to show posts, including likes and comments.

## Comments on Comments:
- Implement a similar approach for displaying likes and comments on each comment.

## Search Queries:
- Create search queries for users and posts, allowing filtering by date, publisher, and other parameters.

---

## Additional Features:

### Post Retrieval:
- Implement a query to retrieve posts for each newsfeed, displaying the number of likes and shares. Ensure pagination for newsfeeds (set limit of posts like 10)

### User Settings:
- Develop queries to display and modify user settings.

### Group Sharing and Roles:
- Enable users to create sharing groups with a role system. Implement queries to manage group parameters, such as changing the name, description, visibility, inviting users, and assigning roles (Super User, Administrator, Moderator, Editor, or Visitor).

### Analytics:
- Retrieve analytics on social network usage, including:
  - Average duration of user sessions.
  - Average number of posts published per user.
  - Average number of posts viewed per user per session.
  - Average time spent writing a post.
  - Average number of modifications to an already published post.
  - Average time spent on a feed, grouped by the type of newsfeed.

### Log Retrieval:
- Develop queries to visualize logs of various modifications made (account name, post text, etc.).


---

## My white Board

let's walk through the app as a user, as i user i want  to create an account "user table with ID or username as unique way to identify a user ,  first name, last name, email (unique), password (needs to check if i can use sha 256 to hash it !!)", after i create my account i need to be able to publish a post "posts table with ID (numeric), author (linked to the user id in some way), date of publication, text content of the post, image url if found, document url if found, number of like, number of comments, number of shares".
so as a user i started as a visitor and ended by publishing my first post.

User can have many posts, a post can have only one author.

i need to create a comment table with comment id (numeric), author, date, content text, likes

### User Table:

- **Columns:**
    - `id` (numeric, primary key, unique identifier for the user)
    - `first_Name` (varchar)
    - `last_Name` (varchar)
    - `email` (varchar, unique)
    - `password` (varchar, hashed using SHA-256 for security)
    - `followers_count` (numeric)
    - `following_count` (numeric)

### Posts Table:

- **Columns:**
    - `id` (numeric, primary key, unique identifier for the post)
    - `author` (varchar, foreign key referencing the User table first_name)
    - `author_id` (numeric, foreign key referencing the User table)
    - `parent_id` (numeric, foreign key referencing this same table)
    - `date` (timestamp, Date of Publication)
    - `content` (text, Text Content of the Post)
    - `image` (varchar, optional URL if found)
    - `document` (varchar, optional URL if found)
    - `likes` (numeric)
    - `comments` (numeric)
    - `shares` (numeric)

### Follows Table:

- **Columns:**
    - `follower_id` (numeric, foreign key referencing the User table, representing the user who is following)
    - `following_id` (numeric, foreign key referencing the User table, representing the user who is being followed)
    - `date_of_following` (timestamp, Date when the following relationship started)
### Groups Table:

- **Columns:**  
    - `id` (numeric, primary key, unique identifier for the group)
    - `name` (varchar, name of the group)
	- `description` (text, description of the group)
	- `type` (varchar, public or private)
	- `creator_id` (numeric, foreign key referencing the User table, representing the super user)

### GroupMembers Table:

- `group_id` (numeric, foreign key referencing the Groups table)
- `user_id` (numeric, foreign key referencing the User table)
- `role` (varchar, role of the user in the group: Super User, Administrator, Moderator, Editor, or Visitor)
### Session Table:

- `id` (numeric, primary key, unique identifier for the session)
-  `user_id` (numeric, foreign key referencing the User table)
-  `login_date` (time_stamp)
- `logout_date` (time_stamp)
### Post_log Table :
- `id` (numeric, primary key, unique identifier for the log)
- `post_id` (numeric, foreign key referencing the Posts table)
- `content_before` (text, Text Content of the Post before update)
- `log_date` (time_stamp)

### Sub_session:
- `id` (numeric, primary key, unique identifier for the log)
- `session_id` (numeric, foreign key referencing the Session table)
- `type` ( ENUM, ('trending', 'newest' ))
-  `login_date` (time_stamp)
- `logout_date` (time_stamp)



### Relationships:

- Users can have many posts (one-to-many relationship).
- Each post can have only one author (many-to-one relationship with the User table).
- Users can have many comments (one-to-many relationship).
- Each comment has one author (many-to-one relationship with the User table).
- Each post can have many comments (one-to-many relationship).
- Many-to-many relationship between users through the Follows table, indicating followers and users being followed.
- Many-to-many relationship between users and groups through the GroupMembers table, indicating users and their roles in groups.
- Users can have many session (one-to-many relationship) 
- A post can have many logs (one-to-many relationship)
- A session can have many types (one-to-many relationship)

---

## Query's :

>un utilisateurs qui voudra intéragir avec le réseau social devra se créer un compte et pourra alors pendant cette première approche complêter sa page profil avec quelques informations personnelles.

- Create an account
   ```sql
   INSERT INTO Users(first_Name, last_Name, email, password) VALUES ('Omer', 'Hamad', 'omerhamad1o1@gmail.com', 'mypassword');
```
> Une fois inscrit, un utilisateurs sera amené a faire sa première publication. J'imagine une sorte de guide d'utilisation du réseau social.


- Create a post
   ```sql
   INSERT INTO posts (author_id, content) VALUES ('user id', 'text content')
```

- Create a comment
   ```sql
   INSERT INTO comments (post_id, user_id, content) VALUES ('content')
```
> Ainsi qu'une requête pour récupérer les posts pour chacun des fils d'actualités avec leurs nombre de "like" et de partages. Bien sûr les fils d'actualités devro:qqnts être paginés.

Resource : [IBM](https://www.ibm.com/support/pages/how-use-sql-pagination-using-limit-and-offset#:~:text=LIMIT%20n%20is%20an%20alternative,rows%20within%20the%20query%20result.)

-  Show 10 posts of an end user 
   ```sql
   SELECT * FROM posts
   WHERE author_id = 'user id' 
   ORDER BY date 
   DESC LIMIT 10 OFFSET 0;
```

>Vous pourrez déjà préparer une requête permettant de lire les données qui seront injectés dans la page profile d'un utilisateur, probablement a partir d'un nom d'utilisateur.

-  Show user info
	```sql
	SELECT * FROM Users
	WHERE id = x
```

> J'aimerai donc une requête pour afficher les commentaires d'un post ainsi que le nombre de "like" sur celui-ci ainsi qu'une requête pour récupérer toutes les réponses d'un commentaire.
- Show comments
   ```sql
   SELECT * FROM comments 
   WHERE post_id = "x"
```
- Show likes
	 ```sql
	 SELECT likes FROM posts
	 WHERE id = "x"
```

> En plus de ces fonctionnalités de base, j'aimerais également implémenter des fonctionnalités avancées, telles qu'un système de recherche par mots clés pour les utilisateurs et les publications, mais aussi avec la possibilité de filtrer les publications par date, auteur, etc.

- Search Users by Keyword:
   ```sql
   SELECT * FROM Users
   WHERE first_Name ILIKE '%m%' OR last_Name ILIKE '%m%' OR email ILIKE '%m%';
```

- Search Posts by Keyword:
   ```sql
   SELECT * FROM posts
   WHERE content ILIKE '%keyword%'

```

- Filter Posts by Date and Author:
   ```sql
   SELECT * FROM posts
   WHERE date >= 'start_date' AND date <= 'end_date' AND author_id = 'author_id'

```

> la possibilité de changer le nom du groupe et/ou sa description, a changer sa visibilité (publique ou privée), mais aussi à inviter d'autres utilisateurs et a leurs assigner un rôle (Administrateur, Modérateur, Éditeur, ou Visiteur).

- Change group name
 Resource : [W3](https://www.w3schools.com/sql/sql_alter.asp)

   ```sql
   ALTER TABLE Group
   RENAME COLUMN old_name TO new_name
   ```

- Update description
   ```sql
   UPDATE Group
   SET "description or visibilité" = "x"
   WHERE group_id = "x"
```
- Add users into a group
   ```sql
   INSERT INTO GroupeMembers (group_id, user_id, role) VALUES ('group_id', 'user_id', 'role')
```

Si vous avez le temps, il serait intéressant de pouvoir récupérer quelques "Analytics" sur l'utilisation du réseau social. J'ai en tête :

1- La durée moyenne des sessions utilisateur.
  Resource: [W3](https://www.w3resource.com/mysql/date-and-time-functions/mysql-timestampdiff-function.php) and [intersystem](https://docs.intersystems.com/latest/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_timestampdiff#:~:text=The%20TIMESTAMPDIFF%20function%20returns%20the,intervals%20between%20the%20two%20timestamps.)

```sql
SELECT AVG(TIMESTAMPDIFF(SQL_TSI_SECOND, login_date, logout_date)) AS average_session_duration
FROM Session;
```

2-  Le nombre moyens de posts publié par utilisateurs.

   ```sql
   SELECT AVG(posts_per_user) AS average_posts_per_user
   FROM (
    SELECT user_id, COUNT(id) AS posts_per_user
    FROM posts
    GROUP BY user_id
    ) AS post_counts;
```

3 - Le nombre moyens de post visionné par un utilisateurs par session.
the publish date of the post must be between login date and logout date AND the poste Author_id must be equal to following_id

### The questions i need to ask to use JOIN : 
1. **What information do we need to retrieve?**
    
    - In this case, we need to find the number of posts viewed by each user during their sessions.
2. **Which tables are relevant to the question?**
    
    - Identify the tables that contain the necessary information. In this example, the "Session" and "Posts" tables are relevant.
3. **How are the tables related?**
    
    - Understand the relationship between the tables. Look for common columns that can be used to join the tables. In this case, the user ID in the "Session" table and the author ID in the "Posts" table provide the connection.
4. **What conditions should be applied to filter the data?**
    
    - Determine the conditions to limit the data to what is relevant. In this query, sessions with logout dates and posts with dates within the session period are considered.
5. **How should the data be grouped?**
    
    - Decide how the results should be grouped to achieve the desired outcome. Here, grouping by user ID ensures that the count of posts is calculated for each user.
6. **What aggregate functions are needed?**
    
    - Identify the aggregate functions required to perform calculations on grouped data. In this example, the COUNT function is used to count the number of posts viewed by each user.

**Logical Steps:**

1. **Select Relevant Columns:**
    
    - Identify the columns you want in the final result. These are usually specified in the SELECT clause.
2. **Join Tables:**
    
    - Understand how the tables are related and use the JOIN keyword to combine them based on the common columns.
3. **Apply Conditions:**
    
    - Use the WHERE clause to filter the data based on specific conditions. This narrows down the results to the relevant information.
4. **Group Data:**
    
    - Utilize the GROUP BY clause to group the data based on a specific column or columns. This is crucial when you need aggregate information for each group.
5. **Apply Aggregate Functions:**
    
    - Use aggregate functions (like COUNT, SUM, AVG) to perform calculations on the grouped data. This is often necessary when dealing with multiple rows that need to be summarized.
6. **Alias Columns (Optional):**
    
    - Use aliases to rename columns or results for better readability, as seen with "COUNT(p.id) AS posts_viewed" in this query.

```sql
SELECT AVG(posts_viewed) AS average_posts_viewed_per_session
FROM (
    SELECT s.user_id, COUNT(p.id) AS posts_viewed
    FROM Session s
    JOIN posts p ON s.user_id = p.author_id
    WHERE p.date BETWEEN s.login_date AND s.logout_date
    AND p.author_id IN (SELECT following_id FROM Follows WHERE follower_id = s.user_id)
    GROUP BY s.user_id, s.id
) AS post_counts;

```

4- Le temps moyen passé a l'écriture d'un post.
We need  to ask the full stack to make an event listener "write a post" and store the date.now() so i can use when the post if published !!

5- Le nombre moyens de modification d'un post déjà publié.

i think i need to create a table to keep track of each modification (maybe use triggers to store the state of post before each time the post has been updated ) 
```sudo_code
 CREATE TRIGGER update_post
    Before UPDATE Posts
    ON post_log
    SET content_before = Posts.content
    WHERE post_log.post_id = x
```

```sql
	SELECT COUNT(id) / (SELECT COUNT(id) 
	FROM Posts 
	AS average_modifications
    FROM post_log;

	```

6 - Le temps moyens passé sur un fil, regroupé par type de fils d'actualités.

```sql
SELECT AVG(TIMESTAMPDIFF(SQL_TSI_SECOND, login_date, logout_date)) AS average_session_duration
FROM sub_session
where type = "trending"
```

