# *Thursday 15.02.17*
Created a Github account and made two online repositories from tutorials. Tried out various functions:

`mkdir name` - _Creates empty directory to comp._

`cd name` - _Enter the directory._

`git init` - _Creates empty repository from that._

`nano filename.txt`- _Creates the file using the specified program, extensions can be different_

`cat filename.txt` - _Shows the text I wrote in the file on the commandline_

`git status` - _Shows what is going on with your file. If you have added something and commited it in, then using this 
will tell you that there is nothing to commit. However, if you edited the text and use status it will
tell you it is untracked so before committing anything you have to add it to be tracked._

`git diff` - _Differences in the file compared to the most recently saved one._
                
`git commit -m` - _Anything you write here in quotation marks will be added as a comment_

`git add filename.txt` - _This is for adding it for tracking._

`git add <directory with files>` - _Adds all files in the specified directory for tracking._

`git push origin master` - _Adds my updates to the online version masterbranch? Using "pull" instead of push copies changes from remote to local._

`git clone https:plapla` - _Clones a repository I make online to the computer._

Tried to make a diary for the course work updates and progress by following the steps [here](https://guides.github.com/features/pages/). Github suggested to create a readme file and then only published whatever I had written there so all the text written here was not published. Could not figure out what was wrong or how to change it


# *Friday 16.02.17*
Spent the whole morning and evening trying to figure out why the diary does not publish from my master branch, google did not help.


# *Saturday 17.02.17*
Spent the whole morning still trying to figure out why this page only publishes my README file. Then changed the name of the file I want to be published to "index" I don't know what happened, but it worked. I realized after messing around for an hour I can not format the text in the way I had planned with starting some lines with indendations.
Used ssh to log into the school's computer and edit the index file. Got a problem using nano, it would not let me save the addition. Now trying to use emacs.


# *Sunday 18.02.17*

Went through the [tutorial](http://swcarpentry.github.io/shell-novice/) for bash.


# *Monday 19.02.17*

Wrote my bash script for the project and uploaded it to my MTLS repository: 

`#usage: bash folder.sh filename_of_project_main_directory

mkdir "$1" #| cd "$1" | mkdir bin | mkdir data
cd "$1" 
echo "This directory organizes a project that trains a SVM.
It creates folders for:
bin - scripts/binaries
src - source codes
data - has folders for training and testing sets
results - contains folders with results from training/testing and prediction results." > README.txt
mkdir bin
mkdir data
mkdir results
mkdir src
cd data
mkdir training_sets
mkdir testing_sets
cd ../results
mkdir testing_results
mkdir prediction_results`

I wanted to make something basic, but this will probably need a few modifications. When running the script you have to enter
bash folder.sh (this is the name of the script) Name (this is the name you want for the main directory to have that is going t hold all the other folders.)
