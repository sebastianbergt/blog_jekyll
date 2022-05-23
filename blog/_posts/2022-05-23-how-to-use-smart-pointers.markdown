---
layout: post
title:  "How to use smart pointers?"
date:   2022-05-23 10:16:48 +0000
categories: modern cpp smart_pointers
---
Just typed up this small example of using smart pointers in [Compiler Explorer](https://godbolt.org/z/MT937PdWc)

Most important takeaways:
* pointer signals association
* std::unique_ptr signals ownership
* std::shared_ptr signals shared ownership

{% highlight c++ %}
#include <iostream> // std::cout
#include <memory> // std::unique_ptr<> std::make_unique<> 
                  // std::shared_ptr<> std::make_shared<>
#include <tuple> // std::ignore

int main() {
    const int i{42};
    const int* ptr = &i; 
    
    std::unique_ptr<int> unique_ptr = std::make_unique<int>(42);

    // can not be copied and needs to be moved
    std::unique_ptr<int> copy_of_unique_ptr = std::move(unique_ptr);

    if( copy_of_unique_ptr && *copy_of_unique_ptr == 42) {
        std::cout << "unique_ptr is 42.\n";
    }

    std::shared_ptr<int> shared_ptr = std::make_shared<int>(43);

    if( shared_ptr && *shared_ptr == 43) {
        std::cout << "shared_ptr is 43.\n";
    }

    // avoid warning: unused variable
    std::ignore = ptr;
    
    return 0;
}
{% endhighlight %}
