<p><a target="_blank" href="https://app.eraser.io/workspace/kHp070cwYZHUvw8DVmIw" id="edit-in-eraser-github-link"><img alt="Edit in Eraser" src="https://firebasestorage.googleapis.com/v0/b/second-petal-295822.appspot.com/o/images%2Fgithub%2FOpen%20in%20Eraser.svg?alt=media&amp;token=968381c8-a7e7-472a-8ed6-4a6626da5501"></a></p>

---

## layout: page
## title: Case study
subtitle: Why you'd want to go on a date with me

## Abstracts
I'm a big user of Google Docs for writing. One day, I became curious about how its collaboration feature actually works. This sparked an idea: why not try to create something similar myself?

And just like that, the concept for ChainChat, a collaborative text editor, was born.

The main goal of this project is to develop a collaborative text editor, not just any text editor. To achieve this, a library called Slate.js was opted for. It's an excellent starting point as it already offers a robust foundation for building text editors in React. The plan is to extend Slate.js to incorporate the collaborative features necessary for real-time editing with multiple users.

## The basics of a text editor
**What is a Text Editor?**

A text editor serves as a platform for manipulating text, allowing users to add or remove characters and save the edited text in a file.

![Basic Text Editor Operations](/assets/img/kHp070cwYZHUvw8DVmIw___plChzyHPUBexUglVwzpTogxbaxO2___---figure---mDLgjfyHqv81A0KYBLadA---figure---Po90iloTIXIuhAO9NlpkCg.png "Basic Text Editor Operations")

**Character Representation:**

In a text document, each character possesses a distinct value, such as letters, numbers, or symbols, and occupies a specific numerical index denoting its position within the document. For instance, in the sentence "The quick brown fox," each letter ("T", "h", "e", etc.) occupies a unique position within the sequence.

**Insertion Operations:**

Inserting a new character involves utilizing the insert() function, where users specify both the character to be inserted and its intended position within the text. For example, executing `insert("C", 0)` would add the character "C" at the beginning of the text, causing subsequent characters to shift accordingly.

**Deletion Operations:**

Conversely, deleting a character entails employing the `delete()` function, where users specify the index of the character to be removed. For instance, `delete(1)` would erase the character at index 1, leading to subsequent characters adjusting their positions accordingly.

## The basics of a collaborative text editor
**1. Setting Up Collaborative Editing:**

Before diving into collaborative editing, it's essential that each participant has their own version of the document. This enables real-time changes, similar to working individually. To synchronize everyone's efforts, an initial version of the project introduces a Central Relay Server. Think of it as a mediator facilitating communication about document changes among users.

**Challenges in Concurrent Editing:**

Collaborative editing, as seen in platforms like Google Docs, differs from traditional text editors like Notepad. In a collaborative setting, multiple users can edit the same document simultaneously.

However, when several users edit a document concurrently, complications arise. Imagine two users adding and deleting text simultaneously—without coordination, this could lead to chaos and inconsistencies in the document. This is where the concepts of **commutativity** and **idempotency** come into play.

**Understanding Commutativity:**

Collaborative editing poses the challenge of ensuring all participants end up with an identical final document despite simultaneous edits. The issue stems from the fact that the order of applying edits can alter the outcome. For instance, if you insert a paragraph before your friend deletes a sentence, the result differs from if your friend deletes first and then you add.

![Non-Commutative Example](/assets/img/kHp070cwYZHUvw8DVmIw___plChzyHPUBexUglVwzpTogxbaxO2___---figure---afcZpaO8GQ-NBY0i6eLuz---figure---A9oEUAJsFwWoMdlM1uOwxA.png "Non-Commutative Example")

Commutativity ensures that the order of operations doesn't affect the final document. If operations were commutative, the sequence of edits wouldn't matter—the document's outcome would remain consistent. However, non-commutative operations, like simultaneous insertion and deletion, can hinder document convergence, leading to confusion among collaborators.

**Understanding Idempotency:**

In tools like Google Docs, applying the "find and replace all" function to replace a word and clicking "replace" again doesn't produce any changes because all occurrences of the word have already been replaced. This property, known as idempotency, ensures that repeating an operation doesn't alter the document beyond its initial application.

