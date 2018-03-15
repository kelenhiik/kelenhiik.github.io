__Important update: tried to organize, which means I transferred all the random sequence files that I use for testing and training and code-writing are now in data folders. I haven't changed the paths to files in the python scripts, BUT usually the one's that are important, as in PSSM profiles, those are usually in the right path, but for random tests it needs a bit of digging. The model trainer dumper and predictor have been also changed to use pickle, but there the paths have been corrected__

# Thursday 15.03.18

Still made a parser ```parse_pdb.py``` that is going to take in a dssp assigned pdb file and writes out the name of the protein, the amino acid and the structure of it. Was a real struggle since the dssp file looks like hell and when getting columns from it using genfromtxt, it replaces all unknown values with the first value it sees, so instead of 'unknown structure' or basically 'DSSP C-state' is any character....SO I had to write out something that sayd if the values in these columns are not part of any known state, assign them as 'C'. It worked, I had the perfect file...until I realized some of the proteins are superweird and the aminoacid sequences in the DSSP output have ```!*```. DID EVERYTHING, but could not find out how to remove these files, since if google doesn't display the answer to what '!' is, then I will never find it. I had pulled 50 proteins out of PDB and a lot had the ! in there, so I pulled even more proteins,(using ```pdb.py``` around 100 from the pdb dataset I took from PISCES ```cullpdb_pc25_res1.6_R0.25_d180308_chains4177```(the resolution and percent identity cutoffs: 1,6 and 25%. R-factor cut-off 0,25 and only XRAY structures).

For these ~100 proteins I use my ```dssp.sh``` script to get the dssp files. Then I used a one-time code, which I did not save to give me all the names of the proteins that have a ```!*``` somewhere in their sequence column in the dssp files and then manually deleted these names from the list in my ```parse_pdb.py``` script. This script then takes all the dssp files with the protein names, looks at the sequence column and the topology column. Since ```genfromtxt``` uses any amount of whitespaces for delimiting and messes everything up if I manually tried to assign the delimiter as \t\ or a number of whitspaces, it just gives the secondary structure column with all the unknown spaces filled with whatever it can find next. This means I brutally told it to change every element that is not part of the 8-states to be assigned as a 'C'. THEN I had it write the protein name, sequence and structure to  ``` dataset_of_50.txt ```. The shortest sequence from DSSP has 51 residues and the longest has 759. Then I put the file into ```data/testing.sets/```.

What nice productive 2 days of my life. __To everyone else, who asked how to get the sequences, JUST READ THIS ENTRY and I can also explain in person. However, if this was the wrong approach then sry__
__update: if you don't want to use this time-consuming-pointless-method, I heard dmtri also has some kind of a code for it__

# Wednesday 14.03.18

Good thing no one kicks us out of the lab after 18:00, since I keep staying after 21. So today I made scripts to run cross-validations on different parameters using the PSSMs (all the results are in results/testing_results/), also ran all cross vals on the regular sequences AGAIN. Also made a confusion matrix and some other predictor scores on everything. Still don't know how to understand them or plot them. Is going to be a nice final report. But now since we know that the grades are only 40% of the summer fellowship thing AND they expect us to have relevant previous experience, so I'm already screwed, then I guess I will be happy if I just pass and maybe I will not even include all of this in the report, because I don't have time to teach myself what all of this means and how to implement different scores and to make sense of the data, since this is a all a completely new field and no one really explains anything useful here. Also did the matthew scores for everything, but then Shuhan figured out that it's only applicable if you have 2 states, thankfully I was blessed with the 8-states, good thing no one else told us this when we were running them. 

On the same note, we have to retrieve 50 sequences from some other dataset for the additional assignments. Easier said than done. Spent the first part of the day trying to figure out how to get them. Didn't find anything, it's not like google just has them lying around in a usable format. Found a bunch with 3 states - but yet again, I have 8-states, so whatever. I had a breakdown and left school and slept for a few hours, then came back and stayed here for the remainder of the day following steps from articles that use 8-states datasets. So retrieved a bunch of low seq identity protein names from PISCES, tried to get them from PDB, which meant I had to change the file, to make the names into a nice format. Then I found out that I can't download the .pdb files from the PDB webpage, did some googling, and tried to do it from the terminal. It worked, I have a file in the /bin/ folder called /PDB/ that has all these files. Then I tried to read up on using DSSP on the terminal, did that aswell, now I have the DSSP files of all those PDB things also. Then I realised that DSSP assigns a " " for unknown structure, but I have C for that. Genfromtxt is not going to help on that one. Then I was told that I should not continue messing around with that, but rather try my googling skills again, which we all know by now are not competent enough. I mean why teach us relevant things we should actually use like PDB, DSSP from the terminal and getting the sequences, when we can just mindlessly google away with no results. 

