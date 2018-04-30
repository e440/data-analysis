# data-analysis
Python Project 5
Project description Link: http://teaching.bioinformatics.dtu.dk/36610/index.php/Data_analysis
#!/usr/bin/python3
import sys
import re
#Initializing empty dict
people = {}
# opening file with error handling
try:
	infile=  open('people.db', 'r')
except IOError as e:
	sys.stderr.write(str(error) + '\n')
	sys.exit(1)
#Read file, line by line and import to dictionary 
children = ('None')
cpr = ''
name = ''
last_name = ''
height = ''
weight =''
blood_type = ''
flag = False
for line in infile:
	if re.search('^(CPR:)\s',line):
		cpr = re.search('\s(\d{6}[-]\d{4})\s',line).group(1)
		flag = True
	if re.search('^(First name:)\s',line):
		name = re.search('\s(\w+)\s',line).group(1)
	if re.search('^(Last name:)\s',line):
		last_name = re.search('\s(\w+)\s',line).group(1)
	if re.search('^(Height:)\s',line):
		height = re.search('\s(\d+)\s',line).group(1)
	if re.search('^(Weight:)\s',line):
		weight = re.search('\s(\d+)\s',line).group(1)
	if re.search('^(Blood type:)\s',line):
		blood_type = re.search('\s(\w{1,2}[-+]{1})\s',line).group(1)
	if re.search('^(Children:)\s',line):
		children = re.findall('(\d{6}[-]\d{4})',line)
	if line == '\n':
		flag = False
	if flag == False and cpr != '': 
		people[cpr]={"name": name, "last_name": last_name, "height": height, "weight": weight, "blood_type": blood_type, "children": children}
		children = ('None')
		cpr = ''
		name = ''
		last_name = ''
		height = ''
		weight =''
		blood_type = ''
if cpr !='':
	people[cpr]={"name": name, "last_name": last_name, "height": height, "weight": weight, "blood_type": blood_type, "children": children}
infile.close()
cpr_list=list(people.keys())
women = 0
men = 0
age20= 0
age30 =0
age40= 0
age50=0
age60=0
age70=0
age80=0
age80over=0
for i in cpr_list:
	age= 2018 - (1900 +int(i[4:6]))
	if int(age) <=20:
		age20 +=1
	if age <=30 and age >20:
		age30 +=1
	if age <=40 and age >30:
		age40 +=1
	if age <=50 and age >40:
		age50 +=1
	if age <=60 and age >50:
		age60 +=1
	if age <=70 and age >60:
		age70 +=1
	if age <=80 and age >70:
		age80 +=1
	if age >80:
		age80over +=1	
	if int(i[-1]) % 2 == 0:
		women +=1
	if int(i[-1]) % 2 == 1:
		men +=1
		chi = people[i]["children"]
		non_fathers= list()
		if chi == 'None':
			non_fathers
print(non_fathers)



	
	
	
	
