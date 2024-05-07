# Move blokchain from python to rust

https://medium.com/coinmonks/python-tutorial-build-a-blockchain-713c706f6531


# XC in Rust

https://github.com/joerdav/xc

Markdown defined task runner

----

# 7 more ideas

1. Web Scraper with Actix and reqwest:
   Build a web scraper using the Actix web framework for handling HTTP requests and reqwest for making requests. Create a tool that extracts information from websites, providing valuable insights or data for analysis. Focus on error handling, asynchronous programming, and creating a clean API for your scraper.

2. Command-Line Tool for File Encryption:
   Develop a command-line tool that encrypts and decrypts files using Rust’s cryptography libraries. Utilize the `crypto` crate to implement secure encryption algorithms. Ensure proper key management and explore the Rust standard library’s file I/O capabilities. This project will enhance your understanding of security practices in Rust.

3. Concurrency in Image Processing:
   Leverage Rust’s concurrency features to build an image-processing application. Explore parallelism by implementing filters and transformations on images using the `image` crate. Experiment with multithreading and asynchronous programming to optimize the processing speed for large datasets.

4. Blockchain Implementation with Rust:
   Dive into the world of blockchain by creating a simple blockchain implementation in Rust. Learn about cryptographic hash functions, data structures, and networking. Use the `tokio` library for asynchronous networking to build a decentralized network. This project will deepen your understanding of distributed systems and cryptography.

5. Personal Blog with Rocket Framework:
   Create a personal blog using the Rocket web framework. Implement features such as user authentication, CRUD operations, and Markdown rendering for blog posts. This project will give you hands-on experience with web development in Rust and the opportunity to explore the Rocket framework’s powerful features.

6. Chat Application with WebSocket:
   Develop a real-time chat application using Rust and the WebSocket protocol. Use libraries like `ws-rs` to handle WebSocket connections and build a simple chat server. Explore Rust’s ownership system to manage data shared between multiple clients. This project will deepen your understanding of networking and real-time communication.

7. Simulation Game with Amethyst Game Engine:
   Create a simulation game using the Amethyst game engine. Build an interactive and visually appealing experience that incorporates game mechanics, physics, and graphics. Amethyst allows you to focus on game development without sacrificing performance, making it an ideal choice for exploring Rust in the gaming domain.



-----

# from easier to advanced 1-by-1


I’ve broken this down into 3 sections.

Beginner projects which have extra instructions and links to similar projects so that you can follow along and build the projects
Intermediate projects for you to build and figure out for yourself (with the help of a few hints)
Advanced projects for you to do on your own, from scratch, with very limited instructions
Will this be harder and more frustrating for you than it would be if you just had instructions to follow? Yes, but that's the point!

Trust me on this. The struggle is where you learn the most, because it’s where you start to apply theory and knowledge.

