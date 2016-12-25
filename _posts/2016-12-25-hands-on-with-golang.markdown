---
layout: post
title: "Hands on with GO language"
date: 2016-12-25 00:00:00
categories: tech post
---

Basic Golang setup.
------------------
- Best way to install go in Mac is download [official dmg](https://golang.org/dl/) file and install.
- Check that Go is installed correctly by setting up a workspace and building a simple 'hello world' program.
- Create a workspace directory for go project. set the **GOPATH** environment       variable to point to that location.
    ``` mkdir go_work
    ```
    and
    ```export GOPATH=$HOME/go_work
    ```
- Next, make the directories src/github.com/user/hello inside your workspace
- Inside the hello directory create a file named hello.go with the following contents:

{% highlight go %}
    package main

    import "fmt"

    func main() {
        fmt.Printf("hello, world\n")
    }
{% endhighlight %}

- Then compile with ```go install github.com/user/hello```. This will create an executable file _hello_ under **bin** directory of workspace.
- Next, command to execute hello
   {% highlight go %}
   $ $GOPATH/bin/hello
   hello, world
   {% endhighlight go %}
