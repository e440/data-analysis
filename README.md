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
	elif re.search('^(First name:)\s',line):
		name = re.search('\s(\w+)\s',line).group(1)
	elif re.search('^(Last name:)\s',line):
		last_name = re.search('\s(\w+)\s',line).group(1)
	elif re.search('^(Height:)\s',line):
		height = re.search('\s(\d+)\s',line).group(1)
	elif re.search('^(Weight:)\s',line):
		weight = re.search('\s(\d+)\s',line).group(1)
	elif re.search('^(Blood type:)\s',line):
		blood_type = re.search('\s(\w{1,2}[-+]{1})\s',line).group(1)
	elif re.search('^(Children:)\s',line):
		children = re.findall('(\d{6}[-]\d{4})',line)
	elif line == '\n':
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
# initializing dictionary names (cpr numbers) and add into a list  
if cpr !='':
	people[cpr]={"name": name, "last_name": last_name, "height": height, "weight": weight, "blood_type": blood_type, "children": children}
infile.close()
cpr_list=list(people.keys())
# define ranges, lists 
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
age_sum = 0
age_min = ''
age_max = ''
m_age20= 0
m_age30 =0
m_age40= 0
m_age50=0
m_age60=0
m_age70=0
m_age80=0
m_age80over=0
m_age_sum = 0
m_age_min = ''
m_age_max = ''
f_age20= 0
f_age30 =0
f_age40= 0
f_age50=0
f_age60=0
f_age70=0
f_age80=0
f_age80over=0
f_age_sum = 0
f_age_min = ''
f_age_max = ''
non_fathers= list()
fathers = list()
non_mothers= list()
mothers = list()
first_born = list()
children_set = set()
# control age ranges 
for i in cpr_list:
	age= 2018 - (1900 +int(i[4:6]))
	age_sum = age_sum + age
	if age_min == '':
		age_min = age
	elif age < age_min:
		age_min = age
	if age_max == '':
		age_max = age
	elif age > age_max:
		age_max = age 
	if int(age) <=20:
		age20 +=1
	if age <=30 and age >20:
		age30 +=1
	elif age <=40 and age >30:
		age40 +=1
	elif age <=50 and age >40:
		age50 +=1
	elif age <=60 and age >50:
		age60 +=1
	elif age <=70 and age >60:
		age70 +=1
	elif age <=80 and age >70:
		age80 +=1
	elif age >80:
		age80over +=1	
# and gender and ıf has chıldren for females and fırstborn child
	if int(i[-1]) % 2 == 0:
		women +=1
		chi = people[i]["children"]
		if chi == 'None':
			non_mothers[-1:-1] = [i]
		else:
			mothers[-1:-1] = [i]
			first_born_date = ''
			motherhood_age = ''
			first_child = ''
			for a in chi:
				children_set.add(a)
				if first_born_date != '' and int(a[4:6]) < first_born:
					first_born_date = int(a[4:6])
					motherhood_age =  int(a[4:6]) - int(i[4:6])
					first_child = a
				else:
					motherhood_age =  int(a[4:6]) - int(i[4:6])
					first_child = a
			first_born[-1:-1] = [first_child]
			f_age_sum = f_age_sum + motherhood_age
			if f_age_min == '':
				f_age_min = motherhood_age
			elif motherhood_age < f_age_min:
				f_age_min = motherhood_age
			if f_age_max == '':
				f_age_max = motherhood_age
			elif motherhood_age > f_age_max:
				f_age_max = motherhood_age 
			if motherhood_age <=20:
				f_age20 +=1
			if motherhood_age <=30 and motherhood_age >20:
				f_age30 +=1
			if motherhood_age <=40 and motherhood_age >30:
				f_age40 +=1
			elif motherhood_age <=50 and motherhood_age >40:
				f_age50 +=1
			elif motherhood_age <=60 and motherhood_age >50:
				f_age60 +=1
			elif motherhood_age <=70 and motherhood_age >60:
				f_age70 +=1
			elif motherhood_age <=80 and motherhood_age >70:
				f_age80 +=1
			elif motherhood_age >80:
				f_age80over +=1
	
# and the same for males
	elif int(i[-1]) % 2 == 1:
		men +=1
		chi = people[i]["children"]
		if chi == 'None':
			non_fathers[-1:-1] = [i]
		else:
			fathers[-1:-1] = [i]
			first_born_date = ''
			fatherhood_age = ''
			first_child = ''
			for a in chi:
				children_set.add(a)
				if first_born_date != '' and int(a[4:6]) < first_born:
					first_born_date = int(a[4:6])
					fatherhood_age =  int(a[4:6]) - int(i[4:6])
					first_child = a
				else:
					fatherhood_age =  int(a[4:6]) - int(i[4:6])
					first_child = a
			first_born[-1:-1] = [first_child]
			m_age_sum = m_age_sum + fatherhood_age
			if m_age_min == '':
				m_age_min = fatherhood_age
			elif fatherhood_age < m_age_min:
				m_age_min = fatherhood_age
			if m_age_max == '':
				m_age_max = fatherhood_age
			elif fatherhood_age > m_age_max:
				m_age_max = fatherhood_age 
			if fatherhood_age <=20:
				m_age20 +=1
			if fatherhood_age <=30 and fatherhood_age >20:
				m_age30 +=1
			if fatherhood_age <=40 and fatherhood_age >30:
				m_age40 +=1
			elif fatherhood_age <=50 and fatherhood_age >40:
				m_age50 +=1
			elif fatherhood_age <=60 and fatherhood_age >50:
				m_age60 +=1
			elif fatherhood_age <=70 and fatherhood_age >60:
				m_age70 +=1
			elif fatherhood_age <=80 and fatherhood_age >70:
				m_age80 +=1
			elif fatherhood_age >80:
				m_age80over +=1
children = {}
# Checking motherhood age
print(len(children_set))



#print(age_min)
#print(age_max)
#print(age_sum)
#average = age_sum / len(cpr_list)
#print(average)



	
	
	
	

