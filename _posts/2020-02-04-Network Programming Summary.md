---
title: "Network Programming Note"
excerpt: "'Computer Systems - a programmer's perspective 2nd Edition'의 11장 Network Programming을 읽으면서 정리해놓은 부분들에 대한 요약 포스팅"

categories:
    - Study
tags:
    - Network Programming
last_modified_at:
---

'Computer Systems - a programmer's perspective 2nd Edition'의 11장 Network Programming을 읽으면서 정리해놓은 부분들에 대한 요약 포스팅입니다.


## 1 The Client-Server Programming Model
- A **server** manages some **resource**, and it provides some service for its **clients** by manipulating that resource

The fundamental operation in the client-server model is the **transaction** which consists of four steps:
1. When a client needs service, it initiates a transaction by sending a **request** to the server.
2. The server receives the request, interprets it, and manipulates its resources in the appropriate way.
3. The server sends a response to the client, and then waits for the next request.
4. The client receives the response and manipulates it.

![A Client-Server Transaction](/Images/Client_Server_Transaction.JPG)

- A single host can run many different clients and servers concurrently, and a client and server transaction can be on the same or different hosts