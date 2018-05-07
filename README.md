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
#Read file, line by line and classify data  
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
# initializing dictionaries for each person and adding them into main dictinoary
if cpr !='':
	people[cpr]={"name": name, "last_name": last_name, "height": height, "weight": weight, "blood_type": blood_type, "children": children}
infile.close()
cpr_list=list(people.keys())
# define ranges, lists , sets, dictionary for tasks from 1 to 6
sum_women = 0
sum_men = 0
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
m_age40over=0
m_age_sum = 0
m_age_min = ''
m_age_max = ''
f_age20= 0
f_age30 =0
f_age40= 0
f_age40over=0
f_age_sum = 0
f_age_min = ''
f_age_max = ''
non_fathers= 0
fathers = list()
non_mothers= 0
mothers = list()
first_born = set()
children_set = set()
children = {}
# control age ranges (we know the data contains people who were born in 1900s) 
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
# check gender distribution 
	if int(i[-1]) % 2 == 0:
		sum_women +=1
# if she is a mother or not 
		chi = people[i]["children"]
		if chi == 'None':
			non_mothers += 1
# if she is a mother what is first motherhood age( max, min and avg ) and who is first born
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
			first_born.add(first_child)
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
				f_age40over +=1
	
# and the same for males (if a father, fatherhood age : min max avg) 
	elif int(i[-1]) % 2 == 1:
		sum_men +=1
		chi = people[i]["children"]
		if chi == 'None':
			non_fathers += 1
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
			first_born.add(first_child)
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
			elif fatherhood_age >80:
				m_age40over +=1
print('Total number of men and women:', sum_men, 'and', sum_women)
print('Average age is:', age_sum / len(people))
print('Minimum and maximum age :', age_min, 'and', age_max)
print('Age distribution is:','\n', 'Age <= 20: %',(age20*100)/len(people), '\n', '20<age<=30: %', (age30*100)/len(people), '\n', '30<age<=40: %', (age40*100)/len(people), '\n', '40<age<=50: %', (age50*100)/len(people), '\n', '50<age<=60: %', (age60*100)/len(people), '\n', '60<age<=70: %', (age70*100)/len(people), '\n', '70<age<=80: %', (age80*100)/len(people), '\n', 'age< 80: %', (age80over*100)/len(people))
print('Minimum, Maximum and Average Fatherhood age:', m_age_min, m_age_max, (m_age_sum / len(fathers)))
print('Minimum, Maximum and Average Mottherhood age:', f_age_min, f_age_max, (f_age_sum / len(mothers)))
print('Age distribution of fatherhood:','\n', 'Age <= 20: %',(m_age20*100)/len(fathers), '\n', '20<age<=30: %', (m_age30*100)/len(fathers), '\n', '30<age<=40: %', (m_age40*100)/len(fathers), '\n', '40<age %', (m_age40over*100)/len(fathers)) 
print('Age distribution of motherhood:','\n', 'Age <= 20: %',(f_age20*100)/len(mothers), '\n', '20<age<=30: %', (f_age30*100)/len(mothers), '\n', '30<age<=40: %', (f_age40*100)/len(mothers), '\n', '40<age %', (f_age40over*100)/len(mothers)) 
print((non_fathers*100)/sum_men, ' percentage of men do not have children')
print((non_mothers*100)/sum_women, ' percentage of women do not have children')
# initializing a new dictionary for each children show parents
for b in children_set:
	children[b] = {"mother": '', "father": ''}
for c in fathers:
	ch = people[c]["children"]
	for d in ch:
		children[d]["father"] = c
for e in mothers:
	ds = people[e]["children"]
	for f in ds:
		children[f]["mother"] = e
# find couples (marriages) 
couples = set()
for g in children:
	fath = children[g]["father"]
	moth = children[g]["mother"]
	parents = fath, moth
	couples.add(parents)
# calculate age difference between parents (we use couples set for avoiding duplicates, couples who have more than 1 children) 
agedifference = 0
for h in couples:
	difference = abs(int(h[0][4:6]) - int(h[1][4:6]))
	agedifference = agedifference + difference
print('Average age difference between parents:',agedifference/len(couples))
# find who are grand children in database (if a person is both in parents and children list: children of this person should be a grandchildren of this person's parents)
parents_list = fathers + mothers
grand_children = set()
gr = 0
for j in parents_list:
	if j in children_set:
		grands= people[j]["children"]
		for gr in grands:
			grand_children.add(gr)
