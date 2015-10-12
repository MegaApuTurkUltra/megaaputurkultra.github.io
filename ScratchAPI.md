# Scratch API reference #

Just so I can stop capturing things with Fiddler all the time.
Not sure if all of this is required, but just to be safe, include all the headers.

## Comments ##
### Projects ###
#### Posting ####
```http
POST http://scratch.mit.edu/site-api/comments/project/<project id>/add/ HTTP/1.1
Host: scratch.mit.edu
Connection: keep-alive
Accept: text/html, */*; q=0.01
Origin: http://scratch.mit.edu
X-Requested-With: XMLHttpRequest
User-Agent: Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/40.0.2214.115 Safari/537.36
X-CSRFToken: 
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
Referer: http://scratch.mit.edu/projects/<project id>/
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.8,hu;q=0.6
Cookie: scratchlanguage=en; scratchsessionsid=; scratchcsrftoken=

{"content":<A>,"parent_id":<B>,"commentee_id":<C>}
```
Project ID: the project ID (duh)
A: Your comment's contents  
B: Normally just "", if posting a reply, use the numeric comment ID of the parent comment.  
C: Normally just "", if posting a reply, use the numeric ID of the user being replied to.  
Response: Your comment, as HTML.

#### Getting ####
Normal GET request to
```http
http://scratch.mit.edu/site-api/comments/project/<project id>/?page=<page number>
```
Project ID: The project ID  
Page number: The page of comments to get  
Response: The comments, as HTML.

#### Deleting ####
```http
POST /site-api/comments/project/<project id>/del/ HTTP/1.1
Host: scratch.mit.edu
Connection: keep-alive
Origin: https://scratch.mit.edu
User-Agent: Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/45.0.2454.101 Safari/537.36
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
Accept: text/html, */*; q=0.01
X-Requested-With: XMLHttpRequest
X-CSRFToken: 
Referer: https://scratch.mit.edu/projects/<project id>/
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.8
Cookie: scratchlanguage=en; scratchsessionsid=; scratchcsrftoken=

{"id":"comment id"}
```

### User profiles ###
#### Posting ####
```http
POST http://scratch.mit.edu/site-api/comments/user/<username>/add/ HTTP/1.1
Host: scratch.mit.edu
Connection: keep-alive
Content-Length: 58
Accept: text/html, */*; q=0.01
Origin: http://scratch.mit.edu
X-Requested-With: XMLHttpRequest
User-Agent: Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/40.0.2214.115 Safari/537.36
X-CSRFToken: 
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
Referer: http://scratch.mit.edu/users/<username>/
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.8,hu;q=0.6
Cookie: scratchlanguage=en; scratchsessionsid=; scratchcsrftoken= 

{"content":<A>,"parent_id":<B>,"commentee_id":<C>}
```
Username: Who's profile to post to  
A: Your comment's contents  
B: Normally just "", if posting a reply, use the numeric comment ID of the parent comment.  
C: Normally just "", if posting a reply, use the numeric ID of the user being replied to.  
Response: Your comment, as HTML.

#### Getting ####
Normal GET request to
```http
http://scratch.mit.edu/site-api/comments/user/<username>/?page=<page number>
```
Username: Who to get profile comments from  
Page number: The page of comments to get  
Response: The specified page of comments, as HTML.

#### Deleting ####
```http
POST /site-api/comments/user/<username>/del/ HTTP/1.1
Host: scratch.mit.edu
Connection: keep-alive
Origin: https://scratch.mit.edu
User-Agent: Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/45.0.2454.101 Safari/537.36
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
Accept: text/html, */*; q=0.01
X-Requested-With: XMLHttpRequest
X-CSRFToken: 
Referer: https://scratch.mit.edu/users/<username>/
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.8
Cookie: scratchlanguage=en; scratchsessionsid=; scratchcsrftoken=

{"id":"comment id"}
```

# More soon! #
