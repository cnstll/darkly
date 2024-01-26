
### Breach Description:

The Search image input is susceptible to SQL injection such that:

```
ID: 1%27 OR 1=1 UNION SELECT comment, null FROM list_images
Title:
Url : If you read this just use this md5 decode lowercase then sha256 to win this flag ! : 1928e8083cf461a51303633093573c46
```
leads us to the Flag

### Breach Explanation:

This explanation is common with the sql injection for users. much of the discovery was done there, especially listing out the columns and tables, that could be interesting.

FLAG: F2A29020EF3132E01DD61DF97FD33EC8D7FCD1388CC9601E7DB691D17D4D6188

## Breach Fix:

Firstly getting the direct return of the SQL query in the page makes this way too easy, allowing multiple returns instead of one, giving errors that may help with discovery etc. transforming the return to make sure it only returns one field, and does not forward errors would complicate this exploit tremendously.

Secondly to stop this exploit completely, one needs to improve sanitation so that a basic bypass will not work anymore. One can obviously go about this manually, but many libraries and ORM will do this reliably. In this case the id is a simple int, so even throwing an error when any non digit char is met would have stopped this exploit.
