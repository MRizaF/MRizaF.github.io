Mohammad Riza Farhandhy - 1313617038
======
Essay 'Operating System' Assignment 2
------

In here, I will explain how I finish one `Exercism` problem that use `Rust` with medium difficulty.

I finish 5 medium problems :
* Clock
* ISBN Verifier
* Scrabble Score
* Luhn
* Diamond

And I choose to explain "Diamond" problem.

<p align="center">
<img src="https://github.com/MRizaF/OS_A2/blob/image/Diamond-Exercism.PNG" alt="Diamond Exercism"/>
</p>

Why I choose this problem?

* First, because **this problem has the fewest community solutions** from 5 problems that I finish. It has only **39 community solutions**, while the others have at least **200 community solutions**.

<p align="center">
<img src="https://github.com/MRizaF/OS_A2/blob/image/Diamond-Community-Solutions.PNG" alt="Diamond Community Solutions"/></br>
(17 April 2019)
</p>

* Second, I just like it and I hope no one from Tier 2 will choose this too.

Ok, lets finish this problem!

---
### My step to finish this problem

What I do first, is to learn anything that I can get from the instruction, example, and test file. The summary I got :
* Input [char] = a letter, Output [Vec< String >] = ('A' -> Input), form a diamond shape.
* First and last row contains one 'A', others contains two identical letters.
* The diamond is horizontally and vertically symmetric.
* Top half has the letters in ascending order, bottom half has the letters in descending orders.
* All rows have as many trailing spaces as leading spaces, the four corners (containing the spaces) are triangles.
* Example diamond for letter 'C' : ··A·· ·B·B· C···C ·B·B· ··A··

*First look in the lib file.
```rust
pub fn get_diamond(c: char) -> Vec<String> {
  unimplemented!();
}
```

Now lets start build some code. What I wanna do first is to generate alphabet, like the first summary.
* Input [char] = a letter, Output [Vec< String >] = ('A' -> Input), form a diamond shape.

*Because I still dont know a lot about Rust, I search it in google "how to generate alphabet in rust". And I found this https://stackoverflow.com/questions/45343345/is-there-a-simple-way-to-generate-the-lowercase-and-uppercase-english-alphabet-i.

```rust
let alphabet = (b'A'..=b'z')                               // Start as u8
        .filter_map(|c| {
            let c = c as char;                             // Convert to char
            if c.is_alphabetic() { Some(c) } else { None } // Filter only alphabetic chars
        })          
        .collect::<Vec<_>>();
```

That code will generate 'A' until 'z' to vec. We need to modify it a bit, so it can fit our problem.

```rust
let mut alphabet = (b'A'..=c as u8)
        .filter_map(|x| {
            let x = x as char;
            if x.is_alphabetic() {
                if x == 'A' { Some(format!("{1}{0}{1}", x, 1)) }
                else { Some(format!("{1}{0}{2}{0}{1}", x, 1, 2)) }
            } else { None }
            }
        ).collect::<Vec<_>>();
```

The new code will generate = [1A1,1B2B1,1C2C1,...until input c]. The number will be the trailing spaces later.

*Why not add the trailing spaces directly? because I dont know how to get the length of the vec while we create it, so I just use number 1 for outer trailing spaces and number 2 for inner trailing spaces.

Now we need to replace the number in the vec with trailing spaces. Lets add some code to modify the vec.

```rust
let mut alphabet = (b'A'..=c as u8)
  .filter_map(|x| {
    let x = x as char;
    if x.is_alphabetic() {
      if x == 'A' { Some(format!("{1}{0}{1}", x, 1)) }
      else { Some(format!("{1}{0}{2}{0}{1}", x, 1, 2)) }
      } else { None }
    }
  ).collect::<Vec<_>>();

let l = alphabet.len();

for i in 0..l {
  alphabet[i] = alphabet[i].replace("1", &" ".repeat(l-(i+1)));
  if i != 0 {alphabet[i] = alphabet[i].replace("2", &" ".repeat(i+i-1));}
}
```

The new code will replace the number in the vec with trailing spaces, with conditions :
* Number 1 (Outer spaces), from the first row, the number of spaces will be the same as the vec.length-1 and keep reduce by 1 for the next row until the end of the vec.
* Number 2 (Inner spaces), after the first row, the number of spaces resemble odd number (1, 3, 5, ..).

Now we already have the top half diamond,
