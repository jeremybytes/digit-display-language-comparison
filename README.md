# Digit Display - Comparing 3 Language Implementations
This code was written as an exercise to learn some differences between Rust (rust-lang), Go (golang), and C# (csharp). The same problem and general solution was implemented in all 3 languages/environments.

**UPDATE**  
The submodules on this repo are not working at the moment. (I'm new to submodules, and it was probably a bad idea.) In the meantime, feel free to visit the individual repos:  
* Rust: [https://github.com/jeremybytes/digit-display-rust-lang-comparison](https://github.com/jeremybytes/digit-display-rust-lang-comparison)
* Go: [https://github.com/jeremybytes/digit-display-golang-comparison](https://github.com/jeremybytes/digit-display-golang-comparison)
* C#: [https://github.com/jeremybytes/digit-display-csharp-comparison](https://github.com/jeremybytes/digit-display-csharp-comparison)

**Program Purpose:**

Naive hand-written digit recognition with display applications to show image, prediction, and errors.  

The original .NET (F# & C#) implementation of this project is available here: [https://github.com/jeremybytes/digit-display](https://github.com/jeremybytes/digit-display). The C# version varies from the original implementation for consistency purposes.

These implementations were primarily for language practice and do not pretend to be fully optimized.

**Functions**  
* Reading files from the file system
* Training simple nearest-neighbor digit recognizers
    * Manhattan distance
    * Euclidean distance
* Output (pretty bad) ASCII art
* Multi-threading
* Channels
* Chunking / threading
* Parsing command-line parameters

**Usage**

*csharp*
```
PS C:\...> .\digits.exe --help
digits 1.0.0
Copyright (C) 2022 digits

  -o, --offset     (Default: 1000) Offset in the data set (default: 1000)
  -c, --count      (Default: 100) Number of records to process (default: 100)
  --classifier     (Default: euclidean) Classifier to use (default: 'euclidean')
  -t, --threads    (Default: 6) Number of threads to use (default: 6)
  --help           Display this help screen.
  --version        Display version information.
```

*golang*
```
PS C:\...> .\digit-display-golang.exe --help
Usage of C:\...\digit-display-golang.exe:
  -class string
        classifier calculation type - currently supported: 'manhattan', 'euclidean' (default "euclidean")
  -count int
        number of records to identify (default 100)
  -offset int
        starting record in data set (default 1000)
  -threads int
        number of threads to use (default 6)
```

*rust-lang*
```
PS C:\...> .\digits.exe --help
digits 1.0
Jeremy Clark
parses hand-written digits

USAGE:
    digits.exe [OPTIONS]

FLAGS:
    -h, --help       Prints help information
    -V, --version    Prints version information

OPTIONS:
        --classifier <classifier>    Classifier to use (default: 'euclidean')
    -c, --count <count>              Number of records to process (default: 100)
    -o, --offset <offset>            Offset in the data set (default: 1000)
    -t, --threads <threads>          Number of threads to use (default: 6)
```

**Release Builds**  
The speed comparison shown below is based on release builds of the projects. Release builds used the following command-line options:

*csharp*  
```
dotnet build -c release
```

*golang*
```
go build -ldflags "-s -w"
```

*rust-lang*
```
cargo build --release
```

**Speed Comparison**  

> Disclaimer:  
> The code in each environment is in no way  optimized. The main experiment was to use similar constructs where possible (eg. using Channels to communicated between parallel/async operations and to control the number of threads used by the processes). I would expect that manual threading or other language features could be used to speed up operations. 


The following shows a speed comparison between the various environments. These use the following parameters:
* Classifier: euclidean
* Offset: 1000
* Count: 2000
* Threads: 13

*Note about threads: I'm using a machine with 16 virtual cores. I generally run these applications at 15 threads to leave room for the console output to process. In this case, I went back to 13 to give a bit more space for the screen-capture software.*

All outputs show the same number of errors (since they use the same algorithm and data set). 

*csharp*
```
Using Euclidean Classifier -- Offset: 1000   Count: 2000
Total time: 00:00:20.7013789
Total errors: 59
```

*golang*
```
Using Euclidean Classifier -- Offset: 1000   Count: 2000
Total time: 14.2979381s
Total errors: 59
```

*rust-lang*  
```
Using Euclidean Classifier -- Offset: 1000   Count: 2000
Total time (seconds): 6.999
Total errors: 59
```

**Screen Capture**  
The following runs were captured separately (so each would have the full use of the machine (or as much of the machine that was available apart from the OS and other bits)). The results were then combined into a single video.

![Screen capture comparison](/images/comparison.gif)

The numbers vary a bit from those above, but remain pretty consistent on my machine.

**Thoughts**  
As an experiment, this was quite interesting to me. Go and C# are similar in that they are garbage-collected languages (which adds some overhead), so it does not suprirse me that Rust is faster for these operations.  

Because of the memory model in Rust, that version was significantly more challenging to write. The C# code could not be ported directly; there are quite a few inefficiencies that I added to get the Rust code to work.

If you have ideas on "fixing" any of these implementations, I'm interested in seeing them; however, this set of code will most likely remain fairly static.