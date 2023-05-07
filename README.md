Download Link: https://assignmentchef.com/product/solved-engg1003-assignment-1-english-text-ciphers
<br>



<h1>1        Introduction</h1>

A <em>cipher </em>is an algorithm which encrypts a message so that it can be safely transmitted without an eavesdropper being able to (easily) read it. For the purposes of this assignment the “message” will be a block of English text stored as a string of ASCII characters.

Cipher algorithms always perform two tasks: encryption and decryption. The encryption process takes a “message” and “key” as inputs and produces <em>cipher text</em>. The decryption process performs the reverse, it turns cipher text and the key back into the original message.

The exact details of how the key and message are processed to produce the cipher text vary between algorithms. However, the general principle is that algorithms employ a mathematical function which is “easy” to calculate with the key but “difficult” to invert without it. A rigorous discussion of modern cryptographic techniques can be found in the excellent (but <em>highly </em>mathematical) text, An Introduction to Mathematical Cryptography (available in the <a href="http://library.newcastle.edu.au/record=b3576154~S16">Auchmuty library</a><a href="http://library.newcastle.edu.au/record=b3576154~S16">)</a>.

This assignment will cover two ancient<a href="#_ftn1" name="_ftnref1"><sup>[1]</sup></a> cipher algorithms:

<ol>

 <li>The rotation cipher (also known as <em>Caesar cipher</em>)</li>

 <li>The substitution cipher.</li>

</ol>

Your task is to write a C program which performs the following broad tasks:

<ul>

 <li>Encryption given an algorithm, message text, and key</li>

 <li>Decryption given an algorithm, cipher text, and key</li>

 <li>Decryption given cipher text and some assumptions of its contents <em>without </em>the key</li>

</ul>

<h1>2        Cipher Algorithms</h1>

<h2>2.1       Rotation Cipher</h2>

A rotation cipher encrypts a text message by substituting each letter in the message with a letter a fixed number of places away in the alphabet. The “key” is the number of letters by which the alphabet is shifted when calculating a substitution. When performing the shift any letter that “falls off the end” is rotated back to the start of the alphabet.

Example with a shift of 1 to the right:

Message letter:                   ABCDEFGHIJKLMNOPQRSTUVWXYZ

Cipher text letter: ZABCDEFGHIJKLMNOPQRSTUVWXY

A message is encrypted by substituting letters in the top row with ones from the bottom. For example:

Message:                       ATTACK AT SUNRISE

Cipher text : ZSSZBJ ZS RTMQHRD

This cipher can be described mathematically using the modulus operator. The “key” is an integer, <em>k</em>, between -25 and 25 (0 and 26 imply ”no encryption”) and each letter is allocated a number:

<em>a </em>= 0<em>,b </em>= 1<em>,c </em>= 2<em>,…,z </em>= 25<em>.</em>

Encryption can then be performed on a message letter, <em>m</em>, by defining the encryption function, <em>e</em>(<em>m</em>), as:

<em>e</em>(<em>x</em>) = (<em>m </em>+ <em>k</em>)(mod 26)

where “mod” means the modulus operator (% symbol in C).

The decryption of a cipher text letter, <em>c</em>, can be defined by a decryption function, <em>d</em>(<em>c</em>), as follows:

<em>d</em>(<em>c</em>) = (<em>c </em>− <em>k</em>)(mod 26)

<strong>NB: </strong>In C, the modulus operator has undefined behaviour when dealing with negative numbers. If a negative occurs in the calculation of (<em>m </em>+ <em>k</em>) or (<em>c </em>− <em>k</em>) then you can add 26 to make the result positive without impacting the result.

<h3>2.1.1      Rotation Cipher Attacks</h3>

Because the encryption key is an alphabetical rotation there are only 25 different substitutions to choose from (the 26th being “no rotation”). Furthermore, because the encrypted alphabet is still in alphabetical order a full decryption is possible if any one letter substitution is discovered.

Decryption <em>without </em>the key can therefore be done with two methods:

<ul>

 <li>A <em>brute force </em>attack, where every possible key is tested and all the algorithm outputs tested for intelligibility (eg: How many spelling mistakes are there?) or a search for some known phrase (eg: a recipient’s name)</li>

 <li>A statistical attack, where each different letter in the cipher text is counted, the most frequent letter assumed to be ’e’ (or ’t’, then ’a’), and the rotation deduced and tested from that.</li>

</ul>

