# Creating a simple pedigree with Kinship2 in R

### Notes before starting
- Kinship2 is a powerful package to help you visualize family structure of a captive population when there is a well-kept family record of which individuals were born to whom.
- Kinship2 only needs an identifier (Studbook ID or Field ID), and the sex of your individuals to operate 
    - There are other optional affectors that we will get into later
- Kinship seperates individuals by row and has options for 9 columns of data.
    1. id - the unique identifier for an individual
    2.  dadid - the identifier of the father of the individual specified by 'id'
    3. momid - the identifier of the mother of the individual specified by 'id'
    4. sex - the sex of the individual(1=male, 2=female, 3=unkown, 4=terminated). You can use either the number or character string.
    5. affected - an affector status used to show things like the known presence of a trait. 0 or 1 which represent unaffected or affected, respectively.
    6. status - is the individual living (default), dead (1), or censored (0).
    7. relation - for signigfying special relationships such as twins
    8. an option numerator used to seperate data into clusters or "families"
    9. missid - for specifiying if missing data is different from default "NA"

## Making a pedigree
### Step 1:  download and load the kinship2 package into environment
```
install.packages('kinship2')
library('kinship2')
```
##### We are going to use the sample data from Kinship2 for this demo

### Step 2: Load and check your data
```
data("sample.ped")
print(sample.ped)
```
- How you load in your data will vary but the structure of your data should be the same as the one shown below
![](https://i.imgur.com/HsD4Qwt.png)


- We can see that this file has a column for ped, id, father, mother, sex, affected, and avail and a futher look shows us that there are 55 individuals in 2 families.

### Step 3: Creating and plotting pedigree object
- Now that our data has been read in we need to create a pedigree object
- The function for this is 'pedigree' with arguments defined above
    - You can also use '?pedigree' to learn more 
- We will start with the most basic form using only the id, father, mother, and sex arguments (these are the minimum needed for Kinship2 to run)  
```
basic.pedigree <- pedigree(id = sample.ped$id, dadid = sample.ped$father, 
momid = sample.ped$mother, sex = sample.ped$sex)
```
- This will create a pedigree object that we can now plot
```
plot(basic.pedigree)
```
![](https://i.imgur.com/QUBSx5o.jpg)
We can breakdown our figure:
- The pedigree is in classic style with males being denoted by squares and females by circle
- The dashed lines represent an individual that is present in another portion of the pedigree. This can be useful to display change of partners or help make the pedgree more concise if there is a larger family structure within another family.

When we ran our pedigree we got the note:
![](https://i.imgur.com/Tcz4tAa.png)
    
- This appeared because individual 113 has no relatives of any kind in the pedigree so it was excluded
    - This is helpful when creating a pedigree because you can include every individual you have data for and Kinship2 will wort out if they can/need to be included.

### Step 4: Adding other information to pedigree
Now that we know the basics for creating a pedigree object we can start to further add to it

We can add affected variables to display known traits or groupings
```
new.pedigree <- pedigree(id = sample.ped$id, dadid = sample.ped$father, 
momid = sample.ped$mother, sex = sample.ped$sex, affected = sample.ped$affected)

plot(new.pedigree)
```
![](https://i.imgur.com/RVDty6F.jpg)
- Now we can see that all individuals are now given a status for a known trait
- We can take this further by adding a legend to our plot so readers can interpret what these statuses mean
```
plot(new.pedigree)
pedigree.legend(ped = new.pedigree, label = 'Trait1', location = "bottomright",
radius=0.5)
```
![](https://i.imgur.com/7aheRry.jpg)

We can also add things such as status: living(default), dead(1), or censored(0)
```
new.pedigree.2 <- pedigree(id = sample.ped$id, dadid = sample.ped$father, 
momid = sample.ped$mother, sex = sample.ped$sex, affected = sample.ped$affected, 
status = sample.ped$avail)

plot(new.pedigree.2)
pedigree.legend(ped = new.pedigree.2, label = 'Trait1', location = "bottomright", 
radius=0.5)
```
![](https://i.imgur.com/q2NwNTx.jpg)

### Step 5: Make it your own!
- There are a range of other functions that you can play with including:
    - Showing multiple traits within your dat using a matrix of data for 'affected'.
    - Seperating large pedigrees into smaller groups or families using the 'famid' argument.

