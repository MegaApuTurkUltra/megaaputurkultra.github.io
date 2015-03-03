# Scratch API reference #

Just so I can stop capturing things with Fiddler all the time.
Not sure if all of this is required, but just to be safe, include all the headers.

## Comments ##
## Posting ###
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
Response: The comment chain posted on or created, as HTML.

Comments: not sure why your user ID is required for replying but not for posting a new comment.

### Getting ###
```http
GET http://scratch.mit.edu/site-api/comments/project/<project id>/?page=<page number> HTTP/1.1
Host: scratch.mit.edu
Connection: keep-alive
Cache-Control: max-age=0
Accept: */*
X-Requested-With: XMLHttpRequest
User-Agent: Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/40.0.2214.115 Safari/537.36
Referer: http://scratch.mit.edu/projects/<project id>/
Accept-Encoding: gzip, deflate, sdch
Accept-Language: en-US,en;q=0.8,hu;q=0.6
Cookie: scratchlanguage=en; scratchsessionsid=; scratchcsrftoken=


```
Response: The specified page of comments, as HTML.

# More soon! #
