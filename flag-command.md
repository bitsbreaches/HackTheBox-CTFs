Flag command is a browser-based game about escaping a forest by selecting one of four options at each stage (like head north, head west, head south or head east).
However, there is no way to escape the forest even after selecting all visible options. So the trick is to look for an invisible option. 
This was a tricky one but easier than the previous 2, Jailbreak and TimeKORP,

Thetrick is in inspecting the javascript to see where requests are being sent. They are being sent to `/api/monitor` and `/api/options`

```
if (availableOptions[currentStep].includes(currentCommand) || availableOptions['secret'].includes(currentCommand)) {
        await fetch('/api/monitor', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify({ 'command': currentCommand })
        })
```

Send a GET request to `/api/options` to get all possible commands.

```
const fetchOptions = () => {
    fetch('/api/options')
        .then((data) => data.json())
        .then((res) => {
            availableOptions = res.allPossibleCommands;
 ```
 This reveals a command 
 ```
    "secret": [
      "Blip-blop, in a pickle with a hiccup! Shmiggity-shmack"     
 ```
 
 This escapes and wins the game.

 <img width="1916" height="1077" alt="flag1" src="https://github.com/user-attachments/assets/87bbd18d-1920-493c-974a-fccc1597d587" />
 
<img width="1196" height="1020" alt="secret-command" src="https://github.com/user-attachments/assets/e4b0fe85-3acd-4ffc-8986-f6d4bb07a66e" />