I have done all the PSSM things and basically everything to be honest, I just don't know now how to get the 50 sequences and how to analyze the different scores I get, probably the most useful thing I need, but I guess I'll never figure it out, so I think I might opt for just sending in a very basic predictor and final report and just pass this (I don't think they can fail me if I have done everything even if it's crap - if any TA reads this, then would be nice if I'd get a confirmation on that), because this course is just too insane and I don't have the brains nor any motivation left for this whole masters really, at least the applications for masters back in Estonia will start soon, might apply for something more adequate for my abilities that doesn't result in people being in need of psychological intervention. Torture in the 21st century.


# Tuesday 13.03.18

Changed to pickle.
Made new scripts ```eight_state_ss_predictor.py``` this is the bone-structure of the final predictor with sequences,  and one for a PSSM input ```eight_state_ss_predictor_pssm.py```

Created another script ```randomize_dataset.py``` JUST SO I COULD HAVE A FILE WITH RANDOM 109 PROTEINS FROM THE BIG DATASET TO DO ALL THE CROSS-VALIDATING AND ACCURACY STUFF, SINCE I DON'T HAVE TIME ANYMORE. The file I have now is ```randomized109_proteins.3line.txt```.
Now I want to:

1) Split my dataset into 70% train and 30% test.

2) Train a linearSVC using cross_val_score function with different window-sizes and C-scores and class_weight 'balanced' or default.

3) Save the accuracies to a file and plot the outputs to be a separate plot for using class_weight_ 'balanced' model and default one, where for each C-score I would have a plot with window-sizes and their equivalent cross_val_scores. 

4) Do the same thing with random_forest_classifier and decision_trees.

5) Compare the best parameters and their corresponding cross_val_scores between the different classifiers. 

6) Make predictions using those parameters and construct a confusion matrix to see what it's actually predicting. If something is super messed up, go back and use other parameters for testing a confusion matrix construction.

7) If already using the prediction, take a MCC score also and the classification report, which should have all different fancy scores. When finding parameters with what I am happy with choose one model to continue with to step 6.

6) Use the same sets, BUT instead of the sequences in the 70% train-set, use their PSSM's and see if the score changes between the best parameter model with and without using PSSM.

7) survive

# Monday 12.03.18

