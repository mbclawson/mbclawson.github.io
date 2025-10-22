+++
draft = true
date = 2025-10-18
title = "humbled by leetcode"
description = ""
tags = ["python"]
+++

If this post was a Youtube thumbnail, I'd be obliged to put a 'It's not what you think' in there for the clickbait factor.

Setting the stage quickly: I've been using the time after my latest contract ended to sharpen up my cloud and python skills.  My favorite site for python practice isn't `leetcode`, but actually a very similar site called `codewars` ([link](codewars.com)).  It took a little bit to build some inertia, but lately I've felt like I have been **crushing** problem sets.  I'm writing concise python, using cool methods, and coming up with solutions that map to the highest upvoted answers.

I was feeling GREAT until I hit this problem.

## The Problem
Take a string and replace characters with `(` if they appear once and `)` if they appear more than once.  Ignore casing.  Examples:

```python
# "recede"   =>  "()()()"
# "Success"  =>  ")())())"
# "(( @"     =>  "))(("
```

## The Thought Process
OK that's not too bad. Reaching for the toolkit - I'm looking for lists, maybe a for-loop, and definitely the `count()` and `lower()` methods.  I could see the solution building up in my head, similarly to how I'd write a long SQL query.  I'd trade longer code for readability and distinct steps that were easy to follow.

**But wait**.  I pictured the top solution.  It'd be a cool one liner.  It'd have list comprehension.  It'd be absolutely _flooded_ with 'Clever' upvotes.  I'd been practicing for a while now, so why settle for my original idea when I could submit 'the best'?

## The Solution
One liners start with `return` so let's begin with that.  We also know were going to use a list to do the manipulation we need to do, and will eventually have to join everything back together in a string for our answer.

```python
def duplicate_encode():
    return ''.join([])
```

Good start.  Now let's tackle the list comprehension.  The problem specifies to ignore casing, so we know we're also going to be calling `lower()` alongside our `count()`:

```python
def duplicate_encode(word):
    return ''.join(['(' if word.lower().count(c) == 1 else ')' for c in word.lower()])
```

Run code.  All test cases pass.  Submit solution.  Even more test cases pass.  Check top solution.  It's mine.  Enter dopamine.

## The Crash
I'm not sure what compelled me to do this, but for the first time ever I start to read the comments below the top solution.  I'm expecting to see a bunch of self-congratulatory comments.  "Amazing solution".  "nice".  "gg".

Instead I see different.  "This is hard to read and debug".  "Don't call `lower()` multiple times".  **"This is slow".**

My heart sank.

## The Testing
I focused on the solution being slow.  When I first hit submit, my solution was tested instantly.  So what was slow?  Well computer science isn't hard because you have to do something one time, it's hard because you have to do something millions of times (give me some rope here).

I took a brief detour into learning about python decorators to get simple function timing in place:

```python
import time
from functools import wraps

def timer(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        start = time.perf_counter()
        result = func(*args, **kwargs)
        end = time.perf_counter()
        print(f"{func.__name__} took {end - start:.10f} seconds")
        return result
    return wrapper
```

I then implemented some other ways to solve this problem per what I saw in the comments:

```python
@timer
def duplicate_encode_faster(word):
    word_lower = word.lower()
    ans = ''
    for c in word_lower:
        if word_lower.count(c) == 1:
            ans += '('
        else:
            ans += ')'
    return ans

@timer
def duplicate_encode_fastest(word):
    word = word.lower()
    counter = Counter(word)
    return ''.join(('(' if counter[c] == 1 else ')') for c in word)

long_string = '*' * 100000
duplicate_encode(long_string)
duplicate_encode_faster(long_string)
duplicate_encode_fastest(long_string)
```
Results:
```python
python testing.py
duplicate_encode took 6.2152794580 seconds # my original solution
duplicate_encode_faster took 3.7220240000 seconds
duplicate_encode_fastest took 0.0060575830 seconds
```
## The Takeaways
I learned a lot from running other people's examples and also further reinforced habits I've picked up throughout my career:

- Don't sacrifice readability to look clever - trading extra lines for clarity is almost always worth it
- Don't do something a single time more than you have to - in our case `lower()` should have been called once instead of multiple times
- Don't get sucked into the `leetcode` trap - top solutions rarely mimic real life so write code you expect others to read
- There is always a bigger fish - I was able to arrive at `duplicate_encode_faster` independently however I still have to check out `collections.counter` to see what that's all about

---
I'm going to continue to do `codewars` problems but in the future I'll be more mindful of performance and keeping code readable. Hopefully, you learned a little something about leetcode, python performance, and getting humbled from this post!