print(' % of people who has at least one alive grandparent:', (len(grand_children)*100)/len(people))
# to see total number of cousins you should first go up to grand parents and than down to uncles and aunts
cousins_sum = 0
cousin_owners = 0
for honey in grand_children:
	cousins_set = set()
	mor= children[honey]["mother"]
	if mor in children:
		mormor = children[mor]["mother"]
		mormor_children= people[mormor]["children"]
		for sb in mormor_children:
			if sb != mor:
				cousins= people[sb]["children"]
				if cousins != 'None':
					for cousin in cousins:
						cousins_set.add(cousin)
		morfar = children[mor]["father"]
		morfar_children= people[morfar]["children"]
		for sbl in morfar_children:
			if sbl != mor:
				cousins= people[sbl]["children"]
				if cousins != 'None':
					for cousin in cousins:
						cousins_set.add(cousin)					
	far= children[honey]["father"]
	if far in children:
		farmor = children[far]["mother"]
		farmor_children = people[farmor]["children"]
		for sb in farmor_children:
			if sb != far:
				cousins = people[sb]["children"]
				if cousins != 'None':
					for cousin in cousins:
						cousins_set.add(cousin)
		farfar = children[far]["father"]
		farfar_children = people[farfar]["children"]
		for sbl in farfar_children:
			if sbl != far:
				cousins = people[sbl]["children"]
				if cousins != 'None':
					for cousin in cousins:
						cousins_set.add(cousin)
	if cousins_set != set():
		cousins_sum= cousins_sum + (len(cousins_set))
		cousin_owners += 1
print(' average # cousins:', cousins_sum / cousin_owners)
# gender distribution of first born 
boys = 0
girls = 0 
for first in first_born:
	if int(first[-1]) % 2 == 1:
		boys +=1
	elif int(first[-1]) % 2 == 0:
		girls +=1
print('first born gender distribution:', boys*100/len(first_born), '% boy', girls*100/len(first_born), '%girl')
# check for all fathers and mothers how many partners they have (use children)
duo_fathers = 0
duo_mothers = 0
for father in fathers:
	mama = set()
	kids = people[father]["children"]
	if len(kids) > 1:
		for kid in kids:
			mama.add(children[kid]["mother"])
	if len(mama) >1 :
		duo_fathers += 1
for mother in mothers:
	papa = set()
	kids = people[mother]["children"]
	if len(kids) > 1:
		for kid in kids:
			papa.add(children[kid]["father"])
	if len(papa) > 1:
		duo_mothers += 1
print('parents who have children from different partners:', duo_fathers*100/len(fathers), '% of fathers', duo_mothers*100/len(mothers), '% of mothers')
# for each children check parents' surname if they are same 
share = set()
for k in children: 
	mother = children[k]["mother"]
	father = children[k]["father"]
	mot_surname = people[mother]["last_name"]
	fat_surname = people[father]["last_name"]
	if mot_surname == fat_surname:
		couple = father, mother
		share.add(couple)
# if surnames are same check origin of this surname ( if come from woman or man also it is possible to have same surname before marriage
none_of = 0
from_man= 0
from_woman = 0
for l in share:
	ff_surname = ''
	mf_surname = ''
	if l[0] in children_set:
		farfar = children[l[0]]["father"]
		ff_surname = people[farfar]["last_name"]
	if l[1] in children_set:
		morfar = children[l[1]]["father"]
		mf_surname = people[morfar]["last_name"]
	if mf_surname != '' and ff_surname != '' and mf_surname == ff_surname:
		none_of += 1
	elif mf_surname != '' and ff_surname != '' and mf_surname != ff_surname:
		if mf_surname == people[l[0]]["last_name"]:
			from_woman += 1
		elif ff_surname == people[l[0]]["last_name"]:
			from_man += 1
	elif mf_surname == '' and ff_surname != '' and mf_surname != ff_surname:
		if ff_surname == people[l[0]]["last_name"]:
			from_man += 1
	elif ff_surname == '' and mf_surname != '' and mf_surname != ff_surname:
		if mf_surname == people[l[0]]["last_name"]:
			from_woman += 1