David found the root of my problems, in the code, and John showed me how I can get bypass the pylint error where a line in my code was too long. I don't know what to do now with my life. I am not sure what the 50-proteins thing is. I am also not sure with the PSSM thing, that I guess we have to always create a PSSM profile for the protein being predicted?? Sounds weird though. Anyway, today I updated and added:
```
all_parsing_codes.py
train_SVM_and_predict_PSSM.py
pssm_oneprotein.py
```
So to __all_parsing_codes__ I added a code that takes in a PSSM profile, parses it into a proper format for sliding windows. Then another function for sliding windows that adds the padding and then a third one that returns all features and corresponding topologies as labels. Then a fourth one that splits that dataset to 70% training and 30% testing (I used this for scoring, BUT I WANT TO DO THE CROSS-VALIDATON AND SCORING STUFF and visualize them so I could use them in a proper format for the final report, but I DON'T know how.
__train_SVM_and_predict_PSSM.py__ Just for checking if it even works, and also for a quick check of the score with the 70% and 30% using 34 proteins, the regular model.score() was __0.559876625989__.
__pssm_oneprotein.py__ is like the last weeks assignment without saving a model. It trains on 70% of 11 proteins (quicker) and predicts on 2 proteins that are not in the set and prints it out in a file in the bin folder named "prediction_pssm.txt".

# Saturday-Sunday 10-11.03.18
__update__: I think I found it, I have a return statement in the wrong place and some empty lists assigned in the wrong place...
Well technically it is Sunday already, since its 03:30 AM, but I started on Saturday. I managed to get the PSSM thing working...I think. It's hard to follow when the print out is just numbers. I am not sure if having a mix of elements in a list is a good idea or not, I mean, some are integers and some floats? or some are stings and the other are floats? I don't know, but they look different. Anyway, I uploaded some stuff, I also ran it. The accuracy for random forest did not get better, so I have a feeling I did something wrong. Probably something stupid since the code does not look very nice and pylint is also not very appreciative of my efforts....

Added files: 
```
PSSM_parser.py
all_parsing_codes.py

```
I am not pushing everything in, since I need to have a fresh look at it tomorrow, eyes are already upside-down.

# Friday 9.03.18

At the airport, I'll see if I can figure out the parsing. It seems like it could be approached quite easily, just with multiple for-loops. I did try yesterday that with readlines, it takes the lines, then you can specify the positions, maybe just look at the file, skip the first and lsat lines and just manually read until the amino acid scores I want (/100) and there I have it, but I can't lose the sequence information also I guess and the ID. I did get a hint from David to check out numpy genfromtxt. I have to read the documentation. Marco suggested trying regular expression, but they seem a bit too advanced to just jump into. I don't really have a proper idea what they are. I know everyone in class is trying to book computers to run their scripts on, so it would be faster they want to use multiple computers. I am not planning on running anything big on the computer I use, but I do plan on ssh'ing it, so if it messes somebody's script up, then you have been warned about using my computer. It's not technically mine, but it is mine (computer 34)....


# Friday's assignment (I think this was actually thursday 8.03.18)
For prediction, the script is ```Repo_MTLS/8state_predictor/bin/trained_RFC_predictor.py```. Inside the script I have hardcoded two different input sequence files ( ```twoseq.txt``` and ```2protein.fasta```). They have the same proteins, but the first file is in the 3-line format (only using ID and sequence for prediction) the .fasta file is in a 2-line format (ID, seq)(also, fasta files are usually not a full sequence on one line, do I have to fix that in my code????). Both of these files are in the same bin folder, specified with a path.  Both work, but for specifying one you have to __follow the comments I made in the file__. The output of running the trained_RFC_predictor.py is a print-out on the screen of ID, seq, topology AND a Predicition.txt file in ```Repo_MTLS/8state_predictor/results/prediction_results/```. Right now I did not push that .txt file into the folder so you wouldn't think I hardcoded the output. Enjoy!

# Wednesday 7.03.18

```0.493881630949``` New for SVM, parameters: ```ovo rbf 19``` SO I am going to go with random forests, just to be sure I am running the validation again. I know the results are a bit biased because I am not splitting it manually and the function I am using splits them randomly, but I still get somekind of an idea what is the best. I ALSO understand that I might have used more folds for validating, but in consideration of the time, that is a task for another day...

__update__:
I did the random forest again with other windowsizes, best I got was 0.556475697657, which is smaller than before, so the model I am saving for this week's assignment will be trained RANDOM FOREST:
n_estimators1 (here it is 300), min_samples_split1 (6) , win_len (9), score (0.559109750669). Other parameters there are n_jobs=-1, class_weight='balanced'. To be honest, I know it is different for the guy who did it last year, but I don't know, these are my results...

The results for random forest accuracies: ```Repo_MTLS/8state_predictor/bin/randomforest.txt```, ```Repo_MTLS/8state_predictor/bin/randomforest_second_set_of_ws.txt```

Decision trees: ```Repo_MTLS/8state_predictor/bin/decisiontree_result.txt```

SVM results: ```Repo_MTLS/8state_predictor/bin/SVM_result.txt```

I KNOW MY FOLDERS ARE A MESS, I am just trying to survive, I'll make them better when I have the time....

__assignment__: 
I am just writing this here for now so I would not forget it, but I will upload it again on friday, so it would be the first post.

So I trained a model using Random forests as classifier. The pythn script I used for training is ```Repo_MTLS/8state_predictor/bin/Randomforest_model_trainer.py``` for training it uses a file ```Repo_MTLS/8state_predictor/data/training_sets/10proteins.3line.txt```, well the location for the file in the script is ```../data/training_sets/10proteins.3line.txt``` since you are running the script from the bin folder and for the script the location of the training file is the that. The parameters I used were just number of estimators (300), minimum samples split (6), which I got from the 3-fold cross-validation, but I had to use a window-size of 7, so I could upload it here without it being to big (although I got a warning ```remote: warning: File 8state_predictor/src/small_models/RFC_predictor_smallmodel.pkl is 91.07 MB; this is larger than GitHub's recommended maximum file size of 50.00 MB
```). Then it dumps the model to ```../src/small_models/RFC_predictor_smallmodel.pkl```, basically again the ```../``` takes you to the ```Repo_MTLS/8state_predictor/``` since the script is in the bin folder. ANYWAY for predicting something, the script is ```Repo_MTLS/8state_predictor/bin/trained_RFC_predictor.py```. Inside the script I have hardcoded an input sequence file in the same bin folder called ```twoseq.txt```. It has two proteins in it and it is a 3-line text, but I am only using the ID and the sequence, however, if you open the file you can also use the same proteins in a .fasta format (```2protein.fasta```) in the same bin folder, where I removed the secondary structure. Both work, but then you have to follow the comments I made in the file. The output of running the trained_RFC_predictor.py is a print-out on the screen of ID, seq, topology AND a Predicition.txt file in ```Repo_MTLS/8state_predictor/results/prediction_results/```. Right now I did not push that .txt file into the folder so you wouldn't think I hardcoded the output. Enjoy!

# Tuesday 6.03.08

Okay, something weird is going on... The whole thing for SVM, the for-loops to try different parameters, has still not finished. I made a code to find the highest value in the output I have right now. Well, it is still very manual, you have to copy the print out yourself and whatnot, since I didn't create an output file. BUT anyway, the highest value I have turns up two times in the file, at the moment it is ```0.488868580997```, only difference is parameter ```'ovo' or 'ovr'```. I also found that a value ```0.383560879535``` turns up 80 times with different parameters. This seems super odd... But I doubt that tweaking even more parameters, is ever going to take the value over the 0.50 limit, that came pretty easy with random forests. SO I am thinking of going for random forest and read more about the parameters to change perhaps.


__update__:
Actually, I am not even sure if I should be doing what I am doing, since I should try and run it with the PSSM profiles also...And that is going to change the accuracies anyway. AND I am only running it on 34 proteins and I heard there is some kind of bias with this. Maybe I should try and see if I can run it with PSSM's and then mess with the scores, since I have to do it anyay, right? So 

1) Try and make it work with PSSM's
2) Try different parameters (now I kind of know in which direction to look at) using sequence information and PSSM information? Maybe?
3) Get the best parameters
4) Train models for different input with the specified parameters
5) Save models
6) Make a script that makes the whole thing interactive and calls the models???
7) Finish????

```https://www.biostars.org/p/14253/``` - maybe need to use this.

__update__:

SVM finished:
```
[Parallel(n_jobs=-1)]: Done   3 out of   3 | elapsed:  6.5min finished
10 ovr rbf 19 0.493881630949
```
Highest c-score, and highest window length, going to run it again with larger windowsizes....

