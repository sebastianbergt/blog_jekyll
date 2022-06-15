---
layout: post
title:  "Binary search"
date:   2022-05-28 15:29:00 +0000
categories: cpp std container
---

In binary search we want to figure out at which index to find an exact value in a list or an array.
In this example we are searching for an integer value in a vector. Just imagine we do not know the values index;

{% highlight c++ %}
#include <algorithm> // std::generate, std::sort
#include <iostream> // std::cout
#include <random> // std::random_device, std::mt19937 std::uniforn_int_distribution
#include <stdexcept> // std::domain_error
#include <vector> // std::vector

class RNG {  // Random Number Generator
   public:
    RNG()
        : random_device_{},
          generator_{random_device_()},
          distribution_{0, 1000} {}
    int random() { return distribution_(generator_); }

   private:
    std::random_device random_device_;
    std::mt19937 generator_;
    std::uniform_int_distribution<int> distribution_;
};

std::size_t binarySearch(const std::vector<int>& v, int value) {
    std::size_t high = v.size();
    std::size_t low{};

    while (low <= high) {
        std::size_t mid = low + (high - low) / 2;

        if (v[mid] == value) {
            return mid;
        }

        if (v[mid] < value) {
            low = mid + 1;
        } else {
            high = mid - 1;
        }
    }
    throw std::domain_error("Binary search did not find the expected value.");
}

int main() {
    RNG rng;
    // create sorted vector of random integers:
    std::vector<int> v(10);
    std::generate(v.begin(), v.end(), [&rng]() { return rng.random(); });
    std::sort(v.begin(), v.end());

    // find value of index one
    std::cout << binarySearch(v, v[7]);
}
{% endhighlight %}

Find the code editable on [Compiler Explorer](https://godbolt.org/z/rzs4xxcq1)
