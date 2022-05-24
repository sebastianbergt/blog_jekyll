---
layout: post
title:  "Strong types"
date:   2022-05-24 11:34:00 +0000
categories: cpp design_pattern
---

Strong types can make your code much more readable.

{% highlight c++ %}
#include <utility> // std::move
#include <iostream> // std::cout

// Copied from https://www.fluentcpp.com/2016/12/08/strong-types-for-strong-interfaces/
template <typename T, typename Parameter>
class NamedType
{
public:
    explicit NamedType(T const& value) : value_(value) {}
    explicit NamedType(T&& value) : value_(std::move(value)) {}
    T& get() { return value_; }
    T const& get() const {return value_; }
private:
    T value_;
};

// here starts the fun

using Width = NamedType<float, struct WidthParameter>;
using Height = NamedType<float, struct HeightParameter>;
using Area = NamedType<float, struct AreaParameter>;

class Rectangle{
    public:
    Rectangle(Width width, Height height) :width_{width}, height_{height}  {}

    Area calcArea() {
        return Area{width_.get()*height_.get()}; 
    };

    private:
    Width width_;
    Height height_;
};

int main() {
    Rectangle rect{Width{3.0F}, Height{2.0F}};
    std::cout << "Area calculated is " << rect.calcArea().get();

    return 0;
}
{% endhighlight %}

Find the code editable on [Compiler Explorer](https://godbolt.org/z/nq8nbTMW5)
