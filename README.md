Essay 'Operating System' Assignment 2
======

Mohammad Riza Farhandhy - 1313617038
------

Hello World! 😀

In here, I will explain how I finish one `Exercism` problem that use `Rust` with medium difficulty.

I had finished 5 medium problems :
* [Clock](https://exercism.io/tracks/rust/exercises/clock/solutions/20d52310c6a44f929759c5846278f18e)
* [ISBN Verifier](https://exercism.io/tracks/rust/exercises/isbn-verifier/solutions/6cd2dd2ec553420ea64ee1bcb09ee10a)
* [Roman Numerals](https://exercism.io/tracks/rust/exercises/roman-numerals/solutions/c7b74b766d7044abb0614683b9c5637f)
* [Luhn](https://exercism.io/tracks/rust/exercises/luhn/solutions/fcf3996676c14b6ea966cf4e211eaf98)
* [Diamond](https://exercism.io/tracks/rust/exercises/diamond/solutions/7d8c3e699a8d47edb12b18dd0ca3e4a9)

And I choose to explain "Diamond" problem.

<p align="center">
<img src="https://github.com/MRizaF/MRizaF.github.io/blob/images/Diamond-Exercism.PNG" alt="Diamond Exercism"/>
</p>

Why I choose this problem?

* First, because **this problem has the fewest community solutions** from 5 problems that I finish. It has only **39 community solutions**, while the others have at least **200 community solutions**.

<p align="center">
<img src="https://github.com/MRizaF/MRizaF.github.io/blob/images/Diamond-Community-Solutions.png" alt="Diamond Community Solutions (17 April 2019)"/>
</p>

* Second, I just like it and I hope no one from Tier 2 will choose this too.

Ok, lets solve this problem!

---
### My step to solve this problem

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

Now lets start build some code. What I wanna do first is to generate alphabet.

*Because I still dont know a lot about Rust, I search it in google "how to generate alphabet in rust". And I found this <https://stackoverflow.com/questions/45343345/is-there-a-simple-way-to-generate-the-lowercase-and-uppercase-english-alphabet-i.>

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
pub fn get_diamond(c: char) -> Vec<String> {
	let mut alphabet = (b'A'..=c as u8)
		.filter_map(|x| {
			let x = x as char;
			if x.is_alphabetic() {
				if x == 'A' { Some(format!("{1}{0}{1}", x, 1)) }
				else { Some(format!("{1}{0}{2}{0}{1}", x, 1, 2)) }
			} else { None }
		})
		.collect::<Vec<_>>();
}
```

The new code will generate = [1A1, 1B2B1, 1C2C1, ...until input c]. The number will be the trailing spaces later.

*Why not add the trailing spaces directly? well, because I dont know how to get the length of the vec while we create it, so I just use number 1 for outer trailing spaces and number 2 for inner trailing spaces.

Now we need to replace the number in the vec with trailing spaces. Lets add some code to modify the vec.

```rust
pub fn get_diamond(c: char) -> Vec<String> {
	let mut alphabet = (b'A'..=c as u8)
		.filter_map(|x| {
			let x = x as char;
			if x.is_alphabetic() {
				if x == 'A' { Some(format!("{1}{0}{1}", x, 1)) }
				else { Some(format!("{1}{0}{2}{0}{1}", x, 1, 2)) }
			} else { None }
		})
		.collect::<Vec<_>>();

	let l = alphabet.len();

	for i in 0..l {
		alphabet[i] = alphabet[i].replace("1", &" ".repeat(l-(i+1)));
		if i != 0 {alphabet[i] = alphabet[i].replace("2", &" ".repeat(i+i-1));}
	}
}
```

The new code will replace the number in the vec with trailing spaces, with conditions :
* Number 1 (Outer spaces), from the first row, the number of spaces will be the same as the vec.length-1 and keep reduce by 1 for the next row until the end of the vec.
* Number 2 (Inner spaces), after the first row, the number of spaces resemble odd number (1, 3, 5, ..).

Now we already have the top half diamond, how about the bottom half?

Well, the bottom half is basically the same as the top half just in different order. Lets add the last code to finish the problem.

*Finished Code
```rust
pub fn get_diamond(c: char) -> Vec<String> {
	let mut alphabet = (b'A'..=c as u8)
		.filter_map(|x| {
			let x = x as char;
			if x.is_alphabetic() {
				if x == 'A' { Some(format!("{1}{0}{1}", x, 1)) }
				else { Some(format!("{1}{0}{2}{0}{1}", x, 1, 2)) }
			} else { None }
		})
		.collect::<Vec<_>>();

	let l = alphabet.len();

	for i in 0..l {
		alphabet[i] = alphabet[i].replace("1", &" ".repeat(l-(i+1)));
		if i != 0 {alphabet[i] = alphabet[i].replace("2", &" ".repeat(i+i-1));}
	}
  
	let mut alp = alphabet.clone();
	alp.pop();
	while !alp.is_empty() {alphabet.push(alp.pop().unwrap());}
	
	return alphabet
}
```

Ok, what happen in the new code is :
* if vec alphabet = [··A··, ·B·B·, C···C].
* then, we clone it to vec alp and pop the last value, vec alp = [··A··, ·B·B·].
* after that, we will push the last value of vec alp to vec alphabet, until vec alp is empty.
* last, we will return vec alphabet who contains the diamond value = [··A··, ·B·B·, C···C, ·B·B·, ··A··].

Now, the problem is solved and the code will surely pass without error. 😀

---
### Extra part

* The code that I explain and post before, is actually not my very first code that I create and submit for the first time in Exercism. The first code is a little complicated than before... But, yeah I will just post this down for self reminder.  :)

``` rust
pub fn get_diamond(c: char) -> Vec<String> {
    //https://stackoverflow.com/questions/45343345/is-there-a-simple-way-to-generate-the-lowercase-and-uppercase-english-alphabet-i
    let mut alphabet = (b'A'..=c as u8)
        .filter_map(|x| {
            let s = 1;
            let x = x as char;
            if x.is_alphabetic() {
                if x == 'A' {Some(format!("{0}{1}{0}", s, x))}
                else {Some(format!("{0}{1}{2}{1}{0}", s+1, x, s+2))}
            } else { None }
        })          
        .collect::<Vec<_>>();
        
    let l = alphabet.len();
    
    for i in 0..l {
        let mut t: String = alphabet.remove(0);
        t = t.replace("1", &" ".repeat(l-1));
        t = t.replace("2", &" ".repeat(l-(i+1)));
        if i != 0 {t = t.replace("3", &" ".repeat(i+i-1));}
        alphabet.push(t);
    }
        
    let mut alp = alphabet.clone();
    alp.pop();
    for _i in 1..l {alphabet.push(alp.pop().unwrap());}

    alphabet
```

* And, when I review this and see *Why not add the trailing spaces directly?*, I get curious and try to find it again, then somehow found it. This a little simpler and just have 2 iteration i guess.

```rust
pub fn get_diamond(c: char) -> Vec<String> {
    let l = (((c as u8)-b'A'+1)) as usize;
    
    //https://stackoverflow.com/questions/45343345/is-there-a-simple-way-to-generate-the-lowercase-and-uppercase-english-alphabet-i
    let mut alphabet = (b'A'..=c as u8)
        .filter_map(|x| {
            let n = (x-b'A') as usize;
            let x = x as char;
            if x.is_alphabetic() {
                if x == 'A' { Some(format!("{1}{0}{1}", x, &" ".repeat(l-(n+1)))) }
                else { Some(format!("{1}{0}{2}{0}{1}", x, &" ".repeat(l-(n+1)), &" ".repeat(n+n-1))) }
            } else { None }
            }
        ).collect::<Vec<_>>();  
        
    let mut alp = alphabet.clone();
    alp.pop();
    while !alp.is_empty() {alphabet.push(alp.pop().unwrap());}
    
    return alphabet
}
```

Thank You! 😀