![Non-Idempotency Example](/assets/img/kHp070cwYZHUvw8DVmIw___plChzyHPUBexUglVwzpTogxbaxO2___---figure---WJ_RmazexGFmb5V9kthXV---figure---86jDh0GmgUOAfzWjVnTkzw.png "Non-Idempotency Example")

In collaborative editing, idempotency guarantees that repeating an operation produces the same result each time. This consistency prevents unexpected changes, ensuring a smoother editing experience for all users involved.

**Engineering Perspective on Idempotent Concepts:**

In programming and data manipulation, idempotent functions and operations play crucial roles. Idempotent functions, like those in Python, produce the same output with repeated calls using the same input, without altering the program's state. Similarly, idempotent operations in data management can be applied multiple times without changing the result beyond the initial application.

In summary, commutativity ensures consistency in document editing, while idempotency ensures predictability and stability, akin to collaborative editing scenarios where repeated actions yield consistent results.

**Principles for Document Convergence:**

These principles are vital for achieving document convergence, ensuring that despite simultaneous edits by multiple users, everyone ultimately sees the same document. Collaborative editing thrives on seamless teamwork, where everyone's contributions harmonize to create a unified, cohesive document.

## Operational Transforms
Operational Transformation (OT) is an algorithm devised to address concurrent operations in collaborative editing scenarios. Its primary function is to detect potential conflicts between operations that may prevent documents from converging. If such conflicts are identified, OT modifies or transforms the operations to ensure convergence.

![Operational Transformation Proccess](/assets/img/kHp070cwYZHUvw8DVmIw___plChzyHPUBexUglVwzpTogxbaxO2___---figure---rche3MWil0n9WbxD2d2a2---figure---XQmzv9hf5MLcZCZknogVvQ.png "Operational Transformation Proccess")

For instance, consider a scenario where User1 receives a `delete(0)` operation from User2. OT recognizes that since we inserted a new character at position 0, User2's operation must be transformed to `delete(1)` before it can be applied.

OT facilitates commutativity and idempotency in operations. It ensures that insertion and deletion operations can be applied in any order without altering the document's outcome. Furthermore, when attempting to delete a character that has already been deleted, OT recognizes and skips redundant operations.

While OT was initially popular and utilized in collaborative editors like Google Wave, Etherpad, and Firepad, its implementation proved challenging. Engineers encountered difficulties due to the complexity of OT algorithms and the time-consuming nature of correct implementation.

## Conflict-Free Replicated Data Type
CRDTs emerged as an alternative strategy while researchers sought to enhance and simplify Operational Transformation (OT). Unlike OT, which maintains the basic structure of a text editor and relies on algorithms to ensure commutativity and idempotency, CRDTs take a different approach.

Rather than treating characters solely based on their value and absolute position, CRDTs augment the underlying data structure of the text editor. Each character object is endowed with additional properties to facilitate commutativity and idempotency. This structural change enables a simpler algorithm compared to OT.

It's important to note that there's a variety of CRDT types tailored for different use cases. In the context of collaborative text editing, CRDTs aim to achieve consistent data replication without coordination between replicas. Since text documents require characters to maintain a specific order, the CRDT type used here falls under sequence CRDTs.

For effective implementation in a collaborative text editor, CRDTs must meet specific requirements.

### Globally Unique Characters
![Peer Sending Data](/.eraser/kHp070cwYZHUvw8DVmIw___plChzyHPUBexUglVwzpTogxbaxO2___---figure---3jC5uUJq6BLjdkSOFEHrf---figure---iiG8aha692RDz58as9fJRQ.png "Peer Sending Data")

![Globally Unique Characters](undefined "Globally Unique Characters")

In the system, it's crucial that each character is globally unique. This is achieved by assigning a Site ID and Site Counter to every new character upon insertion. As the Site Counter increments with each insertion or deletion at a site, the uniqueness of all characters across the system is ensured.

Having globally unique characters allows precise deletion instructions when a delete request is sent from one user to another. This ensures that the correct character is targeted for deletion.

With the CRDT approach, duplicate deletion operations become idempotent. When a delete operation is received by a user, the globally unique character to delete is checked. If the character has already been deleted, the operation becomes redundant. This idempotency is achieved by maintaining globally unique character objects.

### Globally Ordered Characters
In a collaborative text editor, maintaining the order of characters within a document is essential. However, for each user to have a consistent view, the ensuring that character positions are globally ordered is a necessity.

