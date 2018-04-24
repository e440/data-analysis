# data-analysis
Python Project 5
Project description Link: http://teaching.bioinformatics.dtu.dk/36610/index.php/Data_analysis

#!/usr/bin/env python3
import sys
import re

#Initializing empty dict
people = {}

# opening file with error handling
try:
    filename=  open(people.db, 'r')
except IOerror as e:
    sys.stderr.write(str(error) + '\n')
    sys.exit(1)

#Read file, line by line and import to dictionary 
flag = 0
for line in filename:
    line = filename.readline()
    if line 
