# Commit messages should contain reason IDs

So that we can tie changes back to the reason for the change in the issue system, establishing an audit-trail, we should
always put the reason for the change (eg: #3456) into the commit.
```
git commit -m "<reason-for-change> - freeform comment"
```

Then you can find all the stories which contributed to a particular build using:
```
git log
```