print('percentage of parents who share family name: %', len(share)*100/len(couples))
print('family name from woman, from man or none', from_woman, from_man, none_of)
# define counters for height and weight possibilities 
tall_tall = 0
tall_normal = 0
tall_short = 0
normal_short = 0
normal_normal = 0
short_short = 0 
tall_kids = 0
kids_from_talls = 0
fat_fat = 0
fat_normal = 0
fat_slim = 0
normals = 0 
normal_slim = 0
slim_slim = 0
fat_kids = 0
kids_from_fats = 0
# go through couples categorize them
for m in couples:
	man = ''
	man_height = int(people[m[0]]["height"])
	woman = ''
	woman_height = int(people[m[1]]["height"])
	man_w = ''
	man_weight = int(people[m[0]]["weight"])
	woman_w = ''
	woman_weight = int(people[m[0]]["weight"])
	BMI_woman = woman_weight/((woman_height/100)**2)
	BMI_man = man_weight/((man_height/100)**2)
	if man_height >= 185:
		man = 'tall'
	elif 185 > man_height >= 175:
		man = 'normal'
	elif 175 > man_height:
		man = 'short'
	if woman_height >= 180:
		woman = 'tall'
	elif 180 > woman_height >= 170:
		woman = 'normal'
	elif 170 > woman_height:
		woman = 'short'
	if man == 'tall' and woman == 'tall':
		tall_tall += 1
# find children of tall parents and see if they are tall
		kids = people[m[0]]['children']
		for kid in kids:
			kids_from_talls += 1
			kid_height = int(people[kid]['height'])
			if int(kid[-1]) % 2 == 0 and kid_height >= 180:
				tall_kids += 1
			elif int(kid[-1]) % 2 == 1 and kid_height >= 185:
				tall_kids += 1
	elif man == 'tall' and woman == 'normal' or man == 'normal' and woman == 'tall':
		tall_normal += 1
	elif man == 'tall' and woman == 'short' or man == 'short' and woman == 'tall':
		tall_short += 1
	elif man == 'normal' and woman =='short' or man == 'short' and woman == 'normal':
		normal_short += 1
	elif man == 'normal' and woman == 'normal':
		normal_normal += 1
	elif man == 'short' and woman == 'short':
		short_short += 1
	if BMI_man <= 18.5:
		man_w = 'slim'
	elif 18.5 <  BMI_man <= 25:
		man_w = 'normal'
	elif BMI_man > 25:
		man_w = 'fat'
	if BMI_woman <= 18.5:
		woman_w = 'slim'
	elif 18.5 <  BMI_woman <= 25:
		woman_w = 'normal'
	elif BMI_woman > 25:
		woman_w = 'fat'
	if man_w == 'fat' and woman_w == 'fat':
		fat_fat += 1
# find children of fat parents and see if they are fat
		kids = people[m[0]]['children']
		for kid in kids:
			kids_from_fats += 1
			kid_height = int(people[kid]['height'])
			kid_weight = int(people[kid]['weight'])
			BMI_kid = kid_weight/((kid_height/100)**2)
			if BMI_kid > 25:
				fat_kids += 1
	elif man_w == 'fat' and woman_w == 'normal' or man_w == 'normal' and woman_w == 'fat':
		fat_normal += 1
	elif man_w == 'fat' and woman_w == 'slim' or man_w == 'slim' and woman_w == 'fat':
		fat_slim += 1
	elif man_w == 'normal' and woman_w =='slim' or man_w == 'slim' and woman_w == 'normal':
		normal_slim += 1
	elif man_w == 'normal' and woman_w == 'normal':
		normals += 1
	elif man_w == 'slim' and woman_w == 'slim':
		slim_slim += 1
