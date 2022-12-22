You want to build a word cloud, an infographic where the size of a word corresponds to how often it appears in the body of text.

To do this, you'll need data. Write code that takes a long string and builds its word cloud data in a dictionary ↴ , where the keys are words and the values are the number of times the words occurred.

Think about capitalized words. For example, look at these sentences:

  'After beating the eggs, Dana read the next step:'
'Add milk and eggs, then add flour and sugar.'
What do we want to do with "After", "Dana", and "add"? In this example, your final dictionary should include one "Add" or "add" with a value of 22. Make reasonable (not necessarily perfect) decisions about cases like "After" and "Dana".

Assume the input will only contain words and standard punctuation.

You could make a reasonable argument to use regex in your solution. We won't, mainly because performance is difficult to measure and can get pretty bad.

Breakdown
We'll have to go through the entire input string, and we're returning a dictionary with every unique word. In the worst case every word is different, so our runtime and space cost will both be at least O(n)O(n).

This challenge has several parts. Let's break them down.

Splitting the words from the input string
Populating the dictionary with each word
Handling words that are both uppercase and lowercase in the input string
How would you start the first part?

We could use the built-in split() function to separate our words, but if we just split on spaces we'd have to iterate over all the words before or after splitting to clean up the punctuation. And consider em dashes or ellipses, which aren't surrounded by spaces but nonetheless separate words. Instead, we'll make our own split_words() function, which will let us iterate over the input string only once.

We'll check if each character is a letter with the string method isalpha().

Now how can we split each word? Let's assume, for now, that our helper function will return a list of words.

We'll iterate over all the characters in the input string. How can we identify when we've reached the end of a word?

Here's a simple example. It doesn't work perfectly yet—you'll need to add code to handle the end of the input string, hyphenated words, punctuation, and edge cases.

  def split_words(input_string):
    words = []
    current_word_start_index = 0
    current_word_length = 0

    for i, char in enumerate(input_string):
        if char.isalpha():
            if current_word_length == 0:
                current_word_start_index = i
            current_word_length += 1
        else:
            word = input_string[current_word_start_index:
                current_word_start_index + current_word_length]
            words.append(word)
            current_word_length = 0

    return words

Python 2.7
Careful—if you thought of building up the word character by character (using +=), you'd be doing a lot more work than you probably think. Every time we append a character to a string, Python makes a whole new string. If our input is one long word, then creating all these copies is O(n^2)O(n 
2
 ) time.

Instead, we keep track of the index where our word starts and its current length. Once we hit a space, we can use string slicing to extract the current word to append to the list. That keeps our split function at O(n)O(n) time.

Now we've solved the first part of the challenge, splitting the words. The next part is populating our dictionary with unique words. What do we do with each word?
