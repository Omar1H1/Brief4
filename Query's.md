

### Create an account:

```sql
 INSERT INTO Users(first_Name, last_Name, email, password) VALUES ('Omer', 'Hamad', 'omerhamad1o1@gmail.com', 'mypassword');
```

### Create a post:

```sql
INSERT INTO posts (author_id, post_date, content) VALUES (1, CURRENT_TIMESTAMP, 'This is a post');
```

### Create a comment:

```sql
INSERT INTO Comments (post_id, author_id, content, comment_date) VALUES (1, 1, 'new comment', CURRENT_TIMESTAMP);
```

### Show 10 posts from end user:

```sql
   SELECT * FROM posts
   WHERE author_id = 'user id' 
   ORDER BY date 
   DESC LIMIT 10 OFFSET 0;
```

### Show user info:

```sql
 SELECT *
 FROM Users
 WHERE id = 1;
```

### Show post:

```sql
CREATE VIEW PostDetails
AS SELECT p.id 
AS post_id, p.content 
AS post_content, p.image 
AS post_image, p.document 
AS post_document, p.likes 
AS post_likes, p.comments 
AS post_comments, c.id 
AS comment_id, c.content 
AS comment_content, c.likes 
AS comment_likes, u.id 
AS user_id, u.first_name 
AS user_first_name, u.last_name 
AS user_last_name 
FROM Posts p LEFT 
JOIN Comments c ON p.id = c.post_id 
JOIN Users u ON p.author_id = u.id;
```

```sql
SELECT *
FROM PostDetails
WHERE post_id = 1;
```

### Show popular posts in the last 24h :

```sql
SELECT id AS post_id, likes AS total_likes
FROM Posts
WHERE post_date >= CURRENT_TIMESTAMP - INTERVAL '24 hours'
ORDER BY likes DESC
LIMIT 10;

```

### Show recent posts:

```sql
SELECT *
FROM Posts
ORDER BY post_date DESC
LIMIT 10;
```

### Search for User:

```sql
 SELECT * 
 FROM Users
   WHERE first_Name ILIKE '%omer%' OR last_Name ILIKE '%omer%' OR email ILIKE '%omer%';
```

### Search for post by keyword:

```sql
SELECT *
FROM posts
   WHERE content ILIKE '%FIRST%';
```

### Create a group:
 ```sql
  INSERT INTO Groups (creator_id, name, description, type) VALUES (1, 'Rick', 'You still have the right to take Anything you want seriously !!', 'Public');
```

### Filter Posts by Date and Author:
   ```sql
   SELECT * FROM posts
   WHERE date >= 'start_date' AND date <= 'end_date' AND author_id = 'author_id'

```

### Change Group name:

```sql
   ALTER TABLE Group
   RENAME COLUMN old_name TO new_name
   ```

### Update description:
   ```sql
   UPDATE Group
   SET "description or visibilité" = "x"
   WHERE group_id = "x"
```

### Add users into a group:
   ```sql
   INSERT INTO GroupeMembers (group_id, user_id, role) VALUES ('group_id', 'user_id', 'role')
```

### Average time spend by an end user:  

```sql
SELECT AVG(TIMESTAMPDIFF(SQL_TSI_SECOND, login_date, logout_date)) AS average_session_duration
FROM Session;
```

### Average posts number by an end user:

```sql
   SELECT AVG(posts_per_user) AS average_posts_per_user
   FROM (
    SELECT user_id, COUNT(id) AS posts_per_user
    FROM posts
    GROUP BY user_id
    ) AS post_counts;
```

### Average number of posts an end user have seen by session:

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

### Average modification on a post 
```sql
CREATE TRIGGER update_post
BEFORE UPDATE ON Posts
FOR EACH ROW
BEGIN
    INSERT INTO Post_Log (content_before, log_date)
    VALUES (OLD.content, CURRENT_TIMESTAMP;
END;
```

### Average time passed on news feed by type : 

```sql
SELECT AVG(TIMESTAMPDIFF(SQL_TSI_SECOND, login_date, logout_date)) AS average_session_duration
FROM sub_session
where type = "trending"
```
