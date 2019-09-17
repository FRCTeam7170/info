# Java Code Conventions

This document outlines the code conventions for new projects written in Java by FRC Team 7170.

Most of these rules are not hard and fast; they serve merely as guidelines. There are exceptions to most rules,
especially those subject to any oversights in the creation of this document.

This document is not comprehensive and mainly touches on more controversial issues.
All standard Java-isms apply (e.g. using the most restrictive access modifier possible, usually using setters and
getters, etc.).
Generally speaking, any formatting automatically done by a modern, advanced IDE (e.g. Intellij) is acceptable if not
advisable.

## Package Naming and Project Structure

 - All package names should start with `frc.team7170`.
 - Year/season-specific robot code should be in the package `frc.team7170.robotYEAR`, where "YEAR" is replaced by the
   four digit representation of the year (e.g. "2020").
 - Non-year/season-specific code should go in an appropriately named subpackage of `frc.team7170`, like
   `frc.team7170.lib` for spooky-lib, for example.
 - The `Robot` and `Main` classes (if `Main` is being used) should go in the `frc.team7170.robotYEAR` package; all other
   structural decisions are year-specific.
 - External libraries should be configured using what ever system is in place by FIRST in the current year. Any
   libraries that do not fit into the library system should be place in a top-level folder named `lib` in the form of a
   JAR and setup as a dependency in Gradle.
 - The `.git` folder, `.gitignore` file, gradle-related files/folders, and the `src` folder should all be top-level.
 - The `src` tree should look as follows:
   
        src
        ├──main
        │  └──java
        │     └──<packages>
        └──test
           └──java
              └──<packages>
              
## Source File Structure

 - Every element of a source file should be ordered logically and consistently. One effective order might be as follows:
   - package name
   - newline
   - imports (perhaps with an ordering scheme of their own; IDEs often do this for you)
   - newline
   - class/interface/enum/annotation declaration
     - constants
     - class/static variables
     - instance variables
     - constructors
     - methods
 - Source code elements should be grouped together logically.
   - Overloaded methods should be grouped together.
   - Associated methods should be near each other.
   - Etc.
 
## Naming

 - Local, instance, and class/static variables should be `camelCase` with no prefixes like `m_`.
 - Constants should be in all caps with underscores: `THIS_IS_A_CONSTANT`. Note that a variable that is static and final
   is not necessarily a constant conceptually, and thus this naming scheme does not apply. Strictly speaking, it is
   necessary but not sufficient for a "variable" to be static and final in order for it to be a constant conceptually.
   
## Formatting

 - Pick a number of characters to wrap each line to and stick to it. A good number is around 120.
   Exactly how to wrap each line is subjective, but you should pick some scheme that makes sense to you and be
   consistent. With that said, it is generally obvious what is good and was is bad:
        
        // Bad:
        myObj.
        reaaaaaaalllllllllllllllllllllllyyLooooooooooooooooooongggggggggMeeeeeeeetttthhhhhhhoodCallllll(args);
            
        // Good:
        myObj.reaaaaaaalllllllllllllllllllllllyyLooooooooooooooooooongggggggggMeeeeeeeetttthhhhhhhoodCallllll(
                args
        );

 - Indent the code in each block by four spaces greater than the outer block. (By block we mean the body of if
   statements, for and while loops, and methods; everything inside a class; each case in a switch statement, etc.)
 - Always use braces, even when they are optional (e.g. always use braces for if statements, even if the body contains
   only one statement).
 - Use the so-called K&R style for braces. For example:
 
        // Good:
        if (condition) {
            // body
        } else {
            // body
        }
        
        // Bad:
        if (condition)
        {
            // body
        }
        else
        {
            // body
        }

 - A single space should appear around the parenthesized expression in if statements (see above), for loops,
   while loops, switch statements, etc.
 - The parameter list for a method or constructor should be immediately preceded by the method or constructor name and
   immediately followed by a space and the opening curly brace. For example:
        
        // Good:
        public void myMethod(int arg, int otherArg) {...}
        
        // Bad:
        public void myMethod (int arg, int otherArg){...}

 - Blank lines should be used to logically group code. Superfluous blanks lines are discouraged.
 - Use the modifier order recommended in the JLS:
   `public protected private abstract default static final transient volatile synchronized native strictfp`.
 - Arithmetic and logic expressions should use grouping parentheses whenever there is a chance the code could be
   misinterpreted, even if they are not strictly necessary (one should not assume the reader has memorized all the Java
   operator precedences).
 - Comment/javadoc formatting (note the asterisk alignment and whitespace):
   
        // This is a single line comment.
        
        int x; // This is a single line comment after a statement.
        
        /*
         * This is a
         * multiline
         * comment.
         */
        
        // This is also
        // fine for multiline.
        
        /**
         * This is javadoc.
         * Use HTML and javadoc tags as needed.
         *
         * Use blank lines for grouping.
         */

## Commenting

 - Would anybody ever verbally say something like "I am about to say hello... Hello!"? So why, then, do comments such as
   the following exist? (Do not do this!)
   
        // Increment the counter.
        x++;
        
 - *Talk is cheap. Show me the code.* - Linus Torvalds
 - Comments are to explain the *why*, not the *what*. (Sometimes a line can be complicated enough to warrant commenting
   about the *what*, but this is rare.)
 
## Documentation

 - Formal javadoc documentation for year-specific robot code is not required (and perhaps not advisable given the time
   constraint), but it may be helpful.
 - Reusable bits of code should be properly documented (subject to standard javadoc conventions).
 - Documentation that is not updated as the code is updated is ***worse*** than having no documentation.
 - Documentation gives a high-level specification of what a method/class does and how to use it, not the implementation
   details only of concern to the developer of the method/class.
