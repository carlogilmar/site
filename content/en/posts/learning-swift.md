+++
title = "Learning Swift"
tags = ["swift"]
categories = ["programming"]
date = "2018-07-05"
+++

# Learning Swift

#### Recently I have been studying about Swift. I think that the learning experience is a wonderful resource for other persons who are in the same situation, so I consider valuable talk about this.

## First Steps

When I start to learn swift I decide to watch videos and demonstrations about how to use the Xcode for build iOS apps, but, for be honest, I couldn’t understand some things: optionals, constrains, auto layout… This was my first experience. So I decide start with basics.

I considered that know and understand the language is important, and Swift is a language with a lot of features. The plan was know more about it, and then introduce me to iOS apps development.

### Installation

By default you have swift installed in you MacOs, but you can install Swift in a linux SO: [Installing Swift Oficial Website](https://swift.org/getting-started/#installing-swift)

My first professional language is groovy, so was normal for me find some concepts in this new language. The first things that I found about swift were:

- Xcode for build iOS apps
- Playgrounds

But this aren’t the only tools!

### Meeting Swift in Command Line

I think that use the command line is better for developer life, fortunately swift is a scripting language, and it have a REPL. I found it more useful in this part of my life instead of use only playgrounds, although this are visual and have a great visual debugger, also are slow in some cases.

You can write some like this:

```
print("Hello World from this swift script! 😎")
```

Save as `hello.swift` and run with `swift hello.swift`

*If you have a lot of warnings in your REPL, you have to configure your Xcode Command Lines. Go to Xcode > Preferences > Command Line Tools and select your Xcode version*

Also swift have a own package for build apps. Remember that swift is general-purpose programming language, so you can build web apps too using [Vapor Swift Frameworks](https://vapor.codes/) for example.

This package manager is useful in the command line:

> mkdir sample-app && cd sample-app

> swift package init

```
 ~/D/sample-app >>> swift package init
Creating library package: sample-app
Creating Package.swift
Creating README.md
Creating .gitignore
Creating Sources/
Creating Sources/sample-app/sample_app.swift
Creating Tests/
Creating Tests/LinuxMain.swift
Creating Tests/sample-appTests/
Creating Tests/sample-appTests/sample_appTests.swift
Creating Tests/sample-appTests/XCTestManifests.swift
```

We could make the follow things:

- Test this project with `swift package test`
- Built this project with `swift build`
- Create the Xcode project for use it `swift generate-xcodeproj`
- Use the [LLDB Debugger](https://swift.org/getting-started/#using-the-lldb-debugger)

### A text editor for Swift

You can use your own text editor! I use VIM in my day by day, and I’m in love because it’s a useful tool for me. So I decide to use it with this plugin for swift syntax [Swift VIM Plugin](https://vimawesome.com/plugin/swift-vim-red).

### The language

The swift language is elegant. It’s a dynamic language, so it could be typed or inferred.  Sometimes looks like C, and you can implement some programming approaches as structured, oriented programming and functional programming, have pattern matching, protocols extensions, generics, and more features.

I found interesting make some like this:

``` swift
struct Student{
    var name: String
    var age: Int
}

struct ListOfSomething<T>{

    var list:[T] = []

    mutating func setList(elementList:[T]){
        self.list = elementList
    }

    func countElements() -> Int{
        return self.list.count
    }
}

// A list of students
var student1 = Student(name: "carlo", age: 20)
var student2 = Student(name: "gilmar", age: 21)
var student3 = Student(name: "carlogilmar", age: 22)
var studentsList:[Student] = [student1, student2, student3]

// A list of something of students
var buffer = ListOfSomething<Student>()
buffer.setList(elementList: studentsList)
buffer.countElements()
```


### My Resources for learn

For understand this language I try to start with the basics, so I use the follow books for know more about swift:

- [Mastering Swift 4](https://www.packtpub.com/application-development/mastering-swift-4-fourth-edition) (Difficult:⭐️ /5, Content:⭐️⭐️⭐️⭐️/5)
-  [Swift Data Structure and Algorithms ](https://www.packtpub.com/application-development/swift-data-structure-and-algorithms) (Difficult:⭐️⭐️⭐️ /5, Content:⭐️⭐️⭐️⭐️/5)
- [Classic Computer Science Problems in Swift](https://www.manning.com/books/classic-computer-science-problems-in-swift) (Difficult:⭐️⭐️⭐️⭐️ /5, Content:⭐️⭐️⭐️⭐️/5)

Now, I’m learning about iOS development, and I found some interesting resources:

- [Let’s Build That App Site](https://www.letsbuildthatapp.com/) [Brian Voong ](https://twitter.com/buildthatapp) help us to understand easily how to build an iOS application without storyboards, and talk about some tips, he have a lot of videos for learn and learn!
-  [Swift by Sundell Blog and Podcast](https://www.swiftbysundell.com/)  [John Sundell](https://twitter.com/johnsundell) write about swift, and he have a lot of content about testing!
- [The Learn Swift Podcast](https://learnswift.fireside.fm/)
- [Consult Podcast](http://consultpodcast.com/)  [David Kopec ](https://twitter.com/davekopec) talk about interesting things about swift!

This is my learning experience as beginner learning Swift. I hope to help you with start you own way of learn.

## Thanks for read it!



