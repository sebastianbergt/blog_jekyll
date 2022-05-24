---
layout: post
title:  "Fluent Interface Builder"
date:   2022-05-24 08:34:00 +0000
categories: jekyll update
---

Fluent interfaces are great to make your code more expressive.

Which interface would you prefer to reason about correctness?

{% highlight c++ %}
    Pizza pizza{
        "thin dough", 
        "tomato sauce", 
        {"Onions", "Mushrooms", "Cheese"}, 
        DEFAULT_BAKING_TEMPERATURE_CELSIUS, 
        DEFAULT_BAKING_TIME_MINUTES
    }
{% endhighlight %}

or the next?

{% highlight c++ %}
    FluentPizza pizza;
    pizza.withDough("thin dough")
        .withSauce("tomato sauce")
        .withTopping("Onions")
        .withTopping("Mushrooms")
        .withTopping("Cheese")
        .bake(DEFAULT_BAKING_TEMPERATURE_CELSIUS, DEFAULT_BAKING_TIME_MINUTES);
{% endhighlight %}


Here is the full example:

{% highlight c++ %}
#include <iostream>  // std::cout
#include <sstream>   // std::stringstream
#include <string>    // std::string
#include <vector>    // std::vector

class FluentPizza {
   public:
    FluentPizza& withDough(const std::string dough) {
        dough_ = dough;
        return *this;
    };

    FluentPizza& withSauce(const std::string sauce) {
        sauce_ = sauce;
        return *this;
    };
    FluentPizza& withTopping(const std::string topping) {
        toppings_.push_back(topping);
        return *this;
    }

    FluentPizza& bake(const int temperature_celsius, const int time) {
        baking_temperature_ = temperature_celsius;
        baking_time_ = time;
        return *this;
    }

    std::string getRecipe() {
        std::stringstream recipe;
        recipe << "Roll out " << dough_ << " and put " << sauce_ << " on it.\n";
        recipe << "Add the following topics: \n";
        for (const auto& topping : toppings_) {
            recipe << "  " << topping << "\n";
        }
        recipe << "\nBake it at " << baking_temperature_
               << " degrees Celsius for " << baking_time_
               << " minutes. Enjoy!\n";

        return recipe.str();
    }

   private:
    std::string dough_;
    std::string sauce_;
    std::vector<std::string> toppings_;
    int baking_temperature_{250};
    int baking_time_{8};
};

int main() {
    constexpr int DEFAULT_BAKING_TEMPERATURE_CELSIUS{250};
    constexpr int DEFAULT_BAKING_TIME_MINUTES{8};

    FluentPizza pizza;
    pizza.withDough("thin dough")
        .withSauce("tomato sauce")
        .withTopping("Onions")
        .withTopping("Mushrooms")
        .withTopping("Cheese")
        .bake(DEFAULT_BAKING_TEMPERATURE_CELSIUS, DEFAULT_BAKING_TIME_MINUTES);

    std::cout << pizza.getRecipe();

    return 0;
}
{% endhighlight %}

Find the code editable on [Compiler Explorer](https://godbolt.org/z/M5so6jv4z)