```
Warning: [psiblast] lcl|Query_1 D1GL1: Warning: One or more U or O characters replaced by X for alignment score calculations at positions 662, 691, 765, 776 
Warning: [psiblast] lcl|Query_1 D1GL1: Warning: Composition-based score adjustment conditioned on sequence properties and unconditional composition-based score adjustment is not supported with PSSMs, resetting to default value of standard composition-based statistics Warning: One or more U or O characters replaced by X for alignment score calculations at positions 662, 691, 765, 776 
Warning: [psiblast] lcl|Query_1 D1GL1: Warning: One or more U or O characters replaced by X for alignment score calculations at positions 662, 691, 765, 776 
Warning: [psiblast] lcl|Query_1 D1GN2: Warning: Composition-based score adjustment conditioned on sequence properties and unconditional composition-based score adjustment is not supported with PSSMs, resetting to default value of standard composition-based statistics 
Warning: [psiblast] lcl|Query_1 D1GNQ: Warning: Composition-based score adjustment conditioned on sequence properties and unconditional composition-based score adjustment is not supported with PSSMs, resetting to default value of standard composition-based statistics 
```
Should I have somehow changed the sequences in my fastas??
OH yeah, so I managed to get the PSSM profiles.

# Monday 5.03.18

Today I started to create for-loops to try different parameters in SVM, Randomforest and Decision trees. I just asked around and googled and read some articles to figure which parameters to use. I am only using 34 proteins from my dataset of over 300, but it is still taking forever with SVM. Randomforest and Decision trees were faster. I will push the files into github also. Probably under ```Repo_MTLS/8state_predictor/bin/``` with filenames ```training.py, rfc.py, decisiontree.py``` and I also saved the results in txt files ``` decisiontree_result.txt, randomforest.txt ``` and then I made a weird function to visually sort the highest score in ``` koodid ```, but it just prints out the sorted values from the txt file I give it and it sorts from the smallest andthe last values are random comments I made in the txt file, where I specify what each position means. SO to understand how to use it I just print the values, take the last numerical value it gives me, that is the highest and then go to the txt file and search for that number to know the paramteres....complicated, yes, but life is not meant to be easy.
For random forest the best cross-validation score is: ```[Parallel(n_jobs=-1)]: Done   3 out of   3 | elapsed:   16.6s finished
300 6 9 0.559109750669``` 0.559109750669. The positions are RANDOM FOREST:
n_estimators1 (here it is 300), min_samples_split1 (6) , win_len (9), score (0.559109750669). Other parameters there are n_jobs=-1, class_weight='balanced'
score = cross_val_score(model, X_train, Y_train, cv=3, verbose=True, n_jobs=-1)

For Decision trees:

1st pos = min_samples_split (here it is 2)

2nd pos = win length (here it is 9)

3d pos = 3fold cv score (here it is .425225871702)

``` 
[Parallel(n_jobs=-1)]: Done   3 out of   3 | elapsed:    4.1s finished
2 9 0.425225871702 
```
So best score is: 0.425225871702. This was however with the first input I had, the smallest windowsize I used, which was 9. I'll try smaller sizes also, but ptobably will not change. 

Regular SVM is still running....




# Friday 02.03.18

I am messing around in my home computer trying to change the code so I could just use one sequence to predict the structure to. I am not sure what I did, but at one point none of my codes worked, so I am not sure if I will push this new one in, because I maybe might have changed my good and working code also and I am not sure in which format it was working... Since I wanted to harcode a sequence I originally made another function that would encode my string sequence, but I kept getting errors in my original all_parsing_codes file, so I just undid all my modifications and decided if I want to mess something up (I just figured that the errors were because I am not using my function to parse the sequence, since it is not in the file, so I have to manually add in that I want all the letters uppercase and this was the problem all along....), then I am going to do that in another file, so I created another file, where I pretty much hardcode the sequence I want to predict the topology to. I call my good ol' functions from my all_parsing_codes file + add a few mods. It is actually predicting something!

__update__:
Okay so, TAs, you have a choice, either follow the thing I wrote yesterday OR try out the new file: ```Repo_MTLS/8state_predictor/bin/train_SVM_oneprotein_predict.py``` AND it should do what you wanted it to, I also printed out what result I got when I was running it, that's in the same file as a comment. Also try and figure out in which comment I have a grammatical mistake, since I am not going to log in again to change one letter in that file! (if I have more mistakes then I have no excuses)


# Thursday 01.03.18

__Mandatory week assignment__:
This weeks assignment is to upload a code that creates an input for SVM and check if the SVM is capable of predicting a label for just a feature. I am not sure what the assignment description means by one sequences...So I am still using multiple ones. I uploaded a code called ```train_SVM_and_predict.py``` in my ```Repo_MTLS/8state_predictor/bin/```. It takes in a smaller file I made with 34 proteins, trains on 70% of that and predicts the labels for the other 30%. I also made it to save the sequence into a txt file, absolutely not necessary, but the file is called ```prediction.txt```, also available in the bin folder. I think this is what was meant by the week assignment, to have a working code that creates an input to the SVM and that the SVM would also be able to create and output. 


