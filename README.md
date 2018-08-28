# Exploring_CPP_11

This is my study notes of `Exploring C++ 11`.

# 01 Honing Your Tools

-  <http://cpphelp.com>
-  <http://www.apress.com/9781430261933>
-  <https://github.com/Apress/exploring-c-11>

Visual Studio includes a number of **doodads**, **froufrous**, and **whatnots** that are unimportant for your core task of learning C++.

-  `cl /EHsc /Za list0101.cpp`
-  `g++ -Wall -W -o list0101 -pedantic -std=c++11 list0101.cpp`
-  `clang++ -Wall -W -o list0101 -pedantic -std=c++11 list0101.cpp`

You may have to use these tools for this book, even if you must revert to some **crusty**, rusty, old tools for your job.

```C++
std::istream& in{std::cin};
std::string   line;
while (std::getline(in, line))
    std::cout << line << '\n';

std::perror("my error");

std::copy(vec.begin(), vec.end(),
          std::ostream_iterator<std::string>(std::cout, "\n"));
```

`list0102.cpp` is worth studying.

# 02 Reading C++ Code

Note that you cannot put a space between the slashes (`//`). That's true in general for all the multi-character symbols in C++. It's an important rule, and one you must **internalize** early. A **corollary** to the "no spaces in a symbol" rule is that when C++ sees adjacent characters, it usually constructs the longest possible symbol, even if you can see that doing so would produce meaningless results.

```C++
#include <limits>

std::numeric_limits<int>::max();
std::numeric_limits<bool>::digits();
```

# 03 Integer Expressions

```C++
int sum{0};
int count{}; // default initialization, false for bool, 0 for int
```

When a program reads from the null file, the input stream always sees an end-of-file condition.

```Shell
./list0301 < /dev/null
list0301 < NUL          # On Windows the null file is NUL
```

# 04 Strings

# 05 Simple Input

# 06 Error Messages

If you use command-line tools, expect to see **a slew of** errors running across your screen. If you use an IDE, it will help **corral** the error messages and associate each message with the relevant point in the source code that the compiler thinks is the cause of the error.

Modern compilers are pretty good at detecting problematic, but valid, code and issuing warnings, so get in the habit of **heeding** the warnings.

- `g++ -Wall`
- `clang++ -Wall`
- `cl.exe /Wall`

Disable individual warnings:
- `g++ -Wno-unused-local-typedefs`
- `cl.exe /wd4514`

# 07 For Loops

Sometimes you will see a for loop with a missing condition. That means the condition is always true, so the loop runs without stopping.

# 08 Formatted Output

To format the table of powers, you have to define a field for each column. A field has a width, a fill character, and an alignment.

## Field Width

```C++
#include <iomanip>

std::cout << std::setw(3) << i;
// sets the minimum width of an output field to at least three
// character positions
// if the number requires more space than that the actual output
// will take up as much space as needed.
```

The default field width is zero, which means everything you print takes up the exact amount of space it needs, no more, no less. After printing one item, the field width automatically resets to zero.

## Fill Character

Notice that unlike `setw`, `setfill` is sticky. That is, the output stream remembers the fill character and uses that character for all output fields until you set a different fill character.

## Alignment

To force the alignment to be left or right, use the `left` and `right` manipulators, which you get for free when you include `<iostream>`. (The only time you need `<iomanip>` is when you want to use manipulators that require additional information, such as `setw` and `setfill`.)

The default alignment is to the right.

The `left` and `right` manipulators are sticky.

The manipulators that take arguments, such as `setw` and `setfill`, are declared in `<iomanip>`. The manipulators without arguments, such as `left` and `right`, are declared in `<iostream>`.

If you include a header that you don’t really need, you might see a slightly slower compilation time, but no other ill effects will **befall** you.

The `left` and `boolalpha` manipulators are not declared in `<iostream>`. I lied to you. They are actually declared in `<ios>`. But `<iostream>` contains `#include <ios>`.

The input operator (`>>`) is actually declared in `<istream>`, and the output operator (`<<`) is declared in `<ostream>`. As with `<ios>`, the `<iostream>` header always includes `<istream>` and `<ostream>`.

```C++
std::cout.fill('0');
std::cout.width(6);
std::cout.setf(ios_base::left, ios_base::adjustfield);
```

When setting sticky properties, such as fill character or alignment, you might prefer using member functions instead of manipulators. You can also use member functions to query the current fill character, alignment and other flags, and field width—something you can’t do with manipulators.

To query the current fill character, call `cout.fill()`. That's the same function name you use to set the fill character, but when you call the function with no arguments, it returns the current fill character. Similarly, `cout. width()` returns the current field width. Obtaining the flags is slightly different. You call `setf` to set flags, such as the alignment, but you call `flags()` to return the current flags.

Printing an empty string with a nonzero field width is a quick and easy way to print a repetition of a single character.

```C++
std::cout << setfill('-') << width(10) << ""; // print 10 '-'
```

# 09 Arrays and Vectors

Those of you who suffered through a college algorithms course may remember how to write a bubble sort or quick sort, but why should you need to **muck about with** such low-level code?

# 10 Algorithms and Iterators

Most of the standard algorithms are declared in the `<algorithm>` header, although the `<numeric>` header contains a few that are numerically oriented.

The standard algorithms run the **gamut** of common programming activities: sorting, searching, copying, comparing, modifying, and more.

`assert` from `<cassert>` needs no `std::`.

*for-each* loop defines a hidden iterator that traverses all the elements. Each time through the loop, the iterator is dereferenced.

```C++
for (int element : data)
    std::cout << element << '\n';

#include <iterator>

std::istream_iterator<int> iit(std::cin);
std::istream_iterator<int> iit_end();
std::back_inserter(data);
// returns a back inserter iterator which call data.push_back any time assigned a value
std::ostream_iterator<int> oit(std::cout, "\n");
```

`list1003.cpp` is worth reading.