print('height distribution:', '\n', 'tall_tall: %', tall_tall*100/len(couples), '\n', 'tall_normal: %', tall_normal*100/len(couples), '\n', 'tall_short: %', tall_short*100/len(couples), '\n','normal_normal: %', normal_normal*100/len(couples), '\n', 'normal_short: %', normal_short*100/len(couples), '\n', 'short_short: %', short_short*100/len(couples))
print('tall children percentage from tall parents: %', tall_kids*100/kids_from_talls)
print('weight distribution:', '\n', 'fat_fat: %', fat_fat*100/len(couples), '\n', 'fat_normal: %', fat_normal*100/len(couples), '\n', 'fat_slim: %', fat_slim*100/len(couples), '\n','normal_normal: %', normals*100/len(couples), '\n', 'normal_slim: %', normal_slim*100/len(couples), '\n', 'slim_slim: %', slim_slim*100/len(couples))
print('fat children percentage from fat parents: %', fat_kids*100/kids_from_fats)
# comparision of blood types 
adopted = list()
donor_fathers= {}
# for each children check blood type and parents' blood type, see which are not possible according to genetic inheritance
for n in children:
	child_blood = people[n]['blood_type']
	mother_blood = people[children[n]['mother']]['blood_type']
	father_blood = people[children[n]['father']]['blood_type']
	if ((mother_blood[:-1] == 'A' and father_blood[:-1] == 'A') or ( mother_blood == 'A' and father_blood[:-1] == 'O') or ( mother_blood[:-1] == 'O' and father_blood[:-1] == 'A')) and ( child_blood[:-1] == 'B' or child_blood[:-1] == 'AB'):
		adopted[-1:-1] = [n]
	elif ((mother_blood[:-1] == 'B' and father_blood[:-1] == 'B') or ( mother_blood == 'B' and father_blood[:-1] == 'O') or ( mother_blood[:-1] == 'O' and father_blood[:-1] == 'B')) and ( child_blood[:-1] == 'A' or child_blood[:-1] == 'AB'):
		adopted[-1:-1] = [n]
	elif (mother_blood[:-1] == 'O' and father_blood[:-1] == 'O') and (child_blood[:-1] == 'A' or child_blood[:-1] == 'B' or child_blood[:-1] == 'AB'):
		adopted[-1:-1] = [n]
	elif ((mother_blood[:-1] == 'AB' and father_blood[:-1] == 'AB') or ( mother_blood == 'AB' and father_blood[:-1] == 'O') or ( mother_blood[:-1] == 'O' and father_blood[:-1] == 'AB')) and ( child_blood[:-1] == 'O' ):
		adopted[-1:-1] = [n]
	elif mother_blood[-1] == '-' and father_blood[-1] == '-' and child_blood[-1] == '+':
		adopted[-1:-1] = [n]
# we know father and son blood types ask if father can donate blood to his son(s)
	if int(n[-1]) % 2 == 1 and ((child_blood[-1] == '+') or (father_blood[-1] == '-' and child_blood[-1] == '-')) :
		if father_blood[:-1] == 'A' and (child_blood[:-1] == 'A' or child_blood[:-1] == 'AB'):
			donor_father= children[n]['father']
			son = n + ' ' + child_blood
			if donor_father not in donor_fathers:
				donor_fathers[donor_father] = { 'father_blood_type': father_blood, 'sons_and_blood_types': [son]}
			else:
				donor_fathers[donor_father]['sons_and_blood_types'] += [son]
		elif father_blood[:-1] == 'B' and (child_blood[:-1] == 'B' or child_blood[:-1] == 'AB'):
			donor_father= children[n]['father']
			son = n + ' ' + child_blood
			if donor_father not in donor_fathers:
				donor_fathers[donor_father] = { 'father_blood_type': father_blood, 'sons_and_blood_types': [son]}
			else:
				donor_fathers[donor_father]['sons_and_blood_types'] += [son]
		elif father_blood[:-1] == 'AB' and  child_blood[:-1] == 'AB':
			donor_father= children[n]['father']
			son = n + ' ' + child_blood
			if donor_father not in donor_fathers:
				donor_fathers[donor_father] = { 'father_blood_type': father_blood, 'sons_and_blood_types': [son]}
			else:
				donor_fathers[donor_father]['sons_and_blood_types'] += [son]		
		elif father_blood[:-1] == 'O':
			donor_father= children[n]['father']
			son = n + ' ' + child_blood
			if donor_father not in donor_fathers:
				donor_fathers[donor_father] = { 'father_blood_type': father_blood, 'sons_and_blood_types': [son]}
			else:
				donor_fathers[donor_father]['sons_and_blood_types'] += [son]
