---
layout: page
title: Set up a Swift project
---

Step by step instruction to set up a project on the command line using the Swift package manager.
Using the bowling kata setup as an example.
Tests with Quick and Nimble.

1. `mkdir kata`
2. `cd kata`
3. `swift package init --type executable`

*`--type executable` is important, otherwise the package is created as a library*

At this point the project structure looks like this:
`.
├── Package.swift
├── README.md
├── Sources
│   └── kata
│       └── main.swift
└── Tests
`
4. `cd Sources`
5. `mkdir bowling`
6. `cd bowling`
7. `touch bowling.swift`
Move back to the root folder

7. `cd Tests`
8. `mkdir bowlingSpec`
9. `cd bowlingSpec`
10. `touch bowlingSpec.swift`
Move back to the root folder

At this point the project structure looks like this:
`.
├── Package.swift
├── README.md
├── Sources
│   ├── bowling
│   │   └── bowling.swift
│   └── kata
│       └── main.swift
└── Tests
    └── bowlingSpec
        └── bowlingSpec.swift
`
11. `vim Package.swift`

Edit the file to include Quick and Nimble as dependencies.
The file should look like this in the end:

```swift
// swift-tools-version:4.0
// The swift-tools-version declares the minimum version of Swift required to build this package.

import PackageDescription

let package = Package(
    name: "kata",
    dependencies: [
        // Dependencies declare other packages that this package depends on.
        // .package(url: /* package url */, from: "1.0.0"),
        .package(url: "https://github.com/Quick/Quick.git", from: "1.2.0"),
        .package(url: "https://github.com/Quick/Nimble.git", from: "7.0.3"),

    ],
    targets: [
        // Targets are the basic building blocks of a package. A target can define a module or a test suite.
        // Targets can depend on other targets in this package, and on products in packages which this package depends on.
        .target(
            name: "kata",
            dependencies: ["bowling"]),
        .target(
            name: "bowling",
            dependencies: []),
        .testTarget(
            name: "bowlingSpec",
            dependencies: ["bowling", "Quick", "Nimble"]),
    ]
)

```

12. `swift package update`
13. `swift build`
14. `swift package generate-xcodeproj`
