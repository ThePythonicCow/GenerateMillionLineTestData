# GenerateMillionLineTestData
Efficiently generate one million short random ASCII lines of text data

This program generates random short lines of ASCII text.
Lines consist of zero to fifteen ASCII characters from
a subset (the first 32 characters) of the RFC 4648 base64
set [A-Za-z0-9+/], each line (except perhaps the last)
terminated by a newline ('\n').

## Motivation:
The primary motivation for this command is to generate input
test for a high performance "getline()" variant that I am
currently working on, which will be called "rawscan_getline()",
to be made available in both C (for now anyway just for gcc on
Linux, where I have decades of experience) and Rust (as a Rust
learning exercise for myself.)

## Underlying technology:
This program is currently of use with the GNU C compiler on Linux
systems, though it should be easily portable to other systems.
It makes good use of Melissa E. O’Neill's delightful "PCG" pseudo
random number generator: http://www.pcg-random.org/.

## How to build and use:
The working source consists of a single C language source file.

### Download and build
Download it, and compile it.

Minimal simple compile command:

  ```gcc -o ran_len_line_gen ran_len_line_gen.c```
  
Fancy optimized compile command:

```gcc -fwhole-program -march=native -O3 -Wall -pedantic -Werror -o ran_len_line_gen ran_len_line_gen.c```
 
### Running the command
Then you can use this program to feed a million lines of test
data into some other program. For example, to test how a command
"foo" handles this particular input, at a shell prompt, pipe
the output of the compiled ran_len_line_gen into the input of foo:

  ```./ran_len_line_gen | foo```

### Command line option
Currently this command has one command line option, ```-n number```,
to change the number of lines output, from the default one million,
to whatever positive number of lines you need.

### Example output

Here's an example of using this command line option to obtain
just ten lines of output, including showing exactly what those
ten lines will be.

Running the command:

  ```./ran_len_line_gen -n 10```
  
at a shell prompt will produce the output:

  ```
AB
CDEFGHIJK
LMNOPQRSTUV
WXYZabcdefAB
CDEF
GHIJKLMNOPQRS
TUVW
XYZab
cdefABCDE
FGHIJKLMNOPQR
```

## How to test:
The quite predictable default million line output has a sha256 sum of:
8f560d2518f0df1b0f309b7c34395d21ffd50648726e26ea37ea019b79acf6e9

To test this, build as above and run the following command at a shell
prompt (assuming you have the command sha256sum installed):

  ```./ran_len_line_gen | sha256sum```

This command will output the string:

  ```8f560d2518f0df1b0f309b7c34395d21ffd50648726e26ea37ea019b79acf6e9```