When a character is inserted by a user, it should appear in the same position for all users. In the initial editor, surrounding characters might shift when a character is inserted or deleted, leading to non-commutative operations.

To ensure commutativity, fractional indices are used instead of numerical ones. For instance, inserting "E" between "T" and "H" at position 1 becomes inserting at position 0.5, represented as [0, 5].

![Fractional Index](/assets/img/kHp070cwYZHUvw8DVmIw___plChzyHPUBexUglVwzpTogxbaxO2___---figure---af3nhG0XHUWE6_aPsAgVe---figure---2RQsi6dTX81JE5PmZWWgNQ.png "Fractional Index")

Using fractional indices avoids shifting surrounding characters. It can be thought of as inserting characters into a tree structure. If no space exists between positions, the next level is moved to.

![Fractional Index](/assets/img/kHp070cwYZHUvw8DVmIw___plChzyHPUBexUglVwzpTogxbaxO2___---figure---XbgyCnybU-vwcl7nsPPgl---figure---51SWVFGIXxPhWhew2jx3Ew.png "Fractional Index")

The CRDT approach allows globally unique characters with fractionally indexed positions. This means that deleting a character doesn't affect inserting a new one, ensuring commutativity.

To handle simultaneous insertions by multiple users, the Site ID is appended to position identifiers as a tiebreaker, ensuring globally unique characters.

## Implementing CRDT
After understanding the theory behind CRDTs, how can one implement them in code?

**Creating a Web Text Editor**
As previously mentioned, inserting a globally unique character into the text editor entails adding a node to a tree structure. This project leverages the open-source SlateJs text editor because its underlying data structure operates similarly to a tree.

Given that both utilize tree structures, integrating the CRDT functionality seamlessly aligns with the editor's existing architecture, facilitating smoother synchronization of CRUD operations between the CRDT and the text editor.

![Tree Data Structure](/.eraser/kHp070cwYZHUvw8DVmIw___plChzyHPUBexUglVwzpTogxbaxO2___---figure---q5BDpltJQC0kcHJ9VGMAl---figure---Hke3nTmr_8IIjzKBrAdWLg.png "Tree Data Structure")



**Designing the CRDT Structure**

Furthermore, our CRDT needs to manage four fundamental operations:

1. Local Insert: When a user inserts a character into their local text editor, the operation is transmitted to all other users.
2. Local Delete: When a user deletes a character from their local editor, the deletion operation is broadcasted to all other users.
3. Remote Insert: Upon receiving an insert operation from another user, the recipient inserts the character into their local editor.
4. Remote Delete: Upon receiving a delete operation from another user, the recipient removes the corresponding character from their local editor.
## Challenges with Central Relay Server Architecture
This inital prototype of the project operated on a client-server communication model, enabling collaborative document editing among multiple users. A central server relays operations to all participants within the document's network.

Originally chosen to handle editing conflicts, this architecture was reevaluated due to its limitations.

Firstly, there's a notable issue with high latency between users. All operations, regardless of user proximity, are routed through the server. Consequently, even users physically close to each other may experience delays if the server is located elsewhere.

Secondly, scalability presents financial hurdles. With a growing user base, the server's workload increases, demanding more resources and incurring higher costs. Keeping expenses low is a key concern for this open-source project.

Additionally, the client-server model necessitates users to trust the server and associated entities with their data, including developers, hosting services, and potentially regulatory bodies.

Lastly, dependence on a central server introduces a single point of failure. Any downtime experienced by the server leads to an immediate loss of collaborative capabilities for all users. It's crucial to explore alternative architectures to mitigate these risks.

## Peer-to-Peer Architecture
To overcome these limitations, a transition to a peer-to-peer architecture is made where operations are sent directly to each other by users. In this system, rather than having a single server and multiple clients, each user acts as both a client and a server. This eliminates the need for a centralized server to relay operations, allowing users to perform this task themselves at no additional cost. Essentially, users become responsible for relaying operations to others they are connected to.

## Facilitating Direct Messaging
To enable nodes to send and receive messages directly, a technology called WebRTC is leveraged. WebRTC is a protocol designed for real-time communication over peer-to-peer connections. While it's primarily intended for audio or video calling without plugins, its simplicity makes it suitable for text messaging needs.

While WebRTC allows direct communication between users, a small server is necessary to initiate these peer-to-peer connections through a process called "signaling."

