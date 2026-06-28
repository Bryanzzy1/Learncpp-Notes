# Chapter 28 — Input and Output

Pin: No
Fav: No
Created: April 13, 2026 8:45 PM
Last edited: June 28, 2026 3:41 PM

## Notes

- **Input and output (I/O) streams**
    - a **stream** is just a sequence of bytes that can be accessed sequentially
        - a stream may produce or consume potentially unlimited amounts of data.
    - **Input streams** are used to hold input from a data producer, such as a keyboard, a file, or a network.
        - With input streams, the **extraction operator (>>)** is used to remove values from the stream.
    - **output streams** are used to hold output for a particular data consumer, such as a monitor, a file, or a printer.
        - With output streams, the **insertion operator (<<)** is used to put values in the stream.
    - `ios` is a typedef for `std::basic_ios<char>` that defines a bunch of stuff that is common to both input and output streams.
        - The **iostream** class can handle both input and output, allowing bidirectional I/O.
    - A **standard stream** is a pre-connected stream provided to a computer program by its environment. C++ comes with four predefined standard stream objects that have already been set up for your use.
        1. **cin** -- an istream object tied to the standard input (typically the keyboard)
        2. **cout** -- an ostream object tied to the standard output (typically the monitor)
        3. **cerr** -- an ostream object tied to the standard error (typically the monitor), providing unbuffered output
        4. **clog** -- an ostream object tied to the standard error (typically the monitor), providing buffered output
- **Input with istream**
    - A **manipulator** is an object that is used to modify a stream when applied with the extraction (>>) or insertion (<<) operators.
    - C++ provides a manipulator known as **setw** (in the iomanip header) that can be used to limit the number of characters read in from a stream.
    
    ```cpp
    #include <iomanip>
    char buf[10]{};
    std::cin >> std::setw(10) >> buf;
    ```
    
    - The extraction operator skips whitespace (blanks, tabs, and newlines).
    - the **get()** function, which simply gets a character from the input stream. Here’s the same program as above using get()
    - **getline()** that works similarly to get(), but will extract (and discard) the delimiter.
- **Output with ostream and ios**
    - There are two ways to change the formatting options: flags, and manipulators.
        - think of **flags** as boolean variables that can be turned on and off. **Manipulators** are objects placed in a stream that affect the way things are input and output.
    - To switch a flag on, use the **setf()** function, with the appropriate flag as a parameter.
        - To turn a flag off, use the **unsetf()** function
    - A **format group** is a group of flags that perform similar (sometimes mutually exclusive) formatting options.
- **Basic file I/O**
    - File outputs
    
    ```cpp
    int main()
    {
        // ofstream is used for writing files
        // We'll make a file called Sample.txt
        std::ofstream outf{ "Sample.txt" };
    
        // If we couldn't open the output file stream for writing
        if (!outf)
        {
            // Print an error and exit
            std::cerr << "Uh oh, Sample.txt could not be opened for writing!\n";
            return 1;
        }
    
        // We'll write two lines into this file
        outf << "This is line 1\n";
        outf << "This is line 2\n";
    
        return 0;
    
        // When outf goes out of scope, the ofstream
        // destructor will close the file
    }
    ```
    
    - **File input**
    
    ```cpp
    #include <fstream>
    #include <iostream>
    #include <string>
    
    int main()
    {
        // ifstream is used for reading files
        // We'll read from a file called Sample.txt
        std::ifstream inf{ "Sample.txt" };
    
        // If we couldn't open the output file stream for reading
        if (!inf)
        {
            // Print an error and exit
            std::cerr << "Uh oh, Sample.txt could not be opened for reading!\n";
            return 1;
        }
    
        // While there's still stuff left to read
        std::string strInput{};
        while (inf >> strInput)
            std::cout << strInput << '\n';
    
        return 0;
    
        // When inf goes out of scope, the ifstream
        // destructor will close the file
    }
    ```
    
    -