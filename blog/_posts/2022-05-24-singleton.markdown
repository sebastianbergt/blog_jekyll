---
layout: post
title:  "Singleton"
date:   2022-05-24 10:34:00 +0000
categories: cpp design_pattern
---

Often the Singleton is nowadays considered an Anti-Pattern.
Do NOT use it if you don't absolutely have to.

{% highlight c++ %}
#include <iostream>

class Singleton {
   public:
    static Singleton& instance() {
        static Singleton singleton;
        return singleton;
    }

   private:
    Singleton() = default;
};

int main() {
    const auto& singleton1 = Singleton::instance();
    const auto& singleton2 = Singleton::instance();

    if (&singleton2 == &singleton1) {
        std::cout << "both singletons point to the same object.\n";
    }
    return 0;
}
{% endhighlight %}

Find the code editable on [Compiler Explorer](https://godbolt.org/z/G1svnsjqq)
