# Tryhackme

## Q1

Just introduces SSTi

## Q2

We are required to find out which character requence cause errors. After trying out a bunch of things, its `{{`

## Q3

We have to find out which tempeleting engine is being used. They have given a very useful decision tree.

![image](https://github.com/HACKER097/WebEx/assets/38581702/4208f7ee-8db9-4529-8961-1db6103b68ea)

`${7*7}` Not evaluated

`{{7*7}}` Evaluated

`{{7*'7'}}` Evaluated as `7777777`

So according to the tree, we are using Jinja2 or Twig

## Q4

Jinja2 comments start with `{#`, but there are also other ways to start comments.

## Q5



