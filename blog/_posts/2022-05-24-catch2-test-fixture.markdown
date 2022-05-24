---
layout: post
title:  "Catch2 Test Fixture"
date:   2022-05-24 08:40:00 +0000
categories: cpp catch2 testing
---

Simplest possible Test Fixture with the excellent [Catch2 test framework](https://github.com/catchorg/Catch2).

{% highlight c++ %}
#include <catch2/catch_test_macros.hpp>
#include <string>

struct TestFixture {
    std::string str{"a"};
};

TEST_CASE_METHOD(TestFixture, "Description", "[function_under_test]") {
    CHECK(str.size() > 0);
}

// BDD style (Behavior Driven Development)
SCENARIO_METHOD(TestFixture, "Scenario Description") {
    GIVEN("a str") {
        WHEN("querying its size") {
            const std::size_t len = str.size();
            THEN("it is greater than zero") {
                CHECK(len > 0);
            }
        }
    }
}

int main() {} // prevents Compiler Explorer from adding its own stub
{% endhighlight %}

Find the code editable on [Compiler Explorer](https://godbolt.org/z/4T1WoEe1o)
