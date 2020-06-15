---
title: netcat
taxonomy:
    category: docs
---

# Send HTTP GET
We need to use `-C` or `--crlf`  to replace `\n` with `\r\n` on stdin:

```
$ ncat -C google.com 80
GET / HTTP/1.0
Host: google.com

```
Note that we need to add an empty line (i.e. press enter twice). You can press `Ctrl+Shift+E` in FireFox to open `Network Monitor` for seeing `GET` and `POST` detail commands. For more information visit this [page](https://developer.mozilla.org/en-US/docs/Learn/HTML/Forms/Sending_and_retrieving_form_data).

# Send HTTP POST
We write the same command as `HTTP GET`:

```
$ ncat -C httpbin.org 80
POST /post HTTP/1.0
Host: httpbin.org
Content-Type: application/json
Content-Length: 17

{"Assignment": 1}


```

It's important to use `Content-Length` and separate header and body with an empty line (line before `{"Assignment": 1}`) and after we enter all input, we press enter key twice.

# Accept TCP connections Simultaneously

Run the follwing command:

```
$ ncat -kl 7777
```

Then establish multiple connection. You can check socket information by running the following command:

```
$ lsof -i4 -a  -p[process_id]
```
