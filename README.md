How To Train Data on both local and Raspberry Pie
First, follow the https://ucsd-ecemae-148.github.io/markdown-documentation/50-Virtual-Machine-with-DonkeyCar-AI-Simulator/Donkeysim-on-an-ARM-Chip-Mac/
And then you should have the conda environment
you need to git clone this repo to your local computer
$conda activate donkey
cd to your working folder
$mkdir data

then open a new terminal, ssh to raspberry pie
cd to your working folder, ucsdrobocar-148-12
everytime you want to train new data, run
$rm -rf data //remove the old data
$mkdir data  //create the data folder

And you are ready to record new data.
Now, you will need to know your external internet ip first
on mac use 
$ipconfig getifaddr en0
and you will get xxx.xxx.xxx.xxx
if you get 127.0.0.1, there is something wrong.

then back to pie, in ucsdrobocar-148-12 folder, run 
$python manage.py drive
in your local computer, run
$ http://<ip address you get before>:8887


when you finish collecting data, you will need to transfer your data to your mac/windows by scp(Secure Copy Protocol)
the format will be 
$scp -r <address of data in pie> <data folder in your own computer>
remember, if you don't know the whole address, just use pwd to print your current location, and most of the command run on your local pc
ex. 
scp -r pi@ucsdrobocar-148-12.local:/home/pi/ucsdrobocar-148-12/data/tub_1_25-10-21 /Users/edwardwang/Documents/FA25/ece148/ucsdrobocar-148-12/
when you finish scp the data to your local computer, use
$python3 train.py --model=models/<YOUR_MODEL_NAME>.h5 --type=linear --tubs=data/<the data folder> (your local computer)

after training, you will have the model in your model folder, and you need to transfer your model to pie by scp
the format scp <address of model in your pc> <address of model your want to put in pie>

$scp /Users/edwardwang/Documents/FA25/ece148/ucsdrobocar-148-12/models/10-21-2025.h5 pi@ucsdrobocar-148-12.local:/home/pi/ucsdrobocar-148-12/models/

Now we back to raspberry pie,
run 
$python3 manage.py drive --model=models/<YOUR_MODEL_NAME>.h5 --type=linear