To perform either of the above attacks you only need knowledge of the cipher algorithm and the language of the original message. Even if done by hand, brute forcing 25 different encryption keys is quite straightforward.

The security of the rotation cipher mostly comes from the algorithm being kept secret. In the modern world “security by obscurity” is not considered to be of much use so this cipher is studied purely out of historic curiosity.

<h2>2.2       Substitution Cipher</h2>

A substitution cipher encrypts a text message by replacing each of the 26 message letters to an encrypted letter. Each letter is only chosen once. The “key” is therefore knowledge of all 26 different substitutions. The number of possible key combinations is: 26! ≈ 4 × 10<sup>26</sup>.

Example with a substitution chosen from the qwerty keyboard layout:

Message letter:                   ABCDEFGHIJKLMNOPQRSTUVWXYZ

Cipher text letter: QWERTYUIOPASDFGHJKLZXCVBNM

As before, a message is encrypted by substituting each letter in the top row with the one below it:

Message:                           PLEASE GET MILK AT THE SHOPS

Cipher text : HSTQLT UTZ DOSA QZ ZIT LIGHL

There is no neat algebraic method to write the encryption function. It is effectively a “look-up-table” where each letter, <em>x<sub>n</sub></em>, becomes a different letter, <em>y<sub>n </sub></em>based on a fixed substitution rule.

<h2>2.3       Substitution Cipher Attacks</h2>

With 26! different encryption keys a brute force attack is not possible. Even when testing a million combinations <em>per second </em>it would take almost 1000 times longer than the <em>age of the universe </em>to test every encryption key. Aside: the German Enigma machine (used in WWII) was a form of substitution cipher and a new encryption key was used every day. Even with modern computers a naive brute force attack would not have been possible. You can read about the Enigma decryption efforts on <a href="https://en.wikipedia.org/wiki/Cryptanalysis_of_the_Enigma">Wikipedia</a><a href="https://en.wikipedia.org/wiki/Cryptanalysis_of_the_Enigma">.</a>

Decryption must therefore rely on extra information which reduces the key search space. It is possible, for example, to perform a statistical analysis of the cipher text to estimate which letters were used for ’e’, ’t’,

’a’, ’z’, etc.

If the message is assumed to be normal English text then other assumptions can be made. Any single letter word is likely to be ’a’ or ’i’, the latter being easier to spot if the message is case sensitive. Likewise, the most common three letter word is likely to be ’the’.

By making educated guesses about the most common short words and letters a subset of the encryption key can be deduced. This knowledge, coupled with a dictionary, and some kind of “spell checker” algorithm such as <em>Levenshtein distance</em>, can then be used in an attempt to work out further letter substitutions.

The WWII efforts to decrypt Enigma relied heavily on <em>cribs</em>. These were known, or assumed, sections of plain text for a given encrypted message. For example, many German messages would include regular weather reports in the same format and would reveal several of the day’s letter substitutions.

<h1>3        Programming Task</h1>

<em>Please read the full marking guide before reacting emotionally to the task difficulty. A pass will be “reasonably easy” but a high distinction will be “unreasonably difficult”.</em>

Write a C program which performs the following tasks:

<ol>

 <li>Encryption of a message with a rotation cipher given the message text and rotation amount</li>

 <li>Decryption of a message encrypted with a rotation cipher given cipher text and rotation amount</li>

 <li>Encryption of a message with a substitution cipher given message text and alphabet substitution</li>

 <li>Decryption of a message encrypted with a substitution cipher given cipher text and substitutions</li>

 <li>Decryption of a message encrypted with a rotation cipher given cipher text only</li>

 <li>Decryption of a message encrypted with a substitution cipher given cipher text only</li>

</ol>

<h2>3.1       User Interface Specification</h2>

<h3>3.1.1      Inputs</h3>

For all data inputs (message, keys, cipher text, algorithm selection, etc) you may choose from the following methods (<strong>NB: </strong>methods are <em>not </em>worth equal marks):

<ul>

 <li>Hard-coded variable initialisation</li>

 <li>Read from stdin with scanf()</li>

 <li>Read from a file using the C standard file I/O library</li>

</ul>

It is <em>strongly </em>recommended that variable initialisation be used while debugging your programs and file I/O only be implemented as a final feature.

If file I/O is implemented the file name may be hard coded. You are permitted to read the file name(s) as command line arguments but, at time of writing, this is beyond the scope of ENGG1003 and will not attract extra marks.

When being graded, you should ensure that multiple inputs can be tested quickly. This is especially true for the decryption tasks which will be tested with multiple blocks of cipher text.

