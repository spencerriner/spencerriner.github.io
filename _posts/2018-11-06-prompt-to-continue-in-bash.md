---
layout: post
title: Prompt to Continue in Bash
tags: [bash]
author: Spencer Riner
comment: false
---

Ruthlessly lifted from https://www.shellhacks.com/yes-no-bash-script-prompt-confirmation/.

## Read

```bash
read -p "Are you sure? " -n 1 -r
echo    # (optional) move to a new line
if [[ ! $REPLY =~ ^[Yy]$ ]]
then
    exit 1
fi
```

## Case

```bash
while true; do
    read -p "Do you wish to install this program?" yn
    case $yn in
        [Yy]* ) make install; break;;
        [Nn]* ) exit;;
        * ) echo "Please answer yes or no.";;
    esac
done
```

## Menu

```bash
echo "Do you wish to install this program?"
select yn in "Yes" "No"
case $yn in
    Yes ) make install;;
    No ) exit;;
esac
```
