function 包 function--> decorator

python closure :

def counter(fn): # First closure
count=0 #first time , count =0
def inner(*arg,\*\*kwarg):
nonlocal count #重要，唔用得global
count+=1
print("Function {0} was called {1} times".format(fn.**name**,count))
return fn(*arg,\*\*kwarg)
return inner

# def add(a,b=0):

def add(a,b): # 冇b=0都work
return a+b

# add = counter(add)

# call counter , count=0->closure

# add(1,2)

# print(add(1,2))

# add(2,3)

# add("name","")

# add("1","2")

# print(add("1","2"))

print(add.**name**)
add = counter(add)
print(add.**name**)
