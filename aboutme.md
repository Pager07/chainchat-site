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

## Operational Transforms

Operational Transformation (OT) is an algorithmic solution aimed at addressing challenges in collaborative editing environments by comparing concurrent operations performed by multiple users, adjusting them as necessary to prevent potential divergence in the document's content, and seamlessly integrating edits to ensure document coherence.

### Example of OT

Let's consider a simple text document that initially contains the word "hello." Now, imagine two users, Alice and Bob, are simultaneously editing this document using an Operational Transformation (OT) algorithm.

1. **Initial State:**

   - Document: "hello"

2. **Alice's Edit:**

   - Alice decides to insert the word "world" at the beginning of the document. So, her operation is to insert "world" at index 0.

3. **Bob's Edit:**
   - At the same time, Bob decides to delete the letter "o" from the word "hello." His operation is to delete the character at index 4.

Now, here's where OT comes into play:

- When Alice sends her insertion operation to the server, the server checks if it conflicts with any other concurrent operations. Since Bob's deletion operation (deleting the "o") occurs after Alice's insertion position (index 0), there is no conflict.
- However, when Bob sends his deletion operation to the server, it conflicts with Alice's insertion because it affects the position where Alice intended to insert "world."

- OT algorithm detects this conflict and transforms Bob's delete operation based on Alice's insertion operation. So, Bob's operation is transformed to delete the character at index 5 instead of index 4.

After the transformations, both operations can be applied in the correct order without causing any inconsistencies. So, Alice's "world" is inserted at the beginning of the document, and Bob's "o" is deleted from the word "hello." The final document state is "worldhell".

This example demonstrates how OT ensures that concurrent edits from multiple users are seamlessly integrated to maintain document coherence and consistency.

In practical terms, OT transforms concurrent operations to maintain document convergence, ensuring consistency and coherence even when multiple users make simultaneous edits.

### Drawback of OT

However, despite its effectiveness, implementing OT poses substantial challenges due to the complexity of the underlying algorithms, often requiring significant time and effort to develop robust OT systems, as acknowledged by the author.

## Conflict-Free Replicated Data Type

Researchers discovered an alternative to Operational Transformation (OT) called Conflict-Free Replicated Data Type (CRDT).

While OT focuses on maintaining the basic structure of a text editor and relies on complex algorithms to achieve collaboration, CRDTs take a different approach.

They modify the underlying data structure of the text editor, adding properties to each character object to enable collaboration more efficiently.

Unlike OT, which primarily uses algorithms, CRDTs use a more complex data structure to achieve simplicity.

It's worth noting that there are various types of CRDTs tailored to different needs, with sequence CRDTs being the most relevant for collaborative text editors.

These CRDTs allow for consistent data replication without the need for coordination between replicas.

To use CRDTs specifically for a collaborative text editor, there are a couple critical requirements.

### Globally Unique Characters

In collaborative editing, it's crucial for each character in the document to have a unique identifier to prevent conflicts and ensure consistency across multiple users.

This uniqueness is achieved by assigning two properties to each character upon insertion: a Site ID and a Site Counter.

The Site ID identifies the user or site where the character was inserted, while the Site Counter keeps track of the number of insertions or deletions at that specific site.

As users make changes to the document, the Site Counter increments accordingly, ensuring that each character's identifier remains globally unique.

With globally unique character identifiers, delete operations can accurately target and remove specific characters, contributing to the overall consistency and reliability of collaborative editing.

#### Example

When a user initiates a delete operation, the CRDT checks whether the targeted character still exists in the document based on its globally unique identifier.

If the character has already been deleted, the delete operation becomes redundant, as the desired outcome (the absence of the character) has already been achieved.

TADAA!! Idempotency achieved!

This idempotent behavior is made possible by leveraging the globally unique character identifiers assigned to each character object, simplifying the handling of delete operations and minimizing inconsistencies in the collaborative editing process

### Globally Ordered Characters

The 2nd requirement for a collaborative text editor CRDT has to do with the positioning of characters.

In collaborative text editing, consistency in the order of characters across all copies of the document is crucial for maintaining coherence.

Each user's addition of a character must reflect uniformly across all copies, ensuring that the same character occupies the same position in every instance of the document.

A hierarchical tree structure, unlike numerical indices, allow for seamless insertion of characters between existing ones without displacing neighbouring characters.

A hierarchical tree structure enables characters to be inserted at various levels between existing positions.

The path required to reach the character/node in the tree structure, represent positions as lists of integers, safeguarding the positions of adjacent characters during insertions.

This project implements adaptive allocation strategy, specifically, LSEQ, to intelligently allocate positions for new character insertions.

**Commutativity of Insertion and Deletion Operations:**
Utilizing globally ordered characters with fractional indices ensures that insertion and deletion operations can be executed in any order without altering the document's final state.

Concurrent insertion and deletion operations by different users harmoniously coexist without causing conflicts, as each character is uniquely identified by its fractional position.

Insertion of a character does not impede the deletion of another character, preserving document consistency across multiple users.