It's crucial to note that although WebRTC relies on the signaling server, no document content passes through it. It's solely used to establish connections. Once a connection is established, the server becomes redundant.

When a user first accesses ChainChat, the application establishes a WebSocket connection with the server. Through this connection, the app registers with the signaling server, indicating its location. In response, the server assigns a random, unique Peer ID to the user. The application then generates a Sharing Link, unique to each user, which serves as a pointer to that user. Users can share this link, and upon clicking, collaborators are automatically connected to collaborate on the shared document.

To implement signaling and WebRTC messaging, a library called PeerJS is utilized, which handles much of the process automatically. For instance, when a user clicks another user's sharing link, it requests the signaling server to broker a connection between them. The server responds by providing the user with the other user's IP address, enabling direct messaging.

Since many internet users employ wireless routers, the public IP address is determined using a STUN server. The connecting user then uses this IP address to establish a WebRTC connection. In cases where a connection to the STUN server fails and the WebRTC connection is disrupted, a TURN server acts as a backup to relay operations between users.

In summary, the signaling server facilitates the establishment of WebRTC data connections, enabling direct messaging between users.

## Version Vector
WebRTC, a protocol leveraged for real-time communication over peer-to-peer connections, operates on the UDP transport protocol. While UDP facilitates rapid message transmission without awaiting responses, it lacks the assurance of in-order packet delivery. In essence, messages may reach their destination in a different order than they were sent, potentially disrupting the intended sequence of operations.

Consider a scenario where three peers are collaborating on a document. Peer 1 initiates an operation by typing an "A" and dispatching it to both peers. However, Peer 2, in close proximity, swiftly receives the operation but opts to delete it. Consequently, both the insert and delete operations traverse toward Peer 3. Due to the inherent unpredictability of the Internet, the delete operation might reach Peer 3 before the insert operation.

![Figure 1](/.eraser/kHp070cwYZHUvw8DVmIw___plChzyHPUBexUglVwzpTogxbaxO2___---figure---xnbNt23QSlHrY0qkCmuOn---figure---dj1SQnZ-Dhpx7gLlX1Wcuw.png "Figure 1")

To tackle this challenge, the Version Vector strategy was devised. This approach meticulously tracks the operations received from each user, ensuring coherent sequencing.

When an operation is dispatched, it encompasses not only the character object and its nature (insertion or deletion) but also the character's Site ID and Site Counter value. The Site ID identifies the operation's originator, while the Counter signifies the operation number from that user.

Upon receipt of a delete operation, it's stored in a Deletion Buffer. The buffer undergoes processing after each operation to ascertain if the characters have been inserted. If the delete operation's character is absent in the version vector, indicating it hasn't been inserted, the delete operation remains in the buffer until the insert operation surfaces.

![Worse Version Vector Case](/.eraser/kHp070cwYZHUvw8DVmIw___plChzyHPUBexUglVwzpTogxbaxO2___---figure---C1WHuEh4brEYfycUFHo_1---figure---kAmxms3ohk-x98-5oFdsmA.png "Worse Version Vector Case")

Once the insert operation materializes and is executed, the deletion buffer undergoes another round of processing. This time, the delete operation is extracted from the buffer and executed.

Furthermore, to forestall the application of duplicate operations, a guard clause is employed to verify if an operation has been executed before processing it.

With these pivotal components in place, an application integrates a custom-built CRDT alongside the Version Vector to ensure document replicas converge seamlessly. The `Broadcast` module manages the transmission of WebRTC messages, while the Editor empowers users to engage with their local document copies.

In essence, the architecture heralds a peer-to-peer, real-time collaborative text editor, fostering direct message exchange among users and cultivating a secure and private collaboration environment.

## Implementing CRDT
## Optimization
When developing with ChainChat, elements within the user experience that needed refinement were identified. These observations led to categorizing areas for improvement into 2 distinct sections:

1. Streamlining Peer-to-Peer Connection Management
2. Introducing New Editor Features
### Improving Peer-to-Peer Connectivity
Enhancing the management of WebRTC connections among users was the primary goal. Although WebRTC facilitates direct user-to-user connections, it falls upon developers to effectively handle and distribute these connections throughout the network.

