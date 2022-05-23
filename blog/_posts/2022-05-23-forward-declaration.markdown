---
layout: post
title:  "Forward declaration"
date:   2022-05-23 10:20:00 +0000
categories: jekyll update
---
Helpful technique to keep member variables private.
Find the code editable on [Compiler Explorer](https://godbolt.org/z/1roePvY9b)

{% highlight c++ %}
// Could be put into a header file to
// AVOID making ForwardDeclaredClass a public dependency
#include <memory>  // std::unique_ptr
#include <tuple> // std::ignore

namespace util {
class ForwardDeclaredClass;  // forward declaration
}  // namespace util

class Usage {
   private:
    std::unique_ptr<util::ForwardDeclaredClass> ptr_;
};

// Could be put into cpp file:
namespace util {
class ForwardDeclaredClass {
    // actual implementation
};
}  // namespace util

int main() {
    Usage usage{};

    std::ignore = usage;
    return 0;
}
{% endhighlight %}
