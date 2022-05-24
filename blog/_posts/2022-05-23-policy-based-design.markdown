---
layout: post
title:  "Policy Based Design"
date:   2022-05-23 13:20:00 +0000
categories: jekyll update
---

The "O" in the [SOLID principles](https://medium.com/mindorks/solid-principles-explained-with-examples-79d1ce114ace) refers to the "open-closed principle".
Which is short for "open for extension, but closed for modification".

Policy based design is one useful pattern to change the behavior of a class (here Banana) without modifying the class itself.

Find the code editable on [Compiler Explorer](https://godbolt.org/z/hMjGGEo93)

{% highlight c++ %}
#include <fstream> // std::fstream
#include <iostream> // std::cout

template <class LoggingPolicy>
class Banana {
   public:
    Banana() { LoggingPolicy::log("Banana was created."); }
};

class ShellLogging {
   public:
    static void log(const std::string& s) {
        std::fstream fs;
        fs.open("log.txt", std::fstream::app);
        fs << s;
        fs.close();
    }
};

class FileLogging {
   public:
    static void log(const std::string& s) { std::cout << s; }
};

int main() {
    Banana<ShellLogging> banana1;
    Banana<FileLogging> banana2;

    return 0;
}
{% endhighlight %}
