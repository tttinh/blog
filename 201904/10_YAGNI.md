# YAGNI

[Home](../README.md)

YAGNI, or **“You Ain’t Gonna Need It”** (or “You Aren’t Gonna Need It”), emerged as one of the key principles of Extreme Programming.  Put another way, the principle states:

```
Always implement things when you actually need them, never when you just foresee that you may need them.
```

Even if you're totally, totally, totally sure that you'll need a feature later on, don't implement it now. Usually, it'll turn out either

```
a) you don't need it after all.
```

or

```
b) what you actually need is quite different from what you foresaw needing earlier.
```

This doesn't mean you should avoid building flexibility into your code. It means you shouldn't overengineer something based on what you `think you might need later on`.

There are two main reasons to practise YAGNI:

*   You save time, because you avoid writing code that you turn out not to need.
*   Your code is better, because you avoid polluting it with 'guesses' that turn out to be more or less wrong but stick around anyway.

YAGNI maximizes the amount of unnecessary work that is left undone, which is a great way to improve developer productivity and product simplicity.  Remember, features are expensive, both to develop and maintain, and for users to learn and navigate around.  Features that aren’t actually necessary are a huge source of waste.

```
“Every line of code we don’t write is dollars we didn’t spend, 
and time on the calendar we get back for free.” – Tim Evans-Ariyeh
```

```
“The cheapest, fastest, and most reliable components of a computer system 
are those that aren’t there.” – Gordon Bell
```