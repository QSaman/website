---
title: Memory allocation in std::optional
date: 21:44 06/12/2021 

hero_classes: text-light title-h1h2 overlay-dark-gradient hero-large parallax
show_sidebar: true
hero_image: header.jpg

taxonomy:
    category: en_blog
    en_tag: [C++, programming, memory management]
---
C++17 introduces `std::optional`. Before C++17 we could use `Boost.Optional`. The question is when we should use smart pointers and when to use `std::optional`?

===

## Memory allocation

For smart pointers we have a lot of choices: `std:unique_ptr`, `std::shared_ptr` and `std::weak_ptr`. This [website](https://www.nextptr.com/tutorial/ta1450413058/unique_ptr-shared_ptr-weak_ptr-or-reference_wrapper-for-class-relationships) explains the differences and when to use them.

I'm going to focus on memory allocation differences between `std::optional` and smart pointers. According to [cppreference](https://en.cppreference.com/w/cpp/utility/optional):

<blockquote>
Any instance of optional<T> at any given point in time either contains a value or does not contain a value.

If an optional<T> contains a value, the value is guaranteed to be allocated as part of the optional object footprint, i.e. no dynamic memory allocation ever takes place. Thus, an optional object models an object, not a pointer, even though operator*() and operator->() are defined.
</blockquote>

There are two implications here:
1. when `std::optional` is created, the memory is allocated for `T`. Unlike smart pointers, [lazy initialization](https://en.wikipedia.org/wiki/Lazy_initialization) is not possible
2. `T` may not have default constructor. So `std::optional` must handle this situation

Based on these facts when lazy initialization is important (e.g. the size of `T` is huge or most of the time `std::optional` doesn't have a value), it's better to use smart pointers; otherwise use `std::optional`. For more information read this [answer](https://stackoverflow.com/a/44856779).

## High-level implementation details

The second implication is a little tricky. I've looked at `libstdc++` and I've noticed they address it by using [Unions](https://en.cppreference.com/w/cpp/language/union):

<blockquote>
The union is only as big as necessary to hold its largest data member. The other data members are allocated in the same bytes as part of that largest member. The details of that allocation are implementation-defined but all non-static data members will have the same address (since C++14). It's undefined behavior to read from the member of the union that wasn't most recently written. Many compilers implement, as a non-standard language extension, the ability to read inactive members of a union.
</blockquote>

I wrote a simplified example that shows the idea behind it:

```
#include <iostream>
#include <cstdint>

struct Data
{
  Data(std::int64_t data_) : data(data_) {}
  std::int64_t data;
};

struct EmptyByte { };

union Storage
{
  Storage() : zeroByte()
  {
    std::cout << "zeroByte is initialized" << std::endl;
  }

  Storage(std::int64_t data_) : data(data_)
  {
    std::cout << "data is initialized" << std::endl;
  }

  EmptyByte zeroByte;
  Data data;
};

int main()
{
  std::cout << "Data size: " << sizeof(Data) << std::endl;            // Data size: 8
  std::cout << "EmptyByte size: " << sizeof(EmptyByte) << std::endl;  // EmptyByte size: 1
  std::cout << "Storage size: " << sizeof(Storage) << std::endl;      // Storage Size: 8

  Storage s1;     // zeroByte is initialized
  Storage s2(7);  // data is initialized
}
```

As you can see `Data` doesn't have a default constructor but `Storage` union takes care of it. So `union` addresses the second implication.

For addressing the first implication, `std::optional` is using a technique named [placement new](https://en.cppreference.com/w/cpp/language/new) to use a pre-allocated memory:

```
// within any block scope...
{
    alignas(T) unsigned char buf[sizeof(T)];
    // Statically allocate the storage with automatic storage duration
    // which is large enough for any object of type `T`.
    T* tptr = new(buf) T; // Construct a `T` object, placing it directly into your 
                          // pre-allocated storage at memory address `buf`.
    tptr->~T();           // You must **manually** call the object's destructor
                          // if its side effects is depended by the program.
}                         // Leaving this block scope automatically deallocates `buf`.
```
As you can see the implementation details are elegant and easy to understand.
