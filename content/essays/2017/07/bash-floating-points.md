---
title: "Bash and Floating-Point Arithmetic"
date: 2017-07-11T23:10:42+01:00
draft: false
categories:
    - bash
---

This week I was working on HackerRank's [Arithmetic Operations](https://www.hackerrank.com/challenges/bash-tutorials---arithmetic-operations). Turns out, to my surprise, that Bash **does not** support floating point operations. I had no idea.

Off I went, in my searching. I searched through a lot of StackOverflow answers. The best one I found is [this one](https://unix.stackexchange.com/questions/40786/how-to-do-integer-float-calculations-in-bash-or-other-languages-frameworks) - a summary of possible solutions. This answer is objective and straight to the point, offering enlightenment into a lot of possible solutions to go around this problem. Nonetheless, it is dependent on the context which meant I needed to do some research.

I ended up using the solution involving [bc](https://www.gnu.org/software/bc/manual/html_mono/bc.html), _an arbitrary precision calculator language_. This nifty tool allows you to do floating-point calculations straight from your terminal.

Usage of the tool is pretty simple. You just pipe an expression into it, and it outputs the result of processing said expression. As an example, you can do stuff like this:

```
$ echo "3 / 2" | bc
1
```

As you can see, if you just pipe a usual expressions, it won't produce floating-point types. For that, you need to let `bc` know you want to see the actual decimals using `scale=X`. The above example, with this new information, would be:

```
$ echo "scale=3; 3 / 2" | bc
1.500
```

As you can see it now returns 3 decimal points - `1.500`. Unfortunately, this started showing some odd behaviour. For some calculations, it would round the decimal points incorrectly. I started searching and [found](http://unixetc.co.uk/2014/01/19/bc-rounding-errors/) that there's some _undefined behaviour_ when dealing with decimal points equal to 5. From 1 to 4, it rounds down. From 6 to 9, it rounds up. It starts getting very tricky when the rounding has to be done on a decimal point of 5.

This means that if I had a value of 1.5675, `bc` might actually round that to 1.567. I couldn't have that because, for my specific context, I needed 5 to be rounded up.

I found the solution to this problem by using `printf` along with `bc`. I make my calculations with `bc` and use `printf` for rounding. This has the possible issue of requiring you to decide on a fixed maximum decimal rounding but it works for me.

As an example:

```
$ echo "scale=2; 13 / 7" | bc
1.85
```

Scaling with `scale=1`, we get an undesired rounding:

```
$ echo "scale=1; 13 / 7" | bc
1.8
```

But when using `printf`:

```
$ printf "%0.1f\n" $(echo "scale=2; 13 / 7" | bc)
1.9
```

The more I use floating-points, the more I understand how complex they are. I'll probably never get to the bottom of it all but at least I'll never forget how `bc` and `printf` can, together, be a solution for this problem.