Writing an entry to this diary since my dataset is doing the 3-fold cross-validation with different window sizes. It is taking forever, even though I minimized my dataset like 10 x, so it is only running on 34 sequences. I used a code Kajetan showed me that is a built-in validating code. I made a foor loop and it's looping through odd-number window-sizes from range 3-32, also just something Kajetan suggested to use. The only parameter in addition to windowsizes I defined is cache size 3000, so it would be a bit faster, but I guess ideally I should make additional for loops to start changing all the parameters: for loop for kernels in a for loop that checks window sizes in a foor loop that checks whatever else parameters I think are necessary to change. Since it is taking so long and I am only using my 34 protein dataset I am thinking that maybe I should just use the leave-one-out validation thing, or the thing that just takes the 70% for training and 30% for testing and then make that into a for loop for different parameters and after this thing has stopped running I guess I have some idea what the range of window sizes I want to check are going to be....OR I can just do the 3 fold cross-validation again, just this time I also specify a smaller range of window sizes when checking the other parameters in for loops.


__Update on the windowsizes__:

Felt like I need to copy the print out __while__ it was running, so I could already create a table or paste it here or something. So there I am, highlighting the output and pressing ctrl+C...only to realize...that I just killed the whole thing. I think it doesnt matter though, since the accuracy was already going down.
This was what I managed to salvage:
```
[Parallel(n_jobs=1)]: Done   3 out of   3 | elapsed:  4.4min finished
3 0.42845398133 #first number is the windowsize, second is the average score of 3-fold validation.
[Parallel(n_jobs=1)]: Done   3 out of   3 | elapsed:  6.3min finished
5 0.440179328772
[Parallel(n_jobs=1)]: Done   3 out of   3 | elapsed:  8.2min finished
7 0.456777283344
[Parallel(n_jobs=1)]: Done   3 out of   3 | elapsed: 10.1min finished
9 0.462498947373
[Parallel(n_jobs=1)]: Done   3 out of   3 | elapsed: 12.0min finished
11 0.467738627014
[Parallel(n_jobs=1)]: Done   3 out of   3 | elapsed: 15.1min finished
13 0.474082901137
[Parallel(n_jobs=1)]: Done   3 out of   3 | elapsed: 16.5min finished
15 0.470344819212
[Parallel(n_jobs=1)]: Done   3 out of   3 | elapsed: 17.9min finished
17 0.471930738482
[Parallel(n_jobs=1)]: Done   3 out of   3 | elapsed: 19.7min finished
19 0.467653670019
[Parallel(n_jobs=1)]: Done   3 out of   3 | elapsed: 21.6min finished
21 0.466435675239
[Parallel(n_jobs=1)]: Done   3 out of   3 | elapsed: 24.0min finished
23 0.461054455209
[Parallel(n_jobs=1)]: Done   3 out of   3 | elapsed: 25.8min finished
25 0.458901161453
```

So I guess I could go for it again with other variables and windowsizes between 9-23..although that seems like it will take forever, maybe then just the one's that cross the 47 mark, so 13-17. We'll see.


# Wednesday 28.02.18

Today did not manage to do a lot of coding. I tried to predict a secondary structure using an input where I deleted the positions of the secondary structure, so the whole third line. Then I made a separate parser to make a dictionary that takes those in and makes an SVM input array of the unknown structure sequences. The order of the sequences will probably change if I predict the same file again many times, or I just have to sort the dictionary before hand or something, BUT I guess it doesn't matter, because it did not work. What did work however was when I trained it with my training set and predicted my testing sets topology to see if it predicts something. I had an output, so I guess it basically works, but for some reason did not work on the file, where I erased the secondary structure lines....Also the article chosen for me. It is super hard to understand what they actually did, it's like a big paper written about nothing... Going to be a hard task to find something relevant from this. The only connection to my project is that it is an alternative solution to finding 8 state secondary structures, as described by DSSP.


# Tuesday 27.02.18

