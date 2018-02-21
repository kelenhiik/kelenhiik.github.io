# Wednesday 21.02.17

Went through the basic idea of the project. Most of it I had already figured by reading the provided material and articles. What I still do not know is how to do all of it. I keep going through the assignments for the last course and google stuff, but nothing really klicks with my mind so not really getting anywhere with this. I tried to create something that would be a "sliding window" without hard coding it. The only thing I could come up with meant that I needed a function that takes in a sequence and a value for the sliding window and then prints out a list where the whole sequence is turned into sliding window sequences. I am not even sure if this was the point or if I do really understand the concept, but sth like this: if the seq is "ADERRG" and sw = 3, then I would get ['A'D'E'+'D'E'R'+'E'R'R'+'R'R'G']. So I wanted a for loop go over the sequence starting from position 0 until the position of the sliding window value (that position itself is not included, which is also not needed, since it takes from 0 not 1) and then move on one step at a time. I tried creating a list out of the sequence so I could get the positions for them in the for loop - I did this also by using a for loop, because I couldn't think of anything else. Then I also realised that I need another list that the main for-loop can go through to get the position-number I want, so I just created a variable that makes a list out of the range of numbers in the length of the input sequence. Then I made the main for loop that I wanted that takes the numbers from the variable I just made and also uses the information about the sliding window size: essentially it puts stuff into an empty list I made earlier and that stuff is a number from the list of numbers I made that specifies a position int the sequence list I made and then extend those positions/numbers until it reaches a position/number that is specified by the sliding window value (and onto that I had to add the number that was used from the list of numbers, so the start and the end would always be fixed in relation to the sliding window value). It kept going over the actual sequence, in the sense that when it reached the end of the sequence it still continued, but the "windows" it created were not the right size, thus I figured I have to tell it that it should not go over the length of the actual input sequence with an if clause after the loop. The whole thing in the end looked like this:

```
def slidw1(seq,sw):
    #sw1-sliding window positions after element in view
    #sw2-sliding window positions before element in view
    #sw1=(sw//2)
    #sw2=-sw1
    #print (sw2)
    #sw=int(sw)
    window=[]
    all_windows=[]
    pos_in_window=list(range(0,len(seq)))
    #print(pos_in_window)#---[0, 1, 2, 3, 4, 5]
    for element in seq:
        #for positions in
        window.append(element)
    #print(window) #['A', 'D', 'E', 'R', 'R', 'G']

    
    
    for number in pos_in_window: 
        #print (number)
                        #0 
                        #1
                        #2
                        #3
                        #4
                        #5

        if sw+number <= len(seq):
        # if sliding window size and the number from pos_window sum up to be less or equal to the length of the sequence, so it wouldn't continue going over the sequence creating lists smaller than the window size. 
        
            all_windows.extend (window[number:(number+sw)])
            #adds all the values in window list that correspond to the positions from and between number and sw+number (where it starts the value and goes one by one, so does the sliding window shift as much) into one continous list.
         
    
    print (all_windows) #----['A', 'D', 'E', 'D', 'E', 'R', 'E', 'R', 'R', 'R', 'R', 'G'] this maybe works, but might be too complicated and mby does not work for larger data??
    
```



I have no idea if this is what I should do or if it actually works. ~6h.


# Tuesday 20.02.17

Had to write a parser for the data file in my project. Layout is always:

first line: heading

second line: sequence

third line: topology.

Tried to use previous parsers in the course which used dictionaries:

```
def fasta_parser(filename):

    filehandle=open(filename,'r')
    
    fasta_dict={}
    
    for x in filehandle:
    
        x=x.rstrip()
        
        if x.startswith(">"):
        
            heading=x.split(">")
            
            keys=heading[1]
            
        else:
        
            values=x
            
            fasta_dict[keys] = values
            
    filehandle.close()
    
    return fasta_dict
```
I wanted to modify it so that after "else" it would assign the first line to a variable 1 second to a variable 2: ```else: variable1, variable2 = x```. Not sure why I thought that it is going to automatically assign the first line to the first variable, second to the second one and start over with the third one. It did print out everything nice when just printing out ``` values```, but even when I used to ```x.split()``` to split all the lines in the end and thus create two variables (since I used something similar a while back) - does not work. Not sure why, but could be because the last time I used it the idea was to split one line into 2 and then assign those to variables (Vilde suggested this), here I already have all the lines separately... Using ```x[0 or 1 or 3]``` so maybe it then knows how to take the lines as separate units and I can just use the positions to assign them as values to a dictionary when I get around to doing it. Ofc did not work. It doesn't work with lines, but with lists. Then I tried to use the ```split``` again to get them as lists, but then they are not in one list, but all separate lists. Started many different files to understand what is going on, none worked and I still can't figure out how I could have easily gotten the part after "else" as separate units and didn't get much help on that so I guess it will remain a mistery. Then I scratched that and decided that I should just make them all into one list and thus I decided to do something like this:

