This was a request smuggling challenge to get past the proxy and retrieve the flag from the backend server. 

First step is to review the code and identify how the proxy behaves.
The proxy identifies requests using content length and CRLF characters.
The proxy also blocks Host IPs that are on the private network, necessitating the use of `.nip.io` to obfuscate and bypass the host blacklist.



The IP library is vulnerable to command injection and since flushInterface method passes user input direct into the fucntion to call system commands, we use that to inject code. 

## Remediation
1. Use a well proven proxy that handles `Content-Length` and `Transfer-Encoding` properly as opposed to using a custom parser.
2. Block requests is DNS points to internal disallowed resources. Also, implement a strict outbound allow list.
3. eliminate the vulnerable function `flushInterfaces` or replace the `IP` library that uses the `exec` to execute system commands.

<img width="1232" height="854" alt="flag" src="https://github.com/user-attachments/assets/fce907b0-b5ca-474d-91b1-4d60387a10fc" />
<img width="502" height="506" alt="trials" src="https://github.com/user-attachments/assets/4f95cf18-cb11-4c6a-88ba-b2d75fc33593" />
<img width="889" height="213" alt="flag1" src="https://github.com/user-attachments/assets/bf3c193e-8db2-4100-a44e-5f3ce331fb83" />

