# Code_Testing

### Case Down
Test Cases: [Cases](/Projects/cs236TestCases.zip)

### Example Test Cases

We provide example input and output files. These will help you test your code and answer any questions you have about input and output that are not answered in the specification. 

If you are ever uncertain about how a particular edge case should be handled, you can look for example input and output that addresses it. 

Additionally, these test cases generally serve as a good indicator for whether or not your code will pass the pass-off tests. However, these test cases are not comprehensive! If all of these files pass, it does not guarantee that your program will perfectly pass the pass-off driver. 

**Also write your own test cases**; see if you can put something in that gets an incorrect output.

### Pass off cases

The pass-off driver that is used to pass-off your labs uses the provided cases and this command in a Linux terminal.

Compile (replace 'nameOfExecutable' with the name you want for your executable):

    g++ -Wall -Werror -std=c++17 -g *.cpp -o nameOfExecutable

Run (replace 'input.txt' with the name of the input file you are testing):

    ./nameOfExecutable input.txt

After you run it, compare your output to the answer.txt of the corresponding input.


### Makefile Help

We provide a makefile with each folder of cases. 
  
To use the file, place it in the same directory as your code. Also place the unzipped folder of example test files in the same directory as your code.  

In the terminal/command-line, you can use the command "make compile" to compile your code.  

In the terminal/command-line, you can use the command "make run" to run your program on all of the example input/output files, with diff output.  

In the terminal/command-line, you can use the command "make zip" to compress up all the .h and .cpp files into a .zip folder

To install make and for more information on make and makefiles, click [here](https://www.gnu.org/software/make/) (or [here](http://gnuwin32.sourceforge.net/packages/make.htm) for Windows).