```
def fasta_parser(filename):
    
    heading_list=[]
    
    seqtop_list=[]
    
    #seq_list=[]
    
    #top_list=[]
    
    filehandle=open(filename,'r')
   
    fasta_dict={}
    
    for x in filehandle:
    
        x=x.rstrip()
        
        if x.startswith(">"):
        
            heading=x.split(">")
            
            heading_list.append(heading[1])
            
        else:
        
            if len(x.strip()) != 0 : #add this since the last line in the file is an empty one.
            
                seqtop_list.append(x)
```
I saved and erased so many times that I am not sure if it was this one or not, but the main idea is again that I couldn't figure out how to make it work. Then I decided to try ```enumerate```, which a classmate suggested and was also written on the board, I guess I missed it earlier. I have used that before and figured, maybe it makes something clearer: it gives the position and value of an element. I couldn't think where will I use the positions, but I managed through trial and error to put together something like this:

```
def fasta_parser(filename):
    
    heading_list=[]
    
    seqtop_list=[]
    
    #seq_list=[]
    
    #top_list=[]
    
    for sitt1, sitt2 in enumerate (open(filename)):
    
        sitt2=sitt2.strip()
        
        #print (sitt1)
        
        if sitt2.startswith(">"):
        
            heading=sitt2.split(">")
            
            #print (heading)
            
            heading_list.append(heading[1])
            
        else:
        
            if len(sitt2.strip()) != 0 :
            
                seqtop_list.append(sitt2)
                
    #seq_list.append(seqtop_list[::2])
    
    #top_list.append(seqtop_list[1::2])
    
    dict1=dict(zip(heading_list, zip(seqtop_list[::2], seqtop_list[1::2]))) #found a similar thing in google, zips the keys and values and since I zip both values it kind of zips a zipped value as a value with the specified key.        
                
                #print(read)
    print (dict1)
    
    
```
This actually __worked__. The whole enumerate thing didn't change much though, not sure why this should be useful in this task, but got the job done here too... In the beginning I tried to assign all of them into one list and then separate the positions into two new separate lists as can be seen the comment # parts, this was useless if I could just put it all together in the eventual ```dict``` function. Then I found out that I actually do need the ">" in the beginning of the headings and tried messing with it to get it out and realized that if I don't even have to remove it and I just have to have lists for the dictionary and in lists I can specify positions as I did here then why go through all the trouble and not just write a function that takes the file, puts all the lines into a list, takes out the linebreaks and keeps out the last empty line of the file - this just means put everything into a list and then just make a dictionary out of it using the positions, since I know that position 0 is always the heading, 1 is always the sequence and 2 is always the topology. This pattern happens after every 3 instances. So I just wrote this: 

```
def fasta_parser(filename):

    
    list1=[line.rstrip() for line in (open(filename,'r')) if len(line.strip()) != 0]
    
    dict1=dict(zip(list1[::3],  zip(list1[1::3],list1[2::3])))
    
    #for key in dict1:
    
        #seq, topol = dict1[key]
        
        #assert len(seq) == len(topol) -----for testing if it's right, if the lengths are the same, the chances are it is right.
    
    return (dict1)

if __name__ == "__main__": 
    print(fasta_parser("8_state_smallerset.3line.txt"))

```
As far as I am concerned it also __works and seems simpler__. ~5h = 3 lines.

# Monday 19.02.17

Wrote my bash script for the project and uploaded it to my MTLS repository: 

```
usage: bash folder.sh and follow instructions

echo "Enter the name for your project's main directory:"

read name

mkdir "$name"

cd "$name" 

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

mkdir prediction_results
```

I wanted to make something basic, but this will probably need a few modifications. When running the script you have to enter
bash folder.sh and then it asks me for the name I want to use for my project's main directory.



# Sunday 18.02.17

Went through the [tutorial](http://swcarpentry.github.io/shell-novice/) for bash.



# Saturday 17.02.17
Spent the whole morning still trying to figure out why this page only publishes my README file. Then changed the name of the file I want to be published to "index" I don't know what happened, but it worked. I realized after messing around for an hour I can not format the text in the way I had planned with starting some lines with indendations.
Used ssh to log into the school's computer and edit the index file. Got a problem using nano, it would not let me save the addition. Now trying to use emacs.



# Friday 16.02.17
Spent the whole morning and evening trying to figure out why the diary does not publish from my master branch, google did not help.



# Thursday 15.02.17
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
