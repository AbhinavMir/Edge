notes from [this article](https://blog.usejournal.com/writing-your-own-programming-language-and-compiler-with-python-a468970ae6df).

While this is starting as a project to better understand programs, compilers and stuff, I am hoping that over the course of next few months, I can develop something worthwhile. I want to mostly develop a Python-like language with Static data types and curly brackets. Native modules for stuff like I/O and easy package management with the robustness of C++ is what I'm going for, with stuff like collections from Java and lack of dependency hell from Go. I enjoy all these languages, and I hope I can make something. But if my lockdown attempts at cooking have taught me something, it is this: the final result doesn't always turn out well in the first try.

## Setup
Download Conda, I personally use Miniconda. 
`conda install --channel=numba llvmlite`
`conda install -c conda-forge rply`

## Extended BNF

EBNF is a metasyntax for CFGs. For `4+2;` , we define the EBNF as 
```
expression = number, "+", number, ";";
number = digit+;
digit = [0-9];
```

## Compiler

Edge gets converted to LLVM Intermediate Representation
![](./img/compilers.png)
>Using LLVM, it is possible to optimize your compilation without learning compiling optimization, and LLVM has a really good library to work with compilers.

3 components of the compiler are: Lexer, Parser, Code Generator. Let's talk about them.

![](https://mk0tuzolorusfnc7thxk.kinstacdn.com/wp-content/uploads/2017/02/lexer-parser-center-1030x187.png)

### Lexer

We will use RPLY's Lexer Generator and describe a Class to add tokens. 

```python
   def _add_tokens(self):
        # Print
        self.lexer.add('PRINT', r'print')
        # Parenthesis
        self.lexer.add('OPEN_PAREN', r'\(')
        self.lexer.add('CLOSE_PAREN', r'\)')
        # Semi Colon
        self.lexer.add('SEMI_COLON', r'\;')
        # Operators
        self.lexer.add('SUM', r'\+')
        self.lexer.add('SUB', r'\-')
        # Number
        self.lexer.add('NUMBER', r'\d+')
        # Ignore spaces
        self.lexer.ignore('\s+')
```

We added Lexical Generator for `()`, `+`, `-` and stuff. Then we call this in `main.py`, and then for a text input we process it lexically.
