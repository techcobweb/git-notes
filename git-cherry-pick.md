# Cherry-picking git commits
Here is [the official documentation of the git cherry pick command](https://git-scm.com/docs/git-cherry-pick)

Here is [a good article explains things well](http://think-like-a-git.net/sections/rebase-from-the-ground-up/cherry-picking-explained.html)

Thought I'd try an experiment to prove it works as I thought.

I created a branch called `dirty_branch_with_mixed_commits` and added some commits:

`git log`
outputs this:
```
commit 2b7b7426a17bca87d58cd18161a504a5c82778b0
Author: mike-cobbett <my-email>
Date:   Wed Nov 15 09:27:32 2017 +0000

    Another change to json. This time flow.json
    
commit b3e5f910a8f6e994664086bf2a18c7e822f77441
Author: mike-cobbett <my-email>
Date:   Wed Nov 15 09:26:11 2017 +0000

    a change to myScalaClass.scala
    
commit 847baefdcf3e95927134319e25f021f75ce216c0
Author: mike-cobbett <my-email>
Date:   Wed Nov 15 09:24:00 2017 +0000

    a json change 1 to add space to conditions.json
```

... So this branch has two `.json` changes, and one `.scala` type changes, and gives me something to try cherry picking out on.

## Our objective: Remove the change dealing with `.scala` code.

### Check out master, making sure it's up to date locally.
```
git checkout master
git fetch origin
git pull
```

### Create a new branch I want the cherry-picked changes to go into
```
git branch target_good_branch
git checkout target_good_branch
```

### Identify the commits I want to keep
Now. I identify the commits I want to keep. Numbering them from the most recent to the oldest..
- Commit `2b7b7426a17bca87d58cd18161a504a5c82778b0` `Another change to json. This time flow.json` is known as `dirty_branch_with_mixed_commits~0`. i.e: Zero changes back from the most recent.
- Commit `b3e5f910a8f6e994664086bf2a18c7e822f77441` `a change to myScalaClass.scala` is known as `dirty_branch_with_mixed_commits~1`. i.e: One change back from the most recent.
- Commit `847baefdcf3e95927134319e25f021f75ce216c0` `a json change 1 to add space to conditions.json` is known as `dirty_branch_with_mixed_commits~2`

### Do the cherry picking
One important thing is to first make sure you have checked out the branch you want the changes to end up in.
```
git checkout target_good_branch
git cherry-pick dirty_branch_with_mixed_commits~2 dirty_branch_with_mixed_commits~0
```

### Check your git log on the new (good) branch
`git log` reveals this:
```
commit 1a9e2b8b1d7850d4a5812fd93f34ed0d98a3b0eb
Author: mike-cobbett <my-email>
Date:   Wed Nov 15 09:27:32 2017 +0000

    Another change to json. This time flow.json

commit cc1a893029aedad870cf9a214ac186982ee049e1
Author: mike-cobbett <my-email>
Date:   Wed Nov 15 09:24:00 2017 +0000

    a json change 1 to add space to conditions.json
```

Success ! We have a branch with the `.json` file commits, but missing the `.scala` file commits.
