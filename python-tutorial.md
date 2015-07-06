# Python tutorial

Tutorial: http://ai.berkeley.edu/tutorial.html#PythonBasics

Python quiz: http://www.mypythonquiz.com/


* Ignore functions with underscores "_" around the names; these are private helper methods.
* Beware!!! Python uses the indentation in the source code for interpretation:
** if your Python file switches from using tabs as indentation to spaces as indentation, the Python interpreter will not be able to resolve the ambiguity of the indentation level and throw an exception.
* Lists: sequence of mutable items. They have a fixed order.
** fruits = ['apple','orange','pear','banana']
* Tuple: A data structure similar to the list is the tuple, which is like a list except that it is immutable once it is created.
** pair = (3,5)
* Set: A set is another data structure that serves as an unordered list with no duplicate items. You can do difference, intersection, union...
* Dictionary: stores a map from one type of object (the key) to another (the value). The key must be an immutable type (string, number, or tuple). The value can be any Python data type.
Unlike lists which have a fixed ordering, a dictionary is simply a hash table for which there is no fixed ordering of the keys (like HashMaps in Java). The order of the keys depends on how exactly the hashing algorithm maps keys to buckets, and will usually seem arbitrary. Your code should not rely on key ordering, and you should not be surprised if even a small modification to how your code uses a dictionary results in a new key ordering.
** studentIds = {'knuth': 42.0, 'turing': 56.0, 'nash': 92.0 }
* Like in functional languages, it exists map and filter
>>> map(lambda x: x * x, [1,2,3])
[1, 4, 9]
>>> filter(lambda x: x > 3, [1,2,3,4,5,4,3,2,1])
[4, 5, 4]
* List comprehensions (listcomp.py). List comprehensions provide a concise way to create lists. Common applications are to make new lists where each element is the result of some operations applied to each member of another sequence or iterable, or to create a subsequence of those elements that satisfy a certain condition.
nums = [1,2,3,4,5,6]
plusOneNums = [x+1 for x in nums]
oddNums = [x for x in nums if x % 2 == 1]
print oddNums
oddNumsPlusOne = [x+1 for x in nums if x % 2 ==1]
print oddNumsPlusOne
* Functions:
Rather than having a main function as in Java, the __name__ == '__main__' check is used to delimit expressions which are executed when the file is called as a script from the command line.
fruitPrices = {'apples':2.00, 'oranges': 1.50, 'pears': 1.75}

def buyFruit(fruit, numPounds):
    if fruit not in fruitPrices:
        print "Sorry we don't have %s" % (fruit)
    else:
        cost = fruitPrices[fruit] * numPounds
        print "That'll be %f please" % (cost)

# Main Function
if __name__ == '__main__':        
    buyFruit('apples',2.4)
    buyFruit('coconuts',2)   


* You can create classes: class FruitShop:
* The pass statement does nothing. It can be used when a statement is required syntactically but the program requires no action.