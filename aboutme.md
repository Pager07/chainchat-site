---
layout: page
title: Case study
subtitle: Why you'd want to go on a date with me
---

## Abstract

I'm a big fan of Google Docs for writing.

One day, I found myself wondering how its collaboration feature actually functions.

That curiosity sparked an idea: why not try to create something similar myself?

And just like that, the concept for ChainChat, a collaborative text editor, was born.

So, the main aim of this project is to create a collaborative text editor, not just any text editor.  
To make this happen, I decided to go with a library called Slate.js.  
It's a great starting point because it already provides a solid foundation for building text editors in React.  
My plan is to build on top of Slate.js to add the collaborative features we need for real-time editing with multiple users.

## What is a text editor?

**Definition of a Text Editor:**

- It's somewhere you can manipulate text by adding or removing characters and subsequently saving the edited text in a file.

---

**Character Representation**  
Each character within the text holds a unique value, such as letters, numbers, or symbols, and occupies a specific numerical index denoting its position within the document.

- For instance, in a sentence like "The quick brown fox," each letter ("T", "h", "e", etc.) occupies a distinct position within the sequence.

---

**Insertion Operations**  
Inserting a new character involves utilizing the insert() function, which allows users to specify both the character to be inserted and its intended position within the text.

- For example, executing `insert("C", 0)` would add the character "C" at the beginning of the text, shifting subsequent characters accordingly.

---

**Deletion Operations**  
Conversely, deleting a character entails employing the `delete()` function, where users specify the index of the character to be removed.

- For instance, `delete(1)` would erase the character at index 1, resulting in the subsequent characters adjusting their positions accordingly.

---

## What is a collaborative text editor?

Certainly! Here's a breakdown of the key concepts addressed about collaborative text editing into different parts:

**1. Setup for Collaborative Editing:**
When we're getting ready for collaborative editing, we want to make sure everyone involved has their own version of the document to work on. This way, each person can make changes in real-time, just like they would if they were working alone. To make sure everyone's on the same page, we introduce something called a Central Relay Server. Think of it as the middleman that helps everyone talk to each other about what changes they're making to the document.

**Challenges in Concurrent Editing:**
Collaborative editing, like what you might find in Google Docs, is a bit different from your basic text editor, like Notepad. In a simple text editor, it's just you working on the document. But in collaborative editing, multiple people can work on the same document at the same time.

Now, when you have several people editing a document together, things can get a bit tricky. Imagine if two people are adding and deleting text simultaneously. Without careful coordination, this could lead to chaos and inconsistencies in the document. That's where the concepts of **commutativity** and **idempotency** come into play.

**Concept of Commutativity:**
The challenge with collaborative editing is ensuring that everyone ends up with the same final document, despite making edits at the same time. This challenge arises from the fact that the order in which edits are applied can change the outcome. For instance, if you insert a paragraph before your friend deletes a sentence, the result will be different from if your friend deletes the sentence first and then you add the paragraph.

This phenomenon is called commutativity, and it's crucial for collaborative editing to work smoothly. Commutativity means that the order in which operations are applied doesn't change the final outcome. In our example, if adding and deleting were commutative operations, it wouldn't matter who made their edit first—the final document would look the same.

However, when operations aren't commutative, like in the case of simultaneous insertion and deletion, the final document state may not converge. This means that even though both edits happened, the document doesn't settle on a consistent final version. This lack of convergence can lead to confusion and inconsistency among collaborators, highlighting the importance of commutativity in ensuring that collaborative editing functions effectively.

**Concept of Idempotency:**
In Google Docs, when you use the "find and replace all" function to replace occurrences of a word, if you click "replace" again for the same word, nothing happens because all instances of that word have already been replaced. Each subsequent click of the "replace" button doesn't introduce any additional changes to the document because the operation is idempotent—it doesn't alter the result beyond its initial application.

In the world of collaborative editing, idempotency means that if you delete something from a document and then delete it again, you should end up with the same result both times. In other words, repeating the delete operation shouldn't change the document any further.

This concept is pretty important because it ensures consistency in the editing process. If deleting something repeatedly led to different outcomes each time, things would get messy pretty quickly. With idempotency, you can be confident that your edits won't cause unexpected changes or surprises, making the collaborative editing experience smoother and more predictable for everyone involved.

If you are an engineer, these ideas might help you understand the idea of Idempotent:

Forexample in programming, particularly in areas like functional programming and data manipulation. Two common concepts that align with the idea of idempotency are idempotent functions and idempotent operations.

1. Idempotent Functions: In Python, an idempotent function is one that, when called multiple times with the same input, produces the same output and has no additional side effects. This means that invoking the function repeatedly with the same arguments doesn't change the program's state beyond its initial invocation. For example, a function that increments a counter by 1 each time it's called is not idempotent, while a function that returns the square of a number is idempotent because calling it with the same input will always yield the same result.
2. Idempotent Operations: Similarly, in data manipulation or database operations, idempotent operations refer to actions that can be applied multiple times without changing the result beyond its initial application. For instance, in database management systems, an idempotent operation might involve updating a record with a specific value. If the operation is applied multiple times, the record will still have the same value as it did after the first application of the operation.

In summary, commutativity ensures that everyone sees the same document, no matter who made their edits first. While idempotency ensures predictability and consistency in program behavior and data manipulation, much like in collaborative editing scenarios where repeated actions should yield the same result without unintended changes.

**Principles for Document Convergence:**
These principles are super important because they help us achieve something called document convergence. That's just a fancy way of saying that no matter how many people are editing the document at the same time, everyone eventually sees the same thing. It's like making sure everyone's changes come together harmoniously to create one unified document. And that's what collaborative editing is all about—working together seamlessly to produce something awesome!
