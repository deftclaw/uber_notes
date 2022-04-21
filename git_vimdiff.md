4/7/22,12:10 PM Use vimdiff as git mergetool - Ruslan Osipov

# Use vimdiff as git mergetool

Using vimdiff as a git mergetool can be pretty confusing - multiple windows and little explanation. This is a short
tutorial which explains basic usage, and what the LOCAL, BASE, and REMOTE keywords mean. This implies that
you have at least a little bit of basic vim knowledge (how to move, save, and switch between split windows). If
you don't, there’s a short article for you:
[Using vim for writing code ](http://www.rosipov.com/blog/using-vim-for-writing-code/). Some basic understanding of git and branching is required as well, obviously.

__Git config__  

Prior to doing anything, you need to know how to set vimdiff as a git mergetool. That being said:  
```
git config merge.tool vimdiff
git config merge.conflictstyle diff3

git config mergetool.prompt false
```  

This will set git as the default merge tool, will display a common ancestor while merging, and will disable the
prompt to open the vimdiff.

__Creating merge conflict__  

Let’s create a test situation. You are free to skip this part or you can work along with the tutorial.
```
mkdir zoo
cd zoo
git init

vi animals.txt
```  

Let's add some animals:  

```
cat
dog
octopus

octocat
```  

Save the file:  

```
git add animals.txt

git commit -m "Initial commit"

git branch octodog

git checkout octodog

vi animals.txt # let's change octopus to octodog
git add animals.txt

git commit -m "Replace octopus with an octodog"
git checkout master

vi animals.txt # let's change octopus to octoman
git add animals.txt

git commit -m "Replace octopus with an octoman"

git merge octodog # merge octodog into master
```  

That's where we get a merge error:  

```
Auto-merging animals.txt
CONFLICT (content): Merge conflict in animals.txt

Automatic merge failed; fix conflicts and then commit the result.

Resolving merge conflict with
vimdiff
```  

Let’s resolve the conflict:  

`git mergetool`  

This looks terrifying at first, but let me explain what is going on.
From left to right, top to the bottom:  

`LocaL` - this is file from the current branch BASE — common ancestor, how file looked before both changes

`REMOTE` — file you are merging into your branch MERGED — merge result, this is what gets saved in the repo

Let's assume that we want to keep the “octodog” change (from `REMOTE`). For that, move to the `MERGED` file

(ctrl + w, j), move your cursor to a merge conflict area and then:  
`:diffget RE`  
This gets the corresponding change from REMOTE and puts it in MERGED file. You can also:  
```
:diffg RE " get from REMOTE
:diffg BA " get from BASE
:diffg LO " get from LOCAL
```  
Save the file and quit (a fast way to write and quit multiple files is `:wqa` ).

Run `git commit` and you are all set!  
