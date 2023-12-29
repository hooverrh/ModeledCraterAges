##This script calculates the n-th percentile of x number of point objects that fall within y number of polygon objects##

#workspace is the name of the root folder or gdb where the shape file or table can be found
workspace = "...."
#bestand is the name of the shape or table to be read
bestand = "...."
#the name of the field that contains the values for which you want to know the percentile
name = "...."
#A unique ID field that identifies the polygon object
id = "...."

P = 0.3                                 #P is the percentile


import math
import arcgisscripting
gp = arcgisscripting.create()

gp.Workspace = workspace
cur = gp.SearchCursor(bestand)

List = []                               
List2= []                               
row = cur.Next()                        
while row <> None:                      
    List.append(row.name)            
    List2.append(row.id)
    row = cur.next()                    

List3 = [None] * len(List)              #This list is used to store the calculated percentiles later on
M = [List, List2, List3]

#The list is sorted by ID 
indices = range(len(List))
indices.sort(key = M[1].__getitem__)
for i, sublist in enumerate(M):
    M = [sublist for j in indices]
                         
#Keeps track of the row numbers for wich the list jumps to the next unique ID
grens = []        
grens.append(0)

p = 0
count = 0
while p<=48 and count<=(len(List)-2): #48 is a hardcoded number of unique polygon objects
      
    #This block counts the number of instances of all unique polygon ID's.
    temp = M[1][count]
    while M[1][count]==temp:
        if count == len(List)-1: break
        else: count += 1
    if count == len(List)-1: grens.append(count)    
    else: grens.append(count-1)
        
    #Here a subpart of the list is sorted on ascending order of point object values
    if count == len(List)-1: M[0][grens
+1:count+1]=sorted(M[0][grens
+1:count+1])
    elif p == 0: M[0][grens
:count]=sorted(M[0][grens
:count]) 
    else: M[0][grens
+1:count]=sorted(M[0][grens
+1:count])

    #Excel method for calculating the percentile
    
    #rank
    if p == 0 or count == len(List)-1: n = P * (count-grens
-1)+1   
    else: n = P * (count-grens
-2)+1
    
    if n == 1:
        n = int(n)
        N = M[0][n-1]
    elif n == count-grens
:
        N = M[0][count-grens
-1]
    else:
        rank = math.modf(n)
        d = rank[0] 
        if p == 0: k = int(rank[1]) + grens

        else: k = int(rank[1]) + grens
 + 1 
        N = M[0][k-1] + d * (M[0] - M[0][k-1])
     
    #The calculated percentile is saved in the list   
    if p==0: k=grens

    else: k=grens
+1
    while k<=count:
            M[2]=N
            k += 1
    p += 1

#The calculated percentiles should now be written to the file
i = 0
while i < len(List2):
    where = "id=" + str(M[1])
    cur = gp.UpdateCursor(bestand, where)
    row = cur.Next()
    while row:     
        row.SetValue("percentiel03", M[2])           #percentiel03 is the name of the new field
        cur.UpdateRow(row)
        row = cur.Next()
    i += 1
