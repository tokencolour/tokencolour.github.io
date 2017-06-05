---
layout: post
title: "Undoing commits with git"
date: 2017-06-05
---

Hi! I learnt how to undo commits. I want to document this for my own good. 
(Yeah, you guessed it right. I am one sloppy creature.)

check your files with
```
git status
```

If you want to undo 2 commits
```
git reset HEAD~2
```

Now check with ``git status`` again
you'll find the changes back as unstaged files.
You can do the changes you need and 
You can use ``git add`` to add them.

Consequently you can recommit them.