<h3>3.1.2      Outputs</h3>

All program output should be sent to stdout. You are encouraged to also use file I/O for outputs but, due to time constraints while marking, all output should be sent to <em>both </em>the file and stdout.

<h3>3.1.3      Task Selection</h3>

It should be easy for a <em>demonstrator </em>to quickly test functionality of each task listed in Section 3. It is up to you to decide exactly how this is implemented but the following options are recommended (NB: not all options are worth equal marks):

<ul>

 <li>Hard code an initialised integer variable which selects between the different tasks.</li>

 <li>Take user input from stdin to select each task in a menu system</li>

 <li>(Advanced) Define the task as part of a <em>header </em>inside an input file which contains the message and key (or key and cipher text, or just cipher text). The header could be something like:

  <ul>

   <li>A single integer placed on the first line to indicate the task to be performed, followed by</li>

   <li>A 2nd, optional, line which contains a key (perhaps start the line with a “weird” character like # to indicate that it is a key), followed by</li>

   <li>The message or cipher text</li>

  </ul></li>

 <li>(Advanced-Beyond ENGG1003) Use command line arguments</li>

</ul>

<strong>Each task should be implemented as a different function</strong>. Beyond that you are free to choose how many functions are implemented.

All tasks must be accessible from running a <strong>single </strong>.c file. The GitHub repository should contain this .c file and any other required files (eg: input text).

<h2>3.2       Message Text Specification</h2>

The ciphers above are only defined for letters. This leaves a few potential ambiguities:

<ul>

 <li>What should be done to punctuation and white space?</li>

 <li>What about numerals?</li>

 <li>Should upper and lower case letters be handled differently?</li>

</ul>

For the purposes of this assignment you should apply the following rules:

<ol>

 <li>Do not encrypt white space, punctuation, or numerals. If an input character is not a letter it should be copied to the output unmodified.</li>

 <li>All input data should use UPPER CASE letters only</li>

</ol>

<ul>

 <li>(Advanced) If a lower case letter is found in the input it should be converted to upper case before encryption</li>

</ul>

<h2>3.3       Key Format Specification</h2>

The rotation cipher key is to be a single integer in the range [0,25]. “Encryption” with a shift of zero should process the plain text but produce cipher text equal to plain text.

The substitution cipher key is a string of 26 UPPER CASE letters ordered as-per their alphabetical substitution.

eg: The string “QAZXSWEDCVFRTGBNHYUJMKILOP” would cause ’A’ to become ’Q’, ’B’ to become ’A’, …, ’Z’ to become ’P’. Substitution of a letter with itself (ie: no change for one or more letters) should be allowed.

<h3>3.3.1      ASCII Code</h3>

All input data is to be encoded with the ASCII standard. ASCII encoding defines that upper and lower case letters be stored as the following 8-bit integers:

<table width="104">

 <tbody>

  <tr>

   <td width="27">ABC…YZ</td>

   <td width="30">6566678990</td>

   <td width="27">a b c…y z</td>

   <td width="20">9798100121122</td>

  </tr>

 </tbody>

</table>

If an input byte is outside of the ranges [65<em>,</em>90] and [97<em>,</em>122] then it can be copied to the output without modification. If an input byte is in the lower case range, [97<em>,</em>122], then you should subtract 32 from its value to make it an upper case letter prior to encryption.

<h1>4        Tackling the Problem</h1>

To complete all parts of this assignment some project management techniques are required.

This is a big task. It is intentionally large because I encourage you to discuss potential algorithms with your peers while writing your program. Implementation in C, however, should all be your own work.

It is also expected that some level of independent research will be needed before you can fully implement all the specifications.

Each programming task listed in Section 3 gets progressively more difficult. I am not expecting anybody to 100% “finish” this task but have designed it so that achieving a pass or credit is a “reasonable” amount of work for an average student. By contrast, achieving a high distinction should be an “extensive” amount of work for the high achievers.

You will need to do some planning before doing too much coding. This will involve understanding the problem at a high level (eg: message and key in, encrypted text out) then breaking it down into smaller sub-tasks (eg: read message in, read key, loop over message doing substitutions, store message somewhere).

Each time you define a sub-task try to describe it as a C function. Can you clearly define the inputs, the processing, and the outputs? If so, it is a good candidate for an <em>actual </em>function in your code.

After sub-tasks have been defined the details can be filled in. What variables do you need? Should you use 26 char’s to represent 26 letters or should they be in an array?