So I did not manage to find ou how to concatenate the lists on the level I wanted in the sliding windows I introduced yesterday, but I did the whole thing again, a completely different code that now makes a lot of sense, but I also had a one-on-one explanation on it so I did not come up with this myself. Also I feel like my brain's computing capacity has reached it's limit so I am having difficulties in understanding very basic ideas and pretty much everything around me. But now that the code is working I was able to (with tons of help) to actually create an input for the SVM. So I can get the sliding window features and the corresponding topologies. Make numpy arrays out of them and call them into the SVM. I was also shown how to split my data into training and testing sets (and we even tried to make it less biased, since using dictionarys is a big pain and you can't lose your order so you have to keep looping and doing magic, so I am pretty happy with the splitting we made today. I tried to run it again to generate the model and score it, but it has been running for hours now (4h) and it's long after 6 PM  so I will get kicked out soon. I guess it's a problem for another day. Might even get some sleep today knowin that the toughest part is behind (hopefully). Also if the TAs are reading this, thank you for making an extra help-session day!!!


# Monday 26.02.18 
# Helen 3:0 Whiteboard PART II

```
aminoacids='GALMFWKQESPVICYHRNDT'
seq1=['GQAML']
ws=3
ws2=int(ws//2)


def vect(seq):
    global listAA
    listAA = []
    for residues in seq:
        vector1= [0]*20
        pla=aminoacids.index(residues)
        vector1[pla]=1
        listAA.append(vector1)
    
def slidingwindow(seq):
    vector2 = [[0]*20]*ws2
    seq_ws=vector2+listAA+vector2
    windows2=[]
    list_of_sws=[]
    
    for binary_code_list_position in range(0,len(seq_ws)):
        if binary_code_list_position + ws <= len(seq_ws):
            windows2.append(seq_ws[binary_code_list_position:binary_code_list_position + ws])
    print(windows2)
    
    #  HOW TO GET EACH SLIDING WINDOW IN A FLAT SEPARATE LIST???????
    
    #for lists_of_sliding_window_lists in windows2:
        #print (lists_of_sliding_window_lists)
        
 [[0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], 
 [1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], 
 [0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]]
        
[[1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], 
[0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], 
[0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]]

[[0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], 
[0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], 
[0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]]

[[0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], 
[0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], 
[0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]]

[[0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], 
[0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], 
[0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]]

```
In an ideal world these are the lists for the sliding windows, that should be continuous inside their little lists, but how???

```
        
        
        #for lists_inside in lists_of_sliding_window_lists:
            #print(lists_inside)
            
[0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
[1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
[0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
[1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
[0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
[0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
[0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
[0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
[0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
[0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
[0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
[0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
[0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
[0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
[0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]           
    
```
And here, if I go one for-loop deeper I get that. So I mean in my opinion, it would be great if I could tell the computer to take lines 0,1,2 and add them as a list and then append on top of that the next triplet (sliding window) so I would have a list like this:

```
 [[0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 
   1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 
   0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
        
  [1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 
   0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 
   0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]

  [0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 
   0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 
   0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]

  [0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 
   0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 
   0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]

  [0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 
   0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 
   0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]]
```
Or that is what I THINK it should look like....I mean either I could get the first example to join the lists inside the nested list, at least on the desired level and in the second example as said before, to join lists that correspond to sliding window size.


# Monday 26.02.18 
# Helen 3:0 Whiteboard PART I

I gave up on the sliding window I was trying to make, since I couldn't get it to work and then I couldn't understand my code anymore. I tried arrays now, and with a lot of help I finally semi-understood how they work. Yet, when I tried to implement what we did in class on the whiteboard..I am not sure what to make out of the result:

```
aminoacids='GALMFWKQESPVICYHRNDT'
seq1=['GQAML']
ws=3
ws2=int(ws//2)

def vect(seq):
    global listAA
    listAA = []
    for residues in seq:
        vector1= [0]*20
        pla=aminoacids.index(residues)
        vector1[pla]=1
        listAA.append(vector1)
        
    return listAA 
    #nested list where first list is the 20 position sequence for a specific residue.

def slidingwindow(seq):
    vector2 = [[0]*20]*ws2     #emptyvector full of zeros size 20*length of sliding 
    window//2, so if sw is 3 I have 1 list full of 20 empty zeros. I think I want 
    to add this to the ends and beginnings of my window.
    
    start_finish=[]

       
    for i in range (1, len (seq)): 
    #for index in range 1,2,3,4..,len of seq, should print numbers
        
        start_finish.append(listAA[i-ws2:i+ws2+1]) 
        #whatever position i is, it substracts the ws//2 : extends until i+ws//2+1

        
       
        
    print (start_finish)

for i in seq1:
    vect(i)
    slidingwindow(i)
   
The print out of this is: 

[[[1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], 
[0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], 
[0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]], 

[[0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], 
[0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], 
[0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]], 

[[0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], 
[0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], 
[0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]], 

[[0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], 
[0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]]]

And this is basically [[[G],[Q],[A]], [[Q][A][M]], [[A],[M],[L]], [[M],[L]]] 
so a sliding window yes, ends when the sequence ends. 
Now I should add the vector2 I made to the beginning and the end of this...
Tried it in many many ways, but it never came out the right way. 
I probably have to have a bunch of if statements, but I had to take 
a break exactly at the point the TAs discussed this. 
Although I am not sure what to think of the fact that in the end it has two residues...
should not be correct like this. 
```
BUT if I do it in a way that we were told not to, I manage to get the positions in zeros in the beginning and end:

```
aminoacids='GALMFWKQESPVICYHRNDT'
seq1=['GQAML']
ws=3
ws2=int(ws//2)

def vect(seq):
    global listAA
    listAA = []
    for residues in seq:
        vector1= [0]*20
        pla=aminoacids.index(residues)
        vector1[pla]=1
        listAA.append(vector1)
  
    return listAA 

def slidingwindow(seq):
    vector2 = [[0]*20]*ws2
    seq_ws=vector2+listAA+vector2
    
       
        
    print (seq_ws)

for i in seq1:
    vect(i)
    slidingwindow(i)

Print-out: 
[[0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], 
[1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], 
[0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], 
[0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], 
[0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], 
[0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], 
[0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]]

```

So this is exactly what I imagined it should look like in my head. That whatever sequence or window size I have, it takes the sequence and adds a vector full of zeros (empty spaces) to the end and the beginning in the correct (ws//2) amount. Thus all I have to do next is to make it print out a sliding window appropriate to the size using the flanking regions - features are the sliding windows specifying the middle residue.

# Saturday 24.02.18 (ESTONIA 100) and Sunday 25.02.18

On Saturday messed around with my parser and sliding windows, tried to put the functions together, but could not figure out how to only get the first value of my keys in the dictionary since I have assigned a key to both the topology and the sequence and for the sliding window I only wanted the sequence. Since it's quite uncomfortable to use the ssh and emacs it takes 10x the time to figure out what is going on. At some point gave up and did not commit anything to the reporistory because it was useless. On Sunday started the fun again, using the ssh I can't see the full print out of the sequences since if it goes out of the window then in tmux I don't know how to scroll the ouput so I could see the whole thing, so I decided to create an even smaller file 
shortseq.txt. I wanted to copy a few lines from another file to that one. I could have created some function that copies the last lines to a new file or sth, but it seemes like it will take too long until I figure out how to do that so I thought maybe just open the file I want to copy from in emacs and just copy the lines I want to a new text file I open in emacs. Seemed like a great plan - it was not. Managed to mess up emacs so many times that I had to close the command promp on my computer because I did not know how to get out of the thing. SO I created a bunch of text files to which I could not copy anything. Decided I will just make the file here, on github and pull it in. Did that: copied the lines into a txt file and pulled it in, seemed to work. Continued to edit the  sliding_window_try file so I could get the sequence value from my dictionary to a new list so I could specifically work on the sliding window on those sequences. It was weird because a lot of times when I printed the order was super messed up and I could understand what is going on - I have a hunch that it has something to do with the dictionary, that it has a weird order so when I do the for loop then the values taken from the dictionary are not in the right order. BUT FINALLY figured out how to get the first value, so the sequence value and appended that to a list. Not sure how I feel about the idea that I spent half of my life to make the whole thing into a dictionary just to go back to make them into lists - what a waste of time. Anyway, what I came up with so far is something like this: (I erased a few pointless comments)

```
def fasta_parser(filename):
    list1=[line.rstrip() for line in (open (filename, 'r')) if len (line.strip()) != 0]
    dict1=dict(zip(list1[::3], zip (list1[1::3], list1[2::3])))
    return (dict1)

def slidw1(seq,sw):

    window=[]
    all_windows=[]
    aa_list=[]
    aa_=[]
    #print (seq)
    #prints dictionary
   
    for ID in seq:
       # print (ID)
        #prints the dictionary keys aka ID
       
        aa_list.append((seq[ID])[0])
    print(aa_list)    
    #prints the dictionary's first values, so ID value in position 0 
    
     
    pos_in_window=list(range(0,len(aa_list)))
    
    #print(pos_in_window)#---[0, 1,] position of sequences in list
    
    for element in aa_list:
    
        print (element) #this just prints the sequences, because 
        I guess it takes the element as the thing that fills the positions 0 and 1 that i have.... 
        
```

So I am able to put the file into a dictionary and now finally managed to understand how to get around in the dictionary. Not sure if I am feeling the whole dinctionary thing since it messes up the order of the key:value pairs so every time I printed something it was in a weird order and I erased my code many times and spent a lot of time, since I thought that I am doing something wrong and I hope I didn't have any good codes there somewhere in between that I erased... Now I am not sure what to do, the sliding window code that I made takes in a sequence where all the elements are separated, thus I guess I have to make another for-loop somewhere that would get all the elements separately and maybe put them into another list? I don't know, wish we had any guidance so I would not waste time making the same mistakes over and over again since I do not understand why something is wrong and I just fix it through trial and error - not a very sustainable approach for someone who does not know a thing about coding, but that is the whole theme for the course. 

I keep seeing arrays in my dreams and I can't make out why, is there like a very easy way to do all of this from the begginning of separating the lines in the original file until the end for SVM input using arrays??? Probably for the binary codes or I don't even know. If only I had a deep understanding how arrays can be used, but learning that myself googleing through tons of information (most of which is useless) is a waste of precious project time, so I guess I will never use them.

NOW COMES THE FUN PART: Anyway, I think the code is doing what I want it to do, but I have yet to figure out what it is. I did however change it a million times because I kept getting the output wrong to what I was expecting it to be. Why? Well, I realized that in the beginning when I made the file on github to pull it to the computer so I could access it through ssh, I copied the sequences from the command prompt, but the command prompt breaks the lines and fills them with spaces. I did remove the spaces from the ends, BUT I did not realize that there is still a page break thing in the end of the lines, so when my first lines of code take the file and make lists from list positions 0,1,2 then it just keeps out some of the valuable lines...However maybe that is not the logic behind it, because it should also then have made a key:value pair of random sequences.... I can't figure it out using the ssh, the emax, tmux and github things they are way too complicated and time consuming, I think I'll just wait for the lab to try it out, since I have to be in the airport 4AM tonight anyway then I can be in the lab 8AM sharp.

I guess the idea of this project diary is just to mention with a few words the progress we have made, but since I don't really make any progress I find that the only way to motivate myself to write this thing is when I can write it in a super informal way. ALSO there are absolutely no guidelines on how to do anything in this course so until I am told to do it differently, these are going to be my entries. Last finsihing notes for the next few hours (since I will probably have lunch and then try to do this whole stuff again or something else), in addition to the generic signs of overworking, I am losing my hair.


# Friday 23.02.18

Just quickly writing up stuff so I wouldn't forget anything. The dataframe is useless, I don't know why I wasted time on it. For the sliding window, add in to the beginning and the end of the sequnce the place-holders. Empty places that in the "binary alphabet" would have a sequence of 20 zeros so they wouldn't have any weight, but important so the window to take in the first position of the sequence also (sliding window looks at the middle position). Remember to look at the pictures in my notebook. I have to split my dictionary to test and train. Test is also split to validation. Read about confusion matrix. Download pycharm?


# Thursday 22.02.18


Yesterday night remembered something about a dataframe mentioned in class. Was not sure what it was, but it had something to do with the parser. Maybe. I didn't want to forget it so I tried to access the computer via ssh again and made a new python script with the parser function and then tried to make a dataframe out of the dictionary. Used the fancier screen version tmux so I could modify the script in emacs  (very complicated, backspace doesn't work, but gives the help screen and messes everything up, should probably try vim, since nano does not let me save while using ssh) in one window and then run the script in the other one. Did not get much done though. For today in class I continued with the dataframe. Fortunately it is much more comfortable to do it in class on the actual computer and I managed to get an output from the script. I have yet to figure out if I even need it, but at least I managed to do something, which is already more than usual. Also read a few articles to choose five from for the presentation. I am not sure if I even have the ability to make the presentation let alone present it when trying to survive the SVM and actual project part. Also tried to think of a project plan - did not work.

```

import pandas as pd 
import numpy as np


def fasta_parser(filename):
    
    list1=[line.rstrip() for line in (open(filename,'r')) if len(line.strip()) != 0]
    dict1=dict(zip(list1[::3],  zip(list1[1::3],list1[2::3])))
    
    
    
    return (dict1)

frame=pd.DataFrame.from_dict(fasta_parser("8_state_smallerset.3line.txt"))
print (frame)

```

I hope I need it somewhere in my life or else it was just like a regular day in terms of progress and reading. ~5h. ALso need to remember to figure out how to and why to use the onehotencoder.


# Wednesday 21.02.18

In class we went through the basic idea of the project. Most of it I had already figured by reading the provided material and articles, but these are just the very very basic ideas. What I still do not know is how to do all of it. I keep going through the assignments for the last course and google stuff, but nothing really klicks with my mind so not really getting anywhere with this. I tried to create something that would be a "sliding window" without hard coding it. The only thing I could come up with meant that I needed a function that takes in a sequence and a value for the sliding window and then prints out a list where the whole sequence is turned into sliding window sequences. I am not even sure if this was the point or if I do really understand the concept, but sth like this: if the seq is "ADERRG" and sw = 3, then I would get ['A'D'E'+'D'E'R'+'E'R'R'+'R'R'G']. So I wanted a for loop to go over the sequence starting from position 0 until the position of the sliding window value (that position itself is not included, which is also not needed, since it takes positions from 0 not 1) and then move on one step at a time. I tried creating a list out of the sequence so I could get the positions for them in the for loop - I did this also by using a for loop, because I couldn't think of anything else. Then I also realised that I need another list that the main for-loop can go through to get the position-number I want, so I just created a variable that makes a list out of the range of numbers in the length of the input sequence. Then I made the main for loop that I wanted that takes the numbers from the variable I just made and also uses the information about the sliding window size: essentially it puts stuff into an empty list I made earlier and that stuff is a number from the list of numbers I made that specifies a position in the sequence list I made and then extend those positions/numbers until it reaches a position/number that is specified by the sliding window value (and onto that I had to add the number that was used from the list of numbers, so the start and the end would always be fixed in relation to the sliding window value). It kept going over the actual sequence, in the sense that when it reached the end of the sequence it still continued, but the "windows" it created were not the right size, thus I figured I have to tell it that it should not go over the length of the actual input sequence with an if clause after the loop. The whole thing in the end looked like this:

__"ADERRG" is the seq and sw = 3__

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
        # if sliding window size and the number from pos_window sum up 
        to be less or equal to the length of the sequence, so it wouldn't 
        continue going over the sequence creating lists smaller than the window size. 
        
            all_windows.extend (window[number:(number+sw)])
            #adds all the values in window list that correspond to the positions 
            from and between number and sw+number (where it starts the value and 
            goes one by one, so does the sliding window shift as much) into one continous list.
         
    
    print (all_windows) #----['A', 'D', 'E', 'D', 'E', 'R', 'E', 'R', 'R', 'R', 'R', 'G'] 
    this maybe works, but might be too complicated and mby does not work for larger data??
    
```



I have no idea if this is what I should do or if it actually works and I have a feeling it is not really that productive with the lists, iterations and appending and extending stuff to the list. ~6h.


# Tuesday 20.02.18

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
    
    dict1=dict(zip(heading_list, zip(seqtop_list[::2], seqtop_list[1::2])))
    #found a similar thing in google, zips the keys and values and since I 
    zip both values it kind of zips a zipped value as a value with the specified key.        
                
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
        
        #assert len(seq) == len(topol) 
        -----for testing if it's right, if the lengths are the same, 
        the chances are it is right.
    
    return (dict1)

if __name__ == "__main__": 
    print(fasta_parser("8_state_smallerset.3line.txt"))

```
As far as I am concerned it also __works and seems simpler__. ~5h = 3 lines.

# Monday 19.02.18

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



# Sunday 18.02.18

Went through the [tutorial](http://swcarpentry.github.io/shell-novice/) for bash.



# Saturday 17.02.18
Spent the whole morning still trying to figure out why this page only publishes my README file. Then changed the name of the file I want to be published to "index" I don't know what happened, but it worked. I realized after messing around for an hour I can not format the text in the way I had planned with starting some lines with indendations.
Used ssh to log into the school's computer and edit the index file. Got a problem using nano, it would not let me save the addition. Now trying to use emacs.



# Friday 16.02.18
Spent the whole morning and evening trying to figure out why the diary does not publish from my master branch, google did not help.



# Thursday 15.02.18
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
