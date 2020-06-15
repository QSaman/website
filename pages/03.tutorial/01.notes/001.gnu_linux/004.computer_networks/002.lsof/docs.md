---
title: lsof
taxonomy:
    category: docs
---

# List all open sockets by a process
Use the following command:

```
lsof -i4 -a  -p [process_id]
``` 

If you want to see the numerical value of ports run the following command:

```
lsof -i4 -P -a  -p [process_id]
``` 
