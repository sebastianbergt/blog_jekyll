---
layout: post
title:  "Using try-catch in C++."
date:   2022-05-24 08:34:00 +0000
categories: cpp try catch error_handling
---

{% highlight c++ %}
#include <iostream>  // std::cerr
#include <stdexcept>   // std::runtime_error, std::domain_error

int main() {
    try {
        throw std::runtime_error("I am a std::runtime_error.");
    } catch(const std::runtime_error& e) {
        std::cerr << "Caught runtime error: " << e.what() << "\n";
    }

    std::cerr << "------------------------------------------\n";

    try {
        throw std::domain_error("I am a std::domain_error.");
    } catch(const std::runtime_error& e) {
        std::cerr << "Caught runtime error: " << e.what() << "\n";
    } catch(const std::domain_error& e) {
        std::cerr << "Caught domain_error error: " << e.what() << "\n";
    } catch(...) {
        std::cerr << "Catch all triggered\n";
    }

    return 0;
}
{% endhighlight %}

Find the code editable on [Compiler Explorer](https://godbolt.org/z/vMK48oeq3)
