---
layout: post
title:  "Container:Vector"
categories: CPP
---

## Copying Vectors

1. Method 1 : Iterative method. This method is a general method to copy, in this method a loop is used to push_back() the old vector elements into new vector.They are deeply copied
2. Method 2 : By assignment “=” operator. Simply assigning the new vector to old one copies the vector. This way of assignment is not possible in case of array
```
  // Using assignment operator to copy one 
    // vector to other 
    vect2 = vect1; 
```
You are making a **deep copy** any time you copy a vector. But if your vector is a vector of pointers you are getting the copy of pointers, not the values are pointed to

3. Method 3 : By passing vector as constructor. At the time of declaration of vector, passing an old initialized vector, copies the elements of passed vector into the newly declared vector. They are deeply copied.
```
vector<int> vect1{1, 2, 3, 4}; 
// Declaring new vector and copying 
// element of old vector 
// constructor method, Deep copy 
vector<int> vect2(vect1); 
```
