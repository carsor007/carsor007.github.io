---
layout: post
type: post
tags:
  - C++
  - namespace
  - fundamentals
published: true
title: C++ namespaces
---
## C++ Refresher - Namespaces

In C++ Namespaces are mostly used to avoid name conflicts. For example, you might be writing some code that has a function called draw() and there is another library available with the same function draw(). Now the compiler has no way of knowing which version of draw() function you are referring to within your code.
A namespace is designed to overcome this difficulty and is used as additional information to differentiate similar functions, classes, variables etc. with the same name available in different libraries. 
  Example:
  
> namespace graphics
   {
     void draw()
     {
     // function graphics::draw()
     }
   }
   
 > namespace ui
   {
      void draw()
      {
      //function ui::draw()
      }
   }


 This explains why you type  using namespace std; in programs that reference entities in the standard library. If you don't write  using namespace std; you will have to explicitly add  std:: before anything in the std namespace.
 
 Example:
 
 > std::cout << "Hello World!" << std::endl;
 
 > std::string s;
  
 > std::vector<std::string vs;
 
 Some people prefer to write > std::
 Also if you only need a few distinct functions, you can tell it just that. For example, if you only plan on using cout and endl, there is no need to use the entire std namespace. Instead:
  > using std::cout;
    
  > using std::endl;



A good example of the correct use of namespaces in C++ is the C++ Standard Library. Everything in this quite large library is placed in a single namespace called std - there is no attempt or need to break the library up into (for example) an I/O sub-namespace, a math sub-namespace, a container sub-namespace etc.