It’s not a bad thing to struggle either, because this struggle to figure things out is the most accurate representation of the workplace, and even the best Rust developers struggle and have to think through problems (that's what you get paid the big bucks for!).

But rest assured that these projects will help you become a better Rust developer, whether you are starting out as a new developer or are an experienced Rust developer looking to dig deeper.

Sidenote: Each project has a brief description along with a list of crates to help you make the project. If you want to challenge yourself, complete the "extra features" section to give yourself some more practice.

Before we begin - some foundation crates
The Rust standard library is light on features, so here are some foundation crates that are great to use in nearly any project:

thiserror (Make custom error types)
color-eyre (Report application errors)
clap (Process command line arguments)
serde (Serialize and de-serialize data structures)


## Beginner Rust Projects
OK so let’s get into these and start nice and simple. These beginner Rust projects are great for developers who are starting out with programming, but they’re also good for experienced developers who are just starting to play around with Rust.

Why?

Well, each of these projects focus on a single, well-defined task, making them great for new developers. Whereas for experienced developers starting to use Rust, these help to gain understanding of the workflows and idioms used in Rust.

I've included a link below each of these beginner projects. That link contains instructions on a project that is the same (or similar) to what I'm suggesting you build here. So if you're struggling with something check out the link.

But don't forget - the goal is for you to be able to build projects without needing detailed instructions, so try to think through the problem before you look at the instructions.

When we get to the intermediate and expert projects (and in the real world), you won't have instructions as a crutch to lean on!

### 1. CLI Utilities
blog post image
Command-line utilities are the gateway to learning any programming language. They have simplified input and output systems which makes the code less complex to write. This allows you to focus on the problem at hand and learn Rust instead of learning a specific problem domain.

Rewriting existing tools in Rust can provide a great baseline to get started:

echo: repeats input
cat: concatenates files
ls: lists directories
find: locates files or directories
grep: matches text in files
Other than the "foundation" crates listed at the beginning of this post, these projects don't need any extra crates.

Level up your skills and project by adding in these extra features:
Add extra flags to the tools to enhance their feature set
Use proper error handling so the programs don't end prematurely (no .unwrap() and no .expect())
Add multi-threading support. find and grep are good candidates for this
Add colorized output
Having trouble getting started? Check out this Rust coreutils rewrite which has many of the above tools written in Rust.

### 2. Todo List
blog post image
The humble todo list has become an unofficial first project when starting out with a new programming language. You can choose to use a CLI with clap, or make it interactive using the crates listed below. You can even do both!

The todo list minimal feature set includes:

Add, remove, edit todos
Mark todos as "done"
Save and load todos
Here are some crates to get you started:

console (Interface with terminal)
dialoguer (Terminal user prompting)
tui (Interactive terminal UI)
rusqlite (Connect to SQLite databases)
Level up your skills and Todo List with these extra features:
Make your project fully interactive
Add $EDITOR support for editing todos
Save the todos into a database
Not sure how to start out with this project? Check out this CLI todo app.

### 3. Budget Manager
blog post image
A budget manager is a project that can be useful for keeping your finances in check, while also improving your Rust programming skills. Learning about databases is an important skill, and this project is a great entry point for databases thanks to its limited scope.

For an initial interface, you can use clap to handle CLI flags and arguments, but you can also go interactive using the crates listed later in this section.

The budget manager minimal feature set includes:

Add, remove, edit a budget
Add, remove, edit transactions associated with a budget
Display how much money is remaining in a budget
Handle more than one budget
Here are some crates to get you started:

console (Interface with terminal)
dialoguer (Terminal user prompting)
tui (Interactive terminal UI)
rusqlite (Connect to SQLite databases)
Level up your skills and your Budget Manager project with these extra features:
Provide a web API to make changes to the budget
Display a warning when the user is getting close to going over budget
Allow budgets to be active for a specified time frame
Generate reports summarizing expenditures in a given time frame
If you want to see an alternative of managing bills or budgets, take a look at this project.

### 4. Unit Converter
blog post image
A unit converter provides useful functionality, and is a nice way to get familiar with converting between different data types in Rust. An argument-based CLI using clap is a great starting point for this application.

Here are some crates to get you started:

console (Interface with terminal)
dialoguer (Terminal user prompting)
tui (Interactive terminal UI)
Level up your skills and your Unit Converter project with extra features:
Use new types for each unit
Add conversion functionality using the From trait to make your code more robust
Add conversions for lots of units
Add an "expression" option where the user can type a conversion expression
Example: 10C -> F would convert 10° Celsius to Fahrenheit
If the units are ambiguous, display an error message showing potential matching units so the user can correct the expression
There is an interactive implementation of a unit converter called rink-rs that you can browse the code for if you want to see how a unit converter can be built.

## Intermediate Rust Projects
These intermediate Rust projects are great once you have a grasp of how Rust works and have completed some beginner projects.

Generally, each project has either more than one feature to manage, or use more complicated parts of the Rust language. Getting through these projects will increase your confidence and ability to program in Rust.

As you'll see, for these intermediate projects I'm not providing links to sites that provide instructions on how to build the project.

That's up to you to figure out. Building projects from scratch is an incredible learning experience, so embrace it!

### 5. Social Media Bot
blog post image
A social media bot is a fun project that can allow you to show off your skills. You could make a Discord bot to moderate your server, or a Reddit bot that replies with fun trivia, or even a Twitter bot that tweets some message based on a condition.

Here are some crates to get you started:

reqwest (Make HTTP requests)
serenity (Discord)
kuon (Twitter)
twitter-api (Twitter)
reddit (Reddit)
Although I’ve not added a direct link to how to build this, a little detective work will set you on the right path if you get stuck.

Level up your skills & your Social Media Bot with extra features:
Fully automate the bot
Make a text adventure game where the bot responds to commands in the form of replies
Connect the bot to an external event source: an API, cron job, or even a hardware project

### 6. Metered API Server
APIs are great, and making money from them is even greater! Create a web-accessible API and track usage per API key. The API can do anything that is interesting to you.

There's no need to connect your API to a billing source. The important part is to get correct usage tracking and database planning.

The metered API server minimal feature set includes:

Add/remove API keys
Track the number of times each API key has made a request
Associate API keys with user identity (email address, for example)
Here are some crates to get you started:

sqlx (Connect to databases)
warp (HTTP Server)
Level up your skills and your API Server with extra features:
Generate usage reports per API key in any format you'd like
Add support for usage quotas. This is great for tiered-usage.
Add rate limiting to prevent API overuse
#7. Data Structures
Choosing the correct data structure is one of the most important tasks when creating programs. Writing your own data structure is a great way to gain a deeper understanding of how they work.

You could try creating an ordered HashMap, or a concurrent Queue, a binary search tree, or a persistent data structure.

Here are some crates to get you started:

petgraph (Helps make graph data structures)
Level up your skills with extra features:
Write a developer-friendly API to use your data structure
Write documentation comments for the entire crate
Benchmark your data structure against similar ones in the ecosystem

### 8. Utility Billing System
blog post image
This project is about creating a billing system for a utility company. It can be any utility that has metered usage. You'll need to support customer accounts, meter readings, generation of bills, and due dates.

The great thing about this project is you can start out using CLI and then transition it to a web-based system for an extra challenge.

The utility billing system minimal feature set includes:

Managing customer accounts with CRUD operations
Ingesting meter readings from an external service (CLI argument is fine for an initial implementation)
Generate reports: usage, income
Generate invoices
Here are some crates to get you started:

sqlx (Connect to databases)
console (Interface with terminal)
dialoguer (Terminal user prompting)
tui (Interactive terminal UI)
axum (HTTP Server, general usage)
Level up your skills and your Billing System with extra features:
Make it a web-based system instead of CLI
Enable customers to create accounts and pay their bill
Send notifications (email/SMS/push) to customers when their bill is due
Allow customer to configure notifications (bill due, usage above threshold)



### 9. Prescription Management System

blog post image
Writing programs to handle medical information needs extra care to avoid security problems. This makes Rust's secure, reliable code the perfect fit for creating a prescription management system.

To help ensure correct operation, try using the type state pattern to encode the correct operation of the application into the type system.

The system should:

Allow doctors to add prescriptions for patients
Allow pharmacy personnel to note if the patient accepted or declined a consult
Track number of refills based on what the doctor prescribed
Have either a CLI using clap or an interactive terminal interface of your own design
Here are some crates to get you started:

- sqlx (Connect to databases)
- console (Interface with terminal)
- dialoguer (Terminal user prompting)
- tui (Interactive terminal UI)
- axum (HTTP Server, general usage)

Level up your skills and your Prescription Management System project with extra features:
Generate reports showing the quantities of medicines prescribed
Add a web interface to the system with feature parity to the CLI
Pass these acceptance tests:
Prescription cannot get filled without associated doctor
Prescriptions can get filled when there are refills remaining
Prescriptions cannot get filled when there are no refills remaining
Prescriptions cannot get filled if the current date is past the latest refill date
All prescription refills must note if the patient accepted or declined the consult
Do not allow prescription to get filled when over a defined limit (example: max 30 tablets) in any given time frame (example: 30 days). This applies per-patient and per medicine
Partial refills respect refill date limits
Partial refills respect refill quantities
Partial refills respect defined limits


### 10. Spell Checker Crate
blog post image
The humble spell checker is an important bit of code that runs in the background doing its job. This project doesn't take much explanation: make a crate that takes an input string and then determine if the string has the correct spelling.

A minimal implementation can use a HashMap, but the "intermediate" level starts with the extra features.

Here are some crates to get you started:

unicode-segmentation (Unicode text processing)
Level up your skills and Spell Checker with extra features:
Use a bloom filter to reduce memory usage
Offer suggestions for misspelled words using fuzzy matching (bonus points for writing your own fuzzy matching algorithm)
Write documentation comments for the crate
Make your crate release-ready by going through the API checklist
Benchmark your crate against others in the ecosystem
Advanced Rust Projects
These projects offer a significant challenge. While the beginner and intermediate projects primarily focused on a small number of features in a specified area, these projects require knowledge in more than one discipline, and are feature-rich.

Simply put, if you can build these projects then you know Rust!

### 11. File Format Parser
Write a parser to parse a file format of your choosing. File formats can get complicated, so starting with a minimal text-based format such as INI is a good idea. Don't stop at INI though! There are a lot of file formats you can choose from.

You'll need one crate for this project: nom. The standard library has you covered for the remaining functionality.

Level up your skills with extra features:
Parse a binary format
Parse a programming language
Create new types for your fields (don't "parse" into a bunch of &str, for example)
Benchmark the performance of your crate versus others in the ecosystem (if one exists)
Create a test suite to show that your parser works as expected

### 12. Audio Player
blog post image
Audio players touch a significant number of software development aspects:

file formats
encoding and decoding
digital signal processing
interface development, and
real-time processing
For this project, create a minimal audio player with a GUI that plays a music file.

The audio player API minimal feature set includes:

Play/Pause the audio
Show the length of the audio file and current play progress
Use egui to create your interface. egui has pre-made widgets for a minimal interface and also supports writing your own widgets for more advanced elements.

The rust.audio website has a collection of audio-related Rust crates that you can choose from your project.

Level up your skills and your Audio Player project with extra features:
Add playlist support
Allow seeking to different parts of the audio
Add a media library
Allow editing metadata of the files in the media library
Add a parametric equalizer
Add fun visualizations

### 13. Scripting Language
There's no shortage of different programming and scripting languages available. But sometimes it's helpful to have one designed for your specific situation.

Create your own interpreter for a scripting language of your own design. If you are unfamiliar with this topic, check out this site for a comprehensive tutorial on creating a scripting language in Rust.

Here are some crates to get you started:

nom (Binary parser)
Level up your skills with extra features:
Write syntax highlighting definitions for your favorite editor
Create a Rust integration for your scripting language so you can embed the language into a Rust program

### 14. ACID Compliant Database
Yes, making your own database is a thing you can do in Rust. Don't worry, it doesn't need to be a competitive fully-featured database. A key/value store is adequate, but you can also make a time series database or a document database.

The important part is making sure that your database meets ACID compliance. Ensuring that your database is 100% reliable is a difficult problem, so start with a minimal design and expand from there.

Here are some crates to get you started:

petgraph (Helps make graph data structures)
Level up your skills and your database with extra features:
Write a query system
Allow concurrent access

### 15. Specialized Compression Algorithm
blog post image
Compression algorithms work best when they target a specific kind of data. Design your own compression algorithm for data of your choice, and then implement it as a Rust crate. You can start with a static-dictionary algorithm to get a feel for how compression works, and then move on to more advanced techniques.

Here are some crates to get you started:

itertools (More Iterator adapters)
The standard library windows and chunks methods on slice will also be useful.

Level up your skills and your algo with extra features:
Benchmark your algorithm against similar algorithms
Parallelize your algorithm

### Bonus project: Build a Full-Stack Twitter clone in Rust
blog post image

Fancy a more advanced project? How about building a Full-Stack Twitter clone!

As part of the new 'portfolio projects' event that ZTM is running here for Summer, you can get access to this complete project (with code) as a ZTM member.

Check out the project here.

So what are you waiting for? It’s time to start building!
So there you have it - 15 of my favorite Rust practice project ideas that you can use to create a killer portfolio to improve your Rust skills faster and more efficiently.

Whether you are a beginner or an experienced programmer, working on projects is the best way to learn the Rust programming language. Especially when you push yourself to learn some Google fu, and figure things out for yourself.

You’ll grow in confidence and expertise, and wow any potential employers, and it’s a core skill that makes the difference between a beginner and a kick ass coder.

So pick a project and start working your way it. Start by building the basics, add in some extra features and keep on pushing yourself. You got this!

P.S. If you find yourself stuck, if you’re just starting out, or if you simply want to skill up even further, then come and check out my Complete Rust Programming course.

It’s designed to take you from absolute beginner to getting hired, so there’s something to learn from almost any skill level, with beginner to advanced projects that will help you stand out from the crowd.




see [https://zerotomastery.io/blog/rust-practice-projects/](https://zerotomastery.io/blog/rust-practice-projects/)

------