print('Children with not real parents:', '\n', adopted)
print('Donor fathers and their sons:', '\n', donor_fathers)
# similar to previous one, this time donors are grandchildren important to go through mother and father
donor_grandchildren = {}
for o in grand_children:
	o_blood = people[o]['blood_type']
	o_father= children[o]['father']
	o_mother= children[o]['mother']
	if o_father != 'None' and (o_father in children):
		o_farfar = children[o_father]['father']
		o_farmor = children[o_father]['mother']
		o_farfar_blood = people[o_farfar]['blood_type']
		o_farmor_blood = people[o_farmor]['blood_type']
		if  ((o_farfar_blood[-1] == '+') or (o_blood[-1] == '-' and o_blood[-1] == '-')) :
			if o_blood[:-1] == 'A' and (o_farfar_blood[:-1] == 'A' or o_farfar_blood[:-1] == 'AB'):
				grandparent = o_farfar + ' ' + o_farfar_blood
				if o not in donor_grandchildren:
					donor_grandchildren[o] = { 'grandchildren_blood_type': o_blood, 'grandparents_and_blood_types': [grandparent]}
				else:
					donor_grandchildren[o]['grandparents_and_blood_types'] += [grandparent]
			elif o_blood[:-1] == 'B' and (o_farfar_blood[:-1] == 'B' or o_blood[:-1] == 'AB'):
				grandparent = o_farfar + ' ' + o_farfar_blood
				if o not in donor_grandchildren:
					donor_grandchildren[o] = { 'grandchildren_blood_type': o_blood, 'grandparents_and_blood_types': [grandparent]}
				else:
					donor_grandchildren[o]['grandparents_and_blood_types'] += [grandparent]
			elif o_blood[:-1] == 'AB' and  o_farfar_blood[:-1] == 'AB':
				grandparent = o_farfar + ' ' + o_farfar_blood
				if o not in donor_grandchildren:
					donor_grandchildren[o] = { 'grandchildren_blood_type': o_blood, 'grandparents_and_blood_types': [grandparent]}
				else:
					donor_grandchildren[o]['grandparents_and_blood_types'] += [grandparent]		
			elif o_blood[:-1] == 'O':
				grandparent = o_farfar + ' ' + o_farfar_blood
				if o not in donor_grandchildren:
					donor_grandchildren[o] = { 'grandchildren_blood_type': o_blood, 'grandparents_and_blood_types': [grandparent]}
				else:
					donor_grandchildren[o]['grandparents_and_blood_types'] += [grandparent]
		if  ((o_farmor_blood[-1] == '+') or (o_blood[-1] == '-' and o_blood[-1] == '-')) :
			if o_blood[:-1] == 'A' and (o_farmor_blood[:-1] == 'A' or o_farmor_blood[:-1] == 'AB'):
				grandparent = o_farmor + ' ' + o_farmor_blood
				if o not in donor_grandchildren:
					donor_grandchildren[o] = { 'grandchildren_blood_type': o_blood, 'grandparents_and_blood_types': [grandparent]}
				else:
					donor_grandchildren[o]['grandparents_and_blood_types'] += [grandparent]
			elif o_blood[:-1] == 'B' and (o_farmor_blood[:-1] == 'B' or o_blood[:-1] == 'AB'):
				grandparent = o_farmor + ' ' + o_farmor_blood
				if o not in donor_grandchildren:
					donor_grandchildren[o] = { 'grandchildren_blood_type': o_blood, 'grandparents_and_blood_types': [grandparent]}
				else:
					donor_grandchildren[o]['grandparents_and_blood_types'] += [grandparent]
			elif o_blood[:-1] == 'AB' and  o_farmor_blood[:-1] == 'AB':
				grandparent = o_farmor + ' ' + o_farmor_blood
				if o not in donor_grandchildren:
					donor_grandchildren[o] = { 'grandchildren_blood_type': o_blood, 'grandparents_and_blood_types': [grandparent]}
				else:
					donor_grandchildren[o]['grandparents_and_blood_types'] += [grandparent]		
			elif o_blood[:-1] == 'O':
				grandparent = o_farmor + ' ' + o_farmor_blood
				if o not in donor_grandchildren:
					donor_grandchildren[o] = { 'grandchildren_blood_type': o_blood, 'grandparents_and_blood_types': [grandparent]}
				else:
					donor_grandchildren[o]['grandparents_and_blood_types'] += [grandparent]
	if o_mother != 'None' and (o_mother in children):
		o_morfar = children[o_mother]['father']
		o_mormor = children[o_mother]['mother']
		o_morfar_blood = people[o_morfar]['blood_type']
		o_mormor_blood = people[o_mormor]['blood_type']
		if  ((o_morfar_blood[-1] == '+') or (o_blood[-1] == '-' and o_blood[-1] == '-')) :
			if o_blood[:-1] == 'A' and (o_morfar_blood[:-1] == 'A' or o_morfar_blood[:-1] == 'AB'):
				grandparent = o_morfar + ' ' + o_morfar_blood
				if o not in donor_grandchildren:
					donor_grandchildren[o] = { 'grandchildren_blood_type': o_blood, 'grandparents_and_blood_types': [grandparent]}
				else:
					donor_grandchildren[o]['grandparents_and_blood_types'] += [grandparent]
			elif o_blood[:-1] == 'B' and (o_morfar_blood[:-1] == 'B' or o_blood[:-1] == 'AB'):
				grandparent = o_morfar + ' ' + o_morfar_blood
				if o not in donor_grandchildren:
					donor_grandchildren[o] = { 'grandchildren_blood_type': o_blood, 'grandparents_and_blood_types': [grandparent]}
				else:
					donor_grandchildren[o]['grandparents_and_blood_types'] += [grandparent]
			elif o_blood[:-1] == 'AB' and  o_morfar_blood[:-1] == 'AB':
				grandparent = o_morfar + ' ' + o_morfar_blood
				if o not in donor_grandchildren:
					donor_grandchildren[o] = { 'grandchildren_blood_type': o_blood, 'grandparents_and_blood_types': [grandparent]}
				else:
					donor_grandchildren[o]['grandparents_and_blood_types'] += [grandparent]		
			elif o_blood[:-1] == 'O':
				grandparent = o_morfar + ' ' + o_morfar_blood
				if o not in donor_grandchildren:
					donor_grandchildren[o] = { 'grandchildren_blood_type': o_blood, 'grandparents_and_blood_types': [grandparent]}
				else:
					donor_grandchildren[o]['grandparents_and_blood_types'] += [grandparent]
		if  ((o_mormor_blood[-1] == '+') or (o_blood[-1] == '-' and o_blood[-1] == '-')) :
			if o_blood[:-1] == 'A' and (o_mormor_blood[:-1] == 'A' or o_mormor_blood[:-1] == 'AB'):
				grandparent = o_mormor + ' ' + o_mormor_blood
				if o not in donor_grandchildren:
					donor_grandchildren[o] = { 'grandchildren_blood_type': o_blood, 'grandparents_and_blood_types': [grandparent]}
				else:
					donor_grandchildren[o]['grandparents_and_blood_types'] += [grandparent]
			elif o_blood[:-1] == 'B' and (o_mormor_blood[:-1] == 'B' or o_blood[:-1] == 'AB'):
				grandparent = o_mormor + ' ' + o_mormor_blood
				if o not in donor_grandchildren:
					donor_grandchildren[o] = { 'grandchildren_blood_type': o_blood, 'grandparents_and_blood_types': [grandparent]}
				else:
					donor_grandchildren[o]['grandparents_and_blood_types'] += [grandparent]
			elif o_blood[:-1] == 'AB' and  o_mormor_blood[:-1] == 'AB':
				grandparent = o_mormor + ' ' + o_mormor_blood
				if o not in donor_grandchildren:
					donor_grandchildren[o] = { 'grandchildren_blood_type': o_blood, 'grandparents_and_blood_types': [grandparent]}
				else:
					donor_grandchildren[o]['grandparents_and_blood_types'] += [grandparent]		
			elif o_blood[:-1] == 'O':
				grandparent = o_mormor + ' ' + o_mormor_blood
				if o not in donor_grandchildren:
					donor_grandchildren[o] = { 'grandchildren_blood_type': o_blood, 'grandparents_and_blood_types': [grandparent]}
				else:
					donor_grandchildren[o]['grandparents_and_blood_types'] += [grandparent]
print('Donor grandchildren are:', '\n', donor_grandchildren)
# blood type distribution
A_pos = 0
A_neg = 0
B_pos = 0
B_neg = 0
AB_pos = 0
AB_neg = 0
O_pos = 0
O_neg = 0
for p in people:
	p_blood = people[p]['blood_type']
	if p_blood == 'A+':
		A_pos += 1
	elif p_blood == 'A-':
		A_neg += 1
	elif p_blood == 'B+':
		B_pos += 1
	elif p_blood == 'B-':
		B_neg += 1
	elif p_blood == 'AB+':
		AB_pos += 1
	elif p_blood == 'AB-':
		AB_neg += 1	
	if p_blood == 'O+':
		O_pos += 1
	elif p_blood == 'O-':
		O_neg += 1
print('Blood type distribution:', '\n', 'A+ : %', A_pos*100/len(people), '\n', 'A- : %', A_neg*100/len(people), '\n', 'B+ : %', B_pos*100/len(people), '\n', 'B- : %', B_neg*100/len(people), '\n', 'AB+ : %', AB_pos*100/len(people), '\n', 'AB- : %', AB_neg*100/len(people), '\n', '0+ : %', O_pos*100/len(people), '\n', '0- : %', O_neg*100/len(people))