It's crucial to clarify that in this context, the term "network" refers to all users collaborating on a document, forming a web of peer-to-peer connections. Here, the "network" comprises users exclusively, without any involvement of servers.
Initially, a challenge was faced with users becoming stranded or disconnected from the network. Imagine a scenario with three users, as illustrated below:

Peer 2 and Peer 3 rely on Peer 1 as a message relay. But what happens if Peer 1 leaves?

Peer 2 and Peer 3 are now disconnected, impeding their collaboration. To tackle this issue, a mechanism for users to find each other was needed.

**Listing Users and Discovering Peers**

The solution involved each peer maintaining a comprehensive list of all other users in the network. This list gets updated whenever a user joins or leaves the network.

This ensures that each user is aware of all others in the network, even if they're not directly connected. Consequently, if a connected user leaves, they can select someone from their network list to establish a connection, thus maintaining collaboration.

While this approach is somewhat reactive, the feasibility of a proactive solution was explored. Could a single point of failure be prevented altogether? After all, mitigating such risks was a primary aim behind creating a peer-to-peer network.

**Automated Network Adjustment**

In the Peer-to-Peer Architecture, the process of swiftly connecting to a peer upon clicking their sharing link was explained. However, the initial user sharing the document link with several others could result in a single point of failure, somewhat resembling a client-server architecture.

To address this, an assessment step was introduced before connecting a new user to the network. When a new user attempts to join the document or network, a connection request is first sent.

The recipient evaluates the request to determine if the maximum number of connections has been reached. If so, the request is forwarded to another user in the network. If not, the connection is accepted and reciprocated.

To determine the maximum number of connections, the average number desired for each user was calculated. Given the network's potential growth, a fixed or linear scaling approach could prove unreliable. Hence, a logarithmic scale was opted for.

However, through extensive testing, unexpected bugs were encountered when the network had fewer than five users. Consequently, five was set as the baseline, transitioning to logarithmic growth as the network expands.

While the solution reduces the likelihood of a single point of failure, it doesn't completely eliminate the formation of bottlenecks.

### Revamping Editor Functionality
Upon exploring ChainChat, (family/friend) users testing identified areas for improvement within the in-browser editor. The enhancement of two vital aspects was prioritized: remote cursors and video chat integration

**Remote Cursors**
In the realm of collaborative editing, clarity and coordination are paramount. To mitigate potential confusion stemming from concurrent edits, remote cursors were introduced. Each user is now represented by a distinct cursor, characterized by a unique color and animal combination. Implementing this feature in a decentralized environment posed challenges, necessitating the development of a custom hashing algorithm to ensure consistency across users.

**Video Chat**
Expanding ChainChat's communication toolkit, video chat functionality was seamlessly integrated using WebRTC. This enhancement empowers users to initiate video calls with peers, enabling real-time visual communication within the collaborative workspace.

## Exploring Future Innovations
**Diversified Connection Strategies**

To enrich the collaborative experience, strategies to optimize connection distribution within the network are being explored. One promising approach involves new users initially establishing connections with multiple peers. This proactive measure is designed to minimize the likelihood of collaborators becoming isolated due to connection disruptions, thereby fostering a more seamless collaboration environment.

**Efficient Handling of Bulk Operations**

While the current CRDT framework is tailored for single-character insertions and deletions, there's recognition of the need for enhancing the system to accommodate bulk text manipulations. Enabling the simultaneous addition or removal of text chunks in a single operation holds the potential to significantly reduce processing overhead and enhance the efficiency of extensive edits, such as large-scale cuts, pastes, and uploads.

**Streamlined Testing for P2P Network**

Validating the peer-to-peer (P2P) capabilities of ChainChat presents unique testing challenges. While manual testing has been instrumental thus far, it's resource-intensive and susceptible to human error. To simulate diverse latency scenarios akin to real-world conditions, there's a vision of deploying servers across diverse geographical locations. While this approach is feasible in theory, its execution currently surpasses the resource capacity.

**Enhanced Document Management**

In the absence of a centralized document storage server, facilitating local document saving became imperative. A download feature is needed, allowing users to export the current document as a plain text file to their device.

Additionally, to streamline collaboration on existing documents, file upload functionality is needed. To allow wsers to effortlessly upload files, which can seamlessly integrated into the collaborative environment. This enhancement will foster flexibility and continuity in document editing endeavors.


<!--- Eraser file: https://app.eraser.io/workspace/kHp070cwYZHUvw8DVmIw --->