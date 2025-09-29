# Python Basic

# Basic Python

## Strings
```
print("Goodbye, Pluto")
print('Goodbye, Pluto')
print("""This string runs
on multiple lines""") #triple quote for multi line
print ("This string is " "+" "awesome") #concatenate
print('\n') # new line
print("Test new line!")
```

## Maths
```
print(50 + 50) #add
print(50 - 50) #subtract
print(50 * 50) #multiply
print(50 / 50) #divide
print(50 ** 2) #exponents
print(50 % 6) #modulo-what is leftover
print(50 / 6) #with leftover, float
print(50 // 6) #without leftover

```
## Variables and Methods
```
quote = "All is fair in love and war."

print(quote.upper()) #uppercase
print(quote.lower()) #lowercase
print(quote.title()) #title case
print(len(quote)) #counts characters

name = "Fred" #string
age = 33 #int
gpa  = 3.7 #float

print("My name is " + name + " and I am " + str(age) + " years old.") #age is an interger and needed to be converted to string

age += 1 #adds a year to age
print(age)

birthday = 1
age += birthday #adds age and birthday variables
print(age)
```

## Functions

```
def who_am_i(): #function without parameters
Name = "Fred"
age = 40
print("My name is " + name + " and I am " + str(age) + " years old.")
			
def add_one_hundred(num):
print(num + 100)

add_one_hundred(100) #call function, should be 200
```

#	# Boolean Expressions

```
bool1 = True
bool2 = 3*3 == 9
bool3 = False
bool4 = 3*3 != 9

print(bool1, bool2, bool3, bool4)
```

## Relational and Boolean Operators

```
and1 = (7 > 5) and (5 < 7) #True
and2 = (7 > 5) and (5 > 7) #False
or1 = (7 > 5) or (5 < 7) #True
or2 = (7 > 5) or (5 > 7) #True
```

## Conditional Statements

```
def drink(money):
if money >= 2:
return "You've got yourself a drink!"
else:
return "No drink for you!"

print(drink(3))
print(drink(1))

def alcohol(age, money)
if (age >= 18) and (money >= 5)
return "We're getting a drink!"
elif (age >= 18) and (money < 5)
return "Come back with more money!"
elif (age < 18) and (money >= 5)
return (Nice try kid!)
else:
return "You're too young and too poor!"

print(alcohol(18, 5))
print(alcohol(18, 4))
print(alcohol(17, 5))
print(alcohol(17, 4))
```

## Lists

```
movies = ["Jurassic Park", "Kill Bill", "The Departed", "Toy Story"]

print(movies[1]) # will print second item in list, 0 is first
print(movies[0]) # will print first item
print(movies[1:3]) will print items 2 and 3, in 1:3 its up to not including 3
print(movies[1:]) #prints all after listed number
print(movies[:1]) #prints all before listed number
print(movies[-1]) #print last item

print(leng(movies)) #count items in list

movies.append(JAWS) #add to end of list
movies.insert(2, "Talk to Me") #insert movie at 2 position
movies.pop() # remove last item
movies.pop(3) # remove specific item

wife_movies = ["Legally Blonde", "Legally Blonde 2"]
our_favourite_movies = movies + wife_movies #combine lists
print(our_favourite_movies)
```

## Tuples

```
grades ("a", "b", "c", "d", "f") #Tuples dont change, can't be edited
print(grades[1])
```

## Looping

```
#For loops - start to finish of an iterate
vegetables = ["carrot", "potato", "capsicum"]
for x in vegetables:
print(x)

#while loops, excute as long as true
i = 1
while i < 10
print (i)
i += 1
```

## Advanced strings

```
my_name = "Elvis"
print(my_name[0]) #first letter
print(my_name[-1]) #last letter

sentence = "This is a sentence"
print(sentence[:4]) #prints letters up to 4
print(sentence.split()) #delimeter - default is a space

sentence_split = sentence.split() #splits sentence
sentence_join = ' '.join(sentence_split) #joins it again
print(sentence_join)

quote = "He said 'give me all your money'" # can use single or double quotes inside eachother
quote = "He said \'give me all your money\'" #ignores outside quotes
print(quote)

too_much_space = "                    hello                         "
print(too_much_space.strip()) #strips away spaces by default

print("A" in "Apple") #True
print("a" in "Apple") #False, case sensitive

letter = "A"
word = "Apple"
print(letter.lower() in word.lower()) #Improved, true

movie = "Jurassic Park"
print("My favourite movie is {}.".format(movie))
print("My favourite movie is %s." % movie)
print(f'My favourite movie is {movie}.'') #best way in python3
```

## Dictionaries

```
drinks = {"White Russian" : 7, "Old Fashioned"  : 10, "Lemon Drop" : 8} #drink is the key, price is the value
print(drinks)

employees = {"Finance": ["Bob", "Linda", "Tina"], "IT": ["Gene", "Louise", "Teddy"], "HR": ["Jimmy Jr.", "Mort"]}
print(employees)

employees['Legal'] = ["Mr. Frond"] #adds new key: value pair
print(employees)

employees.update({"Sales": ["Andie", "Ollie"]}) #adds new key: value pair
print(employees)

drinks['White Russian'] = 8 #updates value
print(drinks)

print(drinks.get("White Russian")) #returns only value
```

## Importing

```
import sys #system functions and parameters
from datetime import datetime as dt #import with alias

print(sys.version)
print(dt.now())
```
