---
layout: post
title:  "CPP STL Cheetsheet"
categories: CPP
---
# Common Methods
* Container.empty() - Returns whether the set is empty.
* Container.size() - Returns the number of elements in the set.
* Container.begin() – Returns an iterator to the first element in the set.
* Container.end() – Returns an iterator to the theoretical element that follows last element in the set.
* Container.clear() – It is used to remove all the elements of the vector container
<hr>

## Vector
Implemented with Array
1. push_back() – It push the elements into a vector from the back
2. pop_back() – It is used to pop or remove elements from a vector from the back.

Accessing Front and Back: vector::front() and vector::back() in C++ STL

3. erase() – It is used to remove elements from a container from the specified position or range.
4. insert() – It inserts new elements before the element at the specified position
5. swap() – It is used to swap the contents of one vector with another vector of same type. Sizes may differ.
6. emplace() – It extends the container by inserting new element at position
The vector::emplace() is an STL in C++ which extends container by inserting new element at position. Reallocation happens only if there is a need of more space. Here the container size increases by one.

7. emplace_back() – It is used to insert a new element into the vector container, the new element is added to the end of the vector
8. assign() – It assigns new value to the vector elements by replacing old ones

#### v.assign(int size, int value)
Parameters: 
* size - number of values to be assigned
* value - value to be assigned to the vectorname
Eg.  v.assign(7, 100); Assign v with 7 elements, each is 100

#### v.assign(arr, arr + size)
Parameters: 
* arr - the array which is to be assigned to a vector
* size - number of elements from the beginning which has to be assigned.

<hr>

## Stack
* top() – Returns a reference to the top most element of the stack,**not pop it out** – Time Complexity : O(1)
* push(g) – Adds the element ‘g’ at the top of the stack – Time Complexity : O(1)
* pop() – Deletes the top most element of the stack – Time Complexity : O(1)

<hr>

## Queue
* front
* back
* push_back
* pop_front

<hr>

## Deque
* push_back   Add element at the end (public member function )
* push_front  Insert element at beginning (public member function )
* pop_back    Delete last element (public member function )
* pop_front   Delete first element (public member function )

Element access:
* operator[] Access element (public member function )
* at() Access element (public member function )
* front() Access first element (public member function )  
* back() Access last element (public member function )
<hr>

## SET
[docs](https://www.geeksforgeeks.org/set-in-cpp-stl/)
#### Insert
insert(const g) – Adds a new element ‘g’ to the set.
```
iterator set_name.insert(element)
iterator set_name.insert(iterator position, element)
iterator set_name.insert(iterator position1, iterator position2) 
```
#### Erase
1. setname.erase(position)
2. setname.erase(startingposition, endingposition)

#### Find
find(const g) – Returns an iterator to the element ‘g’ in the set if found, else returns the iterator to end.

#### Count
set_name.count(element) 
Parameters: The function accepts one mandatory parameter element which specifies the element whose count is to be returned.

Return Value: The function returns 1 or 0 as the set contains unique elements only. It returns 1 if the value is present in the set container. It returns 0 if it is not present in the container.

<hr>

## Unordered_map
Set set is an ordered sequence of unique keys whereas unordered_set is a set in which key can be stored in any order, so unordered.
Set is implemented as balanced tree structure that is why it is possible to maintain an order between the elements (by specific tree traversal). Time complexity of set operations is O(Log n) while for unordered_set, it is O(1).  

Methods on unordered_set
For unordered_set many function are defined among which most useful are size and empty for capacity, find for searching a key, insert and erase for modification.
The Unordered_set allows only unique keys, for duplicate keys unordered_multiset should be used.

If insert don't find, it will go to container.end()

#### Go over the set
```
  for (const string& x : mySet) { 
        cout << x 
             << " "; 
    }
```
 
<hr>

## Map
Red-Black Tree for keys. So insert and delete are all O(logN)

#### Insert
pair insert(keyvalue, mapvalue) – Adds a new element to the map
#### Erase
erase(const g)– Removes the key value ‘g’ from the map

erase(iterator position) – Removes the element at the position pointed by the iterator

#### Count
map count() function in C++ STL– Returns the number of matches to element with key value ‘g’ in the map.

#### Find
map find() function in C++ STL– Returns an iterator to the element with key value ‘g’ in the map if found, else returns the iterator to end.

Return Value: The function returns an iterator or a constant iterator which refers to the position where the key is present in the map. If the key is not present in the map container, it returns an iterator or a constant iterator which refers to map.end().

<hr>

## String

<hr>

## Unordered Map
When it comes to efficiency, there is a huge difference between maps and unordered maps.
We must know the internal working of both to decide which one is to be used.

| | map  | unordered_map |
| :--: | :--: | :--: |
| Ordering        | increasing  order(by default)   | no ordering |
| Implementation  | Self balancing BST like Red-Black Tree  | Hash Table |
| search time     | log(n)              | O(1) -> Average  O(n) -> Worst Case |
| Insertion time  | log(n) + Rebalance  | Same as search |                 
| Deletion time   | log(n) + Rebalance  | Same as search |
{:.mbtablestyle}

Unordered Map way to insert:

### Insert and [] works differently
#### []
it has overload the `[`  `umap[key] = val` 

operator[] returns a reference to the element you are searching for. When no element exists, it creates a new default element. (So requires default constructor)

When used to insert an element: myMap[key] = value;, the value will **override the old value** for the key.
    
#### Insert
using insert( pair<T,T>(key,value) 
`insert` Returns an iterator and a bool. The iterator is to the element. The bool indicates if a new element was inserted (true), or it already contained an element for the key (false).

Using insert doesn't require a default constructor.

When used to insert a new element: myMap.insert({key, value});, the **old value does not get updated** if key already exists in the map.

<hr>

## MultiMap
Multimap is similar to map with an addition that multiple elements can have same keys. Rather than each element being unique, the key value and mapped value pair has to be unique in this case.


<hr>

## Pair
This class couples together a pair of values, which may be of different types
A pair is a specific case of a std::tuple with two elements

Accessing with  **p.first** and **p.second** 

mymap.insert ( std::pair<char,int>('a',100) );

Example
```
for(auto i = 0 ; i < list2.size() ; i++ ){
    auto it = myMap.find(list2[i]);
    if(it != myMap.end()){
        if( it->second + i <= curMin){
            if (it->second + i < curMin){
                curMin = it->second + i;
                result.clear();
            }
            result.push_back(it->first);
        }
    }
}
```

<hr>

## Sort
Sort(Beign,End, comparator)


#### Comparator

* Define using a Funciton
```
bool cmp(int a, int b)
{
    return occurrences[a] < occurrences[b];
}

```

* Define using Lambda Function, the syntax begins with `[]`, after this is a normal function
```
void abssort(float* x, unsigned n) {
    std::sort(x, x + n,
        // Lambda expression begins
        [](float a, float b) {
            return (std::abs(a) < std::abs(b));
        } // end of lambda expression
    );
}
```
<hr>

## Priority Queue
```
template<
    class T,
    class Container = std::vector<T>,
    class Compare = std::less<typename Container::value_type>
> class priority_queue;
```

#### Functions
* top() accesses the top element 
* empty() checks whether the underlying container is empty 
* size() returns the number of elements 

* push() inserts element and sorts the underlying container 
* pop() removes the top element 