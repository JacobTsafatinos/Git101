# Git101

# Backtracking - How to undo "most" things.

## Undo a push
Make a change, run ```git push```. Oh no this broke something! We need to undo one of the commits.

Use ```git revert <SHA>```. Reverting creates a new commit that is the inverse of the SHA passed in (anything removed or added in the old commit will be added or removed in the new commit)

Now you can ```git push``` the new "inverse" commit to undo the broken one.


## fix your last commit (message or code)
Let's make a new commit. ```git commit -m "getting shit done"```. oh no! Shopify took "get shit done" out of it's mandate. I guess we should fix this message.

Use ```git commit -amend -m "getting work done"```. 

Now you can see your new commit message with ```git log```. NOTE: the commit SHA is now different than before.

```git commit --amend``` updates and replaces the most recent commit with a new commit which will combine any staged changes with the previous commit. If nothing is currently staged, then it just rewrites the previous commit message. Also if no message is specified then ```git commit --amend``` will open an edit window.

## resetting local changes.

Let's make a few commits...
```git commit -m "badunkadunk"```
```git commit -m "hunkadunk"```
```git commit -m "splunk"```

Those were horrible commits! And what kind of messages are those? Let's reset the last 3 commits we just made.

```git reset <target SHA>``` will change the repository's history back to what it looked like when we commited the target SHA. It's important to note that ```git reset``` preserves the working directory, so although the commits are gone, the contents are still on disk. You can see them if you do ```git status```. Sometimes if you want to undo the commits and changes at the same time you can use ```git reset --hard <target SHA>```. Although we advise that you always use caution when using ```--hard```.

## Undo Undo of local changes (redo after undo)

We just got rid of these changes and now you want them back already?! I know what you're thinking, we can just use the same technique we just learned and ```git reset``` to the last SHA... but I didn't keep track of the SHA's and my ```git log``` doesn't show them anymore.

That's okay because we have ```git reflog``` which shows a list of all the times ```HEAD``` changed (make commits, switch branches, resets).

NOTE: ```reflog``` is specific to you. You can't use reflog to restore someone elses un-pushed commits.
NOTE: ```reflog``` doesn't last forever, if an object becomes "unreachable", git will garbage collect it.

Now that we're familiar with ```git reflog``` we can do a lot with it. We have a few options: 
1. we could find the SHA we want and do ```git reset --hard <SHA>```.
1. If we want to replay a commit into our repo we could use the very useful ```git cherry-pick <SHA>```

## Same thing but with branching

Picture this classic scenario. We've just made a bunch of commits, and are about to push but quickly realise we're on ```master```. If only there was a way to make those commits on a branch.

Follow this recipe: ```git branch <name>``` -> ```git reset --hard origin/master``` -> ```git checkout <name>``` -> ```git push --set-upstream origin <name>```

A lot of things are happening here, let's go through them.

1. ```git branch <name>```: You most likely use ```git checkout -b <name>``` most of the time to make new branches. That's a short cut to make a branch and immediately switch to it. However we don't actually want to switch to it yet. ```git branch <name>``` will create a new branch which points to your most recent commit, but leaves us checked out (head) at ```master```.
1. As we learned above, ```git reset --hard origin/master``` will move ```master``` back to ```origin/master```, before we made any commits. This is okay because we've moved them to the new branch first.
1. ```git checkout <name>``` moves our ```HEAD``` to point to our new branch.
1. Finally we push the new commits up for review.

## Mass undo/redo



## maybe rebasing on master? might be covered already




# TODO
1. Basic workflow simulation: Clone repo -> Create branch -> Make change -> add/commit change (add new file based on participants name) -> push (set upstream) -> merge (no conflicts since all new files should be unique)
2. Squashing simulation: Same as #1 but introduce "PR feedback" and have them do multiple commits which they'll then have to either amend or squash.
3. Many people working on same repo simulation: Clone repo -> Create branch -> Make change -> add/commit change to existing file -> one at a time have them push/merge and keep fixing conflicts until there's no more conflicts to resolve.
4. Collaboration simulation: create branch -> pull changes from someone else's branch -> make multiple commits -> squash commits down -> push changes back to that branch.
5. clone repo -> create branch -> make small change -> pull --rebase new changes from master (instead of merge) -> push into master

