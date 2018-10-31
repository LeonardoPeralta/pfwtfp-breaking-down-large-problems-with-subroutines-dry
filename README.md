# DRY-ing Up Your Code

## Learning Goals

- Identify techniques for breaking down large problems: DRY

## Introduction

In the following code, we perform a similar task multiple times. We repeat
ourselves. When we do this, we increase the chance that we'll make a subtle
error in one of the repetitions.  When developers see repeated behavior 
in code, they "name" that behavior by putting the code into a subroutine.
This pattern has a fun abbreviation: Don't Repeat Yourself (DRY).

"DRY" code is:

* **Easier to fix**: if it's broken for one it's broken for all; if it's fixed for
  one, it fixes all
* **More visible**: Naming processes is important and subroutines help you understand what's
  important to your code. Naming things is a powerful magical act.
* **Less Distracting**: Parent code that uses the subroutine won't have its
  purpose muddled by repeated and similar bits of action

We'll see all these benefits as we "DRY out" some sandwich-making code.

### un-DRY Code

```ruby
# Required to make the Ruby work.
require 'ostruct'
demo_loaf = OpenStruct.new(:type => "light rye", :length => 60)
# (end GIVEN CODE)

def get_two_slices_from_loaf(loaf, width)
  slices = []
  slices_count = loaf.length / width
  slices_count.times do
    slices << "slice of #{loaf.type}"
  end

  if slices.length >= 2
    return slices[0,2]
  else
    raise ArgumentError, "Could not make enough bread from the loaf!"
  end
end

def make_pbj_sandwich(loaf, peanut_butter, jelly, slice_width)
  bread1, bread2 = get_two_slices_from_loaf(loaf, slice_width)

  # Join ingredients
  puts "You join #{bread1} and #{peanut_butter}"
  side1 = "#{peanut_butter} on #{bread1}"

  # Join ingredients
  puts "You join #{bread2} and #{jelly}"
  side2 = "#{jelly} on #{bread2}"

  # Join ingredient on bread
  puts "You join #{side1} with #{side2}"
  "A #{peanut_butter} and #{jelly} sandwich"
end

make_pbj_sandwich(demo_loaf, "crunchy monkey brand peanut butter", "Belgian forest-berry", 2)
```

In the code above, look at the implementation of `make_pbj_sandwich`.

In this method, 6/7 of the lines are about joining ingredients together and
outputting what they were. They're all more-or-less identical. Let's extract
that behavior to a subroutine and "DRY" out `make_pbj_sandwich`.

In the subroutine, we want to 

* `puts` what we've joined (as we do currently)
* return the `String` of `ingredient1 and ingredient2`

Try writing it out yourself on a piece of paper or in a new editor window.

We'll wait :)

No peeking!

![No peeking Lucy Ricardo](https://media.giphy.com/media/26u8y41tkhGq81fr2/source.gif)

## DRYing out the Code

Back already? Great. We think we can make the code simpler by writing this
method called `join_ingredients`.

```ruby
def join_ingredients(i1, i2)
  puts "You join #{[i1, 'and', i2].join(' ')}"
  "(#{i1} and #{i2})"
end
```

This method captures the behavior that's repeated 6 times in the
`make_pbj_sandwich` method. Our implementation becomes even clearer:

```ruby
def make_pbj_sandwich(loaf, peanut_butter, jelly, slice_width)
  bread1, bread2 = get_two_slices_from_loaf(loaf, slice_width)
  side1 = join_ingredients(bread1, peanut_butter)
  side2 = join_ingredients(bread2, jelly)
  join_ingredients(side1, side2)
end

make_pbj_sandwich(demo_loaf, "crunchy monkey brand peanut butter", "Belgian forest-berry", 2)

#=> You join slice of light rye and crunchy monkey brand peanut butter
#=> You join slice of light rye and Belgian forest-berry
#=> You join crunchy monkey brand peanut butter on slice of light rye with Belgian forest-berry on slice of light rye
```

Test out this new code in a test program or in IRB.

## Conclusion

In this lesson we've finished covering one of the most powerful tools a
programmer has for battling confusing code: the subroutine. It can be used to
hide away complex calculations or work and can help you store behavior that
you'll repeat multiple times. As you start in your programming career, use
subroutines to keep you thinking clear and your code simple. It will pay
dividends again and again.
