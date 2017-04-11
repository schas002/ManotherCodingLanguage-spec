Manother Coding Language
Working Draft 1, as of 2017-04-11
Specification

The following spec is licensed under version 4.0 of the Creative Commons Attribution license; see https://creativecommons.org/licenses/by/4.0/ for information on it. However, implementations of this spec can use another license.

* * *

A work in progress esoteric language that is concise.

@ Contents @
^F
(1)    Syntax
(1a)   Data types
(1a.1) Numbers
(1b)   Commands
(1c)   Sanitization
(1d)   Execution
(1d.1) NOP axiom
(2)    Instructions
(2a)   Outside-World Ops
(2b)   Tape Ops
(2c)   Stack Ops
(2d)   Flow Control

@ Instruction Tables @

# Single character commands #

'e' assigned
 e. unassigned

 q. 'w'  e.  r.  t.  y. 'u' 'i' 'o' 'p'
 a.  s. 'd'  f.  g.  h.  j.  k.  l.  z.
 x.  c.  v.  b.  n. 'm'
 Q.  W.  E.  R.  T.  Y.  U. 'I' 'O'  P.
 A.  S.  D.  F.  G.  H.  J.  K.  L.  Z.
 X.  C.  V.  B.  N.  M.
'0' '1' '2' '3' '4' '5' '6' '7' '8' '9'
'@'  #. '$' '%'  &. '*'  (.  ). '-'  \.
 !.  ;. ':'  '.  ". '?' '/' '^'  [.  ].
 {.  }.  <.  >. '+'  =. '_'  ~.  |.  `.

# List of extended commands #

'xh'  'x<'  'x>'

@ (1) Syntax @

# (1a) Data types #

The language has a meaning of data types. However, as of yet the only data type MCL operates on is numbers.

$ (1a.1) Numbers $

Numbers are signed integers which can go at least up to 255 (2^8 - 1).

They are denoted as 'a' in instruction listings, though that can also denote any type at all.

# (1b) Commands #

A command is a character that is not whitespace nor 'x' or an extended command.

An extended command is a combination of characters that is one or more 'x' followed by the same amount of characters, with the added rules that the first character is not 'x', and none of these characters are whitespace.

# (1c) Sanitization #

Before a MCL program is executed, it is first sanitized from non-code by removing these things in order.

1> Block comments
   Any construct that starts with 'x[' and ends with 'x]' is removed. (Blocks that encounter the start or end of the file don’t require a matching 'x[' or 'x]'.)
2> Inline comments
   Any construct that starts with 'x\' and goes up until the end of line is removed.
3> Whitespace
   Any space, tab or newline is removed.

# (1d) Execution #

Commands are executed left-to-right, excluding changes in the flow made by individual commands, and operate on an infinite tape of stacks.

The current stack is the stack the tape pointer points to. Pushes and pops are performed on it.

$ (1d.1) NOP axiom $

If a command could not be executed under current conditions (invalid command, not enough items to pop, or some items to pop don't satisfy requirements), it is not executed, essentially becoming a NOP.

@ (2) Instructions @

# (2a) Outside-World Ops #

Instructions that operate with the outside world.

! 'i' (input integer)
Push an integer from standard input.

! 'I' (input character)
Get a character from standard input. Push that character's ordinal.

! 'o' (output integer)
Pop a. Output a to standard output.

! 'O' (output character)
Pop a. Output the character corresponding to a to standard output.

# (2b) Tape Ops #

Instructions that operate with the tape.

! 'x>' (extended 1) (tape forward)
Move the tape pointer to the next stack.

! 'x<' (extended 1) (tape backward)
Move the tape pointer to the previous stack.

# (2c) Stack Ops #

The majority of instructions operate with the current stack.

$ (2c.1) Push/Pop $

! '0' '1' '2' '3' '4' '5' '6' '7' '8' '9' (push digit)
Push the digit signified by the instruction.

! '_' (pop)
Pop a.

$ (2c.2) Increment/Decrement $

! 'u' (increment)
Pop a. Push a + 1.

! 'd' (decrement)
Pop a. Push a - 1.

$ (2c.3) Arithmetic $

! '+' (add)
Pop a, b. Push a + b.

! '-' (subtract)
Pop a, b. Push a - b.

! '*' (multiply)
Pop a, b. Push a * b.

! '/' (divide)
Pop a, b. Push a / b, rounded down.

! 'm' (modulo)
Pop a, b. Push a mod b.

! 'p' (power)
Pop a, b. Push a to the power b.

$ (2c.4) Primitives $

! '$' (dup) [x a | x a a]
Pop a. Push a twice.

! '%' (swap) [x a b | x b a]
Pop a, b. Push b, a.

! '@' (roll) [a b c d | d a b c]
Put the top of stack to the bottom of stack.

! '^' (pick) [x a b | x a b a]
Pop a, b. Push a, b, a.

# (2d) Flow Control #

Instructions that control flow of the program.

! '?' (if)
Is a nested structure that runs only if the top of stack is nonzero. The top of stack is only peeked at.

! 'w' (while)
Is a nested structure that loops while the top of stack is nonzero. The top of stack is only peeked at.

! ':' (end)
Closes a nested structure.

! 'xh' (extended 1) (halt)
Halts execution of the program.
