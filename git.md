[Conflict Resolution and Code Reviews]

Have you ever tried to do a code review on a PR that merges a large release branch or feature branch back into mainline, fixing merge conflicts? It’s not pretty. The diffs are often and easily very big, — 50k+ LOC big — have hundreds of commits, and the actual changes made by the engineer resolving the merge conflicts are virtually impossible to spot.

This article covers one nifty trick I’ve been using lately in these cases that lets you review exactly the conflict resolution, while ignoring the mountains of changes that don’t represent any conflicts (and can thus be safely ignored).

### Check out the PR locally
If you haven’t done this before, you can just add the author’s remote (provided they’re using a fork, otherwise you can skip this step) like so:

```bash
$ git remote add nico https://github.com/nico/repo.git
```

Then we can fetch the PR branch, let’s assume it’s called fix-merge-conflicts and it was merging branch-with-conflicts into master, fixing the conflicts along the way:

```bash
$ git fetch nico fix-merge-conflicts
$ git checkout fix-merge-conflicts
```
Assess the situation
The git log should indicate they’ve merged a couple branches. The commit hashes are the key bits here.

```bash
$ git log -1 HEAD
commit a9c9ed7gcc0a76670d86df4b732f1f219ccb48de (HEAD -> fix-merge-conflicts)
Merge: aecf730802 ba9918dad5
Author: Bloodninja <bloodninja@usenet.org>
Date:   Mon Apr 8 12:32:38 2019 +0000

    Merge remote-tracking branch 'branch-with-conflicts'
```
And now we can verify that these indeed belong to each of the branches we care about:

```bash
$ git branch --contains=aecf730802
  master
* fix-merge-conflicts
$ git branch --contains=ba9918dad5
  branch-with-conflicts
* fix-merge-conflicts
```
Now for the magic
```bash
git checkout -b what-were-they-thinking
git reset --hard aecf730802
git merge --no-ff ba9918dad5 # this will conflict
git add .
git commit -m WIP
```
At this point, what-were-they-thinking is the version of the conflict resolution where you just committed things exactly as git merge left them, conflict markers and all.

And, presto, the following command lets you see exactly what they did.

```bash
git diff what-were-they-thinking fix-merge-conflicts
```
Obviously all the commit hashes, branch names, and the author are made up here, and you’ll have to use the actual output you get in each command to feed that into the following command, but you already knew that so I’ll just stop mansplaining and typing these words now!

Do you have any other quick and dirty tricks like this that would blow most people’s minds? This one certainly blew my mind, although I can’t claim credit for it. All credit goes to my brilliant co-worker Joe Gallo, who taught me this nifty trick and is a git mastermind! 🥰