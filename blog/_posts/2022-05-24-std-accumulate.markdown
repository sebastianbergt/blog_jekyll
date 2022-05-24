---
layout: post
title:  "std::accumulate"
date:   2022-05-24 12:34:00 +0000
categories: cpp stl
---

std::accumulate can be pretty useful to calculate a sum or a moving average.

{% highlight c++ %}
#include <algorithm>
#include <iostream>
#include <numeric>
#include <vector>

struct StructWithFloat {
    float f;
};

int main() {
    std::vector<StructWithFloat> c{StructWithFloat{1.0F}, StructWithFloat{2.0F},
                                   StructWithFloat{3.0F}};

    const auto sum = std::accumulate(
        c.begin(), c.end(), 0.0F,
        [](float sum, const StructWithFloat& s) { return sum + s.f; });

    std::cout << sum << "\n";
    return 0;
}
{% endhighlight %}

Find the code editable on [Compiler Explorer](https://godbolt.org/z/cT1vcrE55)
