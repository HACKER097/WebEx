# Help

`Help command gives us a list of commands` Trying them one by one.

1. `prepare`: Cool intro
2. `fosciety`: Looks like an invitation
3. `inform`: More random shit
4. `question`: I want to click join now, but lets keep going
5. `join`: Fuck this i am joining

# Key 1

Lets go to `/robots.txt`

We see:

```
User-agent: *
fsocity.dic
key-1-of-3.txt
```

Key 1 is found at `/key-1-of-3.txt`

`073403c8a58a1f80d943455fb30724b9`

# fsocity.dic

Next lets get `/fsocity.dic`

It looks like a wordlist, maybe the key is in here somewhere?

Just going throgh the file tells us that the key is not in here because there many lines which can look like the key

```
56571775856454c0f28d57
Contributions71
2233s
60711260056445d2860db5
```

I would try feeding this into gobuster, but the file is too big.

I did try it with a more reasonable wordlist though. I found `/image` with an interesting looking image.

But `http://10.10.70.121/license` gave me: what you do just pull code from Rapid9 or some s@#% since when did you become a script kitty?

Aah yes, Rapid9 and script kutty.

The html had: `ZWxsaW90OkVSMjgtMDY1Mgo=` which is b64 decoded to `elliot:ER28-0652`

Sadly this is not the second key. `elliot:ER28-0652` is also found in `fsocity.dic`

This does look like a username and password combo.

I tried it in `http://10.10.70.121/wp-content/` which gobuster did find, but I ignored it thinking Its real, and I should leave alone. But it worked! 

I am logged into wordpress now
