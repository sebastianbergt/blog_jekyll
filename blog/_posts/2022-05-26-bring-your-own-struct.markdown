---
layout: post
title:  "Bring your own struct to an unordered_map"
date:   2022-05-26 12:29:00 +0000
categories: cpp std container
---

Using a custom struct Point with an unordered_map.
XOR `^` is used on the two members of the struct, to create the hash for the map.

{% highlight c++ %}
#include <functional> // std::hash
#include <iostream> // std::cout
#include <string> // std::string
#include <unordered_map> // std::unordered_map

struct Point {
    std::size_t row;
    std::size_t col;

    bool operator==(const Point& p) const {
        return row == p.row && col == p.col;
    }
};

struct PointHash {
    std::size_t operator()(const Point& p) const noexcept {
        std::hash<std::size_t> hash_size_t;
        return hash_size_t(p.row) ^ hash_size_t(p.col);
    }
};

int main() {
    std::unordered_map<Point, std::string, PointHash> point_map;
    point_map[Point{1, 1}] = "start";
    point_map[Point{3, 3}] = "stop";

    std::cout << "from " << point_map[Point{1, 1}] << " to "
              << point_map[Point{3, 3}] << "\n";
}
{% endhighlight %}

Find the code editable on [Compiler Explorer](https://godbolt.org/z/bYW7fG6cf)

Here is another example for the unordered_set.

{% highlight c++ %}
#include <functional>     // std::hash
#include <iostream>       // std::cout
#include <string>         // std::string
#include <unordered_set>  // std::unordered_set

struct MyStruct {
    int integer;
    bool boolean;

    bool operator==(const MyStruct& s) const {
        return integer == s.integer && boolean == s.boolean;
    }
};

struct MyStructHash {
    std::size_t operator()(const MyStruct& s) const noexcept {
        std::hash<int> hash_int;
        std::hash<bool> hash_bool;
        return hash_int(s.integer) ^ hash_bool(s.boolean);
    }
};

int main() {
    std::unordered_set<MyStruct, MyStructHash> point_set;
    point_set.insert(MyStruct{1, true});
    point_set.insert(MyStruct{1, true});
    point_set.insert(MyStruct{3, true});

    std::cout << "number of points in set: " << point_set.size() << "\n";
}
{% endhighlight %}

Find the code editable on [Compiler Explorer](https://godbolt.org/z/xP1TfzKer)