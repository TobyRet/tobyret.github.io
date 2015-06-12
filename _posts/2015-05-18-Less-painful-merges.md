---
layout: post
name: Less-Painful-Merges
title: A less painful way to handle merge conflicts
date: 2015-05-18 18:31:00 +00:00
author: Toby Retallick
---

Merge conflicts. 

Been there right? 

Despite your best intentions and following the recommended advice out there (e.g. making small pull requests and merging to master often), you are bound to come across this issue more often than you’d like.

That being the case, I'd like to share with you a simple 4 step process for handling merge conflicts. This technique was passed onto to me recently by my colleague Samir at Codurance. Over a period of a week I had been diligently working on a feature branch, resulting in what was to be a rather large pull request. In the meantime, my colleagues had been doing the right thing - merging their small pull requests into master at regular intervals.

Once I had finished my ticket, I realised my feature branch was way behind. Doing a git merge would be guaranteed to result in a lot of conflicts. I needed the latest changes from master in my feature branch before I could merge my changes <em>back</em> into master.

So, without further adieu, here is the 4 step process for handling conflicts.

###Step 1 - Merge vs Rebase? Use the right tool for the job

Both git rebase and git merge are designed to incorporate changes form one branc into another branch, but the way they go about it differs. 

If you want merge a feature branch back into master and you are certain your code is up to date with master, go for a <strong>git merge</strong>. Performing a merge is a <em>non-destructive operation</em>. The existing branches are not changed in any way

If you are working on a feature branch and you have fallen behind master go for <strong>git rebase</strong>. 
This moves your entire feature branch to begin on the tip of the master branch, effectively incorporating all of the new commits in master. But, unlike a merge, rebasing re-writes the project history by creating brand new commits for each commit in the original branch. However, because rebase merges every commit individually, conflicts will be served in smaller chunks making them easier to fix and understand. This makes it a better option when merge conflicts are likely to occur.

Since my feature branch was behind master, I opted for <em>git rebase</em>.

###Step 2 - Start the rebasing process
From your feature branch type:

```git rebase master```

### Step 3 - Keep it slow and steady, my friend
Now type:

```./gradlew build && git add -A && git rebase --continue```

The above command rebuilds the current project (in this example I am using grade). If everything passes (tests, compiling etc), the project completes its build. If the tests fail, you need to do some fixing.

Once the project is successfully built, your changes are added then the rebase continues to the next conflict.

###Step 4 - Rejoice
Repeat step 3 until all your conflicts have been resolved. You can now push your changes. Note the use of the ‘force' flag.

```git push -f```

Finally, raise your pull request and pretend that this nightmare never happened.

