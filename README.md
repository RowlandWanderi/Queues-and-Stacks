
# Stacks and Queues

In this lesson, we're going to learn about stacks and queues. We'll start by covering a brief overview of each. Then we'll write basic stack and queue functions.

Stacks and queues are both data structures that hold a list of elements. However, there is a key difference in how they work. A queue is first in, first out or FIFO. On the other hand, a stack is last in, first out or LIFO.

Let's use some examples of how both the FIFO and LIFO principles apply in our daily lives.

    When we get in a line (at the grocery store checkout counter, to go to a movie, or anything else), we expect the first person in line to get served first and so on. This is a prime example of FIFO.
    On the other hand, let's say we are reorganizing and stacking books from a bookshelf one at a time. When we take a book off the stack, we will most likely take it off the top of the stack, not the bottom. This is an example of LIFO because the last book added to the stack is the first one that's taken off the stack.

There are plenty of examples in computer programming where we'll use queues and stacks as well. For instance, waiting to download something and others are queued to download that thing first.

When it comes to stacks, we work with the JavaScript call stack every time we write JavaScript code. We can see this clearly in the following example:

function first() {
  return second();
}

function second() {
  return third();
}

function third() {
  return "hello!"
}

When we call first(), what happens? first() calls second() which calls third(). But which one is actually resolved first? Well, first() can't be resolved until second() is - and second() can't be resolved until third() is. How can the first() function ultimately return "hello!" unless the third() function resolves first?

This is the stack:

TOP OF THE STACK

third()
second()
first()

BOTTOM OF THE STACK

The technical term for each function is a stack is a frame. Whenever a function is called in the JavaScript runtime (the time our code is actually executed), the runtime creates a stack frame for that function. There is a limit to that stack - which you've probably noticed if you've ever run an infinite loop by accident and received a Range error: maximum call stack size exceeded error.

By the way, if you are confused about what the runtime is, in the Chrome browser or in Node (JavaScript backend environment) the runtime is the V8 engine.

And when it comes to queues, we are actually working with a queue every time we run async JavaScript code in the browser. The browser actually uses separate web APIs to run async code and when that async code is ready to run (such as when a response from an API is received), that code is actually put in a callback queue which is not the call stack. So our synchronous code is put on the call stack (LIFO) while our asynchronous code is queued up in the callback queue (FIFO). Meanwhile, an event loop determines whether to run code from the call stack or the callback queue. You don't need to have a deep understanding of this to work with JavaScript. 



## Writing our Own Stack and Queue Functions
## Exercise

Next, let's do a little exercise. See if you can write your own stack() and queue() classes which add and remove elements as needed. Here are some hints - and don't overthink it. It's actually surprisingly simple to do.

    Remember that both stacks and queues are data structures.
    What common data structure will allow us to easily add and remove elements?
    Stacks are last in, first out, which means we need to add things to the end of the data structure and remove things from the end of the data structure.
    Queues are first in, first out, which means we need to add things to the end of the data structure and then remove things from the beginning of the data structure.

Take a few minutes to write out these classes and then check out the solution below.
Solution

The hints hopefully made it fairly clear that we can use arrays to easily model both queues and stacks. array.prototype.push() can be used to add elements to the end of either a stack or a queue. Meanwhile, we can use array.prototype.pop() to remove elements from the end of a stack and we can use array.prototype.shift() to remove elements from the beginning of a queue.

Here's our simple implementation of a stack:

class Stack {
  constructor() {
    this.elements = [];
  }

  push(element) {
    return this.elements.push(element);
  }

  pop() {
    return this.elements.pop();
  }
}

Yep, that's it. That is really all a stack is doing - pushing and popping. Note that we can also use array.prototype.shift() and array.prototype.unshift() as well. This feels a bit closer to how a stack actually works since it adds an element to the beginning of the collection and then removes it from the beginning of the collection. The one downside to this approach is that it's a bit slower - that's because JavaScript needs to re-index the array each time an element is added or removed from the beginning of the stack.

Ultimately, either approach is fine - the most important thing is that whichever element was added to the stack most recently must also be the one that gets removed most recently - that's last-in, first-out (LIFO).

Here's our implementation of a queue - which will look very similar:

class Queue {
  constructor() {
    this.elements = [];
  }

  enqueue(element) {
    return this.elements.push(element);
  }

  dequeue() {
    return this.elements.shift();
  }
}

There are a few differences here. First, we call our methods queue.prototype.enqueue() and queue.prototype.dequeue(). That's because when we add an item to a queue, it's known as enqueuing while removing an item from a queue is known as dequeuing.

Actual queues and stacks generally have other methods as well - and they are more complex than these very simple implementations. However, on a basic conceptual level, the code examples above encapsulate what we need to know about stacks and queues.

We'll get more practice with stacks and queues when we write certain algorithms. For instance, it's common to use a queue with a breadth-first search algorithm and a stack with a depth-first search algorithm.



## Depth and Breadth Search Algorithms

There are two ways we can search a tree - regardless of whether that's a binary search tree or a more general tree. We can take a depth-first search (DFS) approach or a breadth-first search (BFS) approach. If we are looking at a tree that has a root node and child nodes, a depth-first search algorithm will search a tree vertically while a breadth-first search algorithm will search a tree horizontally. A graph can't really be measured vertically or horizontally in this way but the same concept still applies - we can search broadly or go deeper into each branch first. We just don't necessarily do so from a root node. Instead, we can start our search from any node in the graph.
Depth-First Search

In a depth-first search, our algorithm will completely traverse the left-most branch. Once that is complete, it will backtrack and then completely traverse the next branch. It will continue doing this until it finds what it's searching for - or, if the value doesn't exist, until the tree is completely traversed.

But how does the algorithm actually backtrack? Well, a DFS algorithm commonly uses a stack (last-in, first-out) to search through each branch to the very end. 

Let's walk through this process for our perfect binary search tree - and in doing so, we'll see how using a stack allows our algorithm to traverse and automatically backtrack through a tree's branches.

In the perfect binary search tree above, we start with the root node. Here's our stack so far:

let stack = [4]

There's only one element, so we can't really demonstrate its LIFO yet. We get the children of 4 (2 and 6) and remove the 4 from the stack. Now our stack looks like this:

stack = [2, 6];

Remember, we want to search the left-most branch first. So now we grab 2 from the beginning of the stack and add this node's children to the beginning of the stack: 1 and 3. Here's the stack now:

stack = [1, 3, 6];

Once again, to continue down the left-most branch, we just need to grab the first value. We take the 1 from the beginning of the stack. It has no children, so we are done with that branch. So here's our stack now:

stack = [3, 6];

Look at that - the first element in our stack is the next branch! We see that the 3 has no children of its own - so now our stack is just this:

stack = [6];

We've completely evaluated the left subtree and now only the right subtree remains. We take 6 from the stack, evaluate its children, and so on.

Now that we've looked at a basic example of how a DFS uses a stack, let's apply a more complex example. This time we'll use a graph. After all, we are in the middle of learning about graph theory.

Here's a graph of friends. We will actually be using this very graph when we actually write our BFS and DFS algorithms over the next several lessons. Let's say we want to find a connection between Jasmine and Thomas. How would we do that with a depth-first search?

First, we'll get an array of Jasmine's friends. This is our initial stack:

let stack = ["Ada", "Lydia", "Rose"];

Now we want to navigate down through the first branch on the list. There's really no working from left-to-right here - nor can we really say we are searching the graph horizontally or vertically like we would with a binary search tree. Instead, we'd just grab the list of Jasmine's friends from an adjacency list or similar structure. Because this is a stack, we are starting with the first friend in the array. We remove Ada from the stack and get her list of friends.

Ada is connected to Dylan and Lydia. Let's add them to the top of the stack. Now our stack looks like this:

stack = ["Dylan", "Lydia", "Lydia", "Rose"];

Ada has been removed from the top of the stack while her friends have been added. Because this is a stack, we need to continue with the last-in, first-out approach. But now hopefully it's clear why we need to do that. What gets added to the top of the stack is any remaining friends in the branch we are currently traversing - as well as friends in sub-branches we will be traversing soon. Meanwhile, what's at the bottom of the stack? A completely different branch. So everything we add right now is what we need to evaluate right now - in other words, last-in, first-out.

So now we take Dylan from the top of the stack and grab his friends. There is just Allison, which we add to the top of the stack.

stack = ["Allison", "Lydia", "Lydia", "Rose"];

When we take Allison from the top of the stack, we'll see that she only has Dylan as a friend. However, we have already traversed the node that represents Dylan - and we don't want to traverse it again. To avoid that, we flag nodes as we traverse them. This is a key difference between how we'd traverse this graph versus how we'd traverse a binary search tree. Our binary search tree is directed - information can only flow from parents to children - and children know nothing about their parents. There is no risk of accidentally going back to a node that has already been traversed. However, our graph is undirected - so we need to make sure our algorithm doesn't accidentally traverse nodes that have already been covered.

So at this point, because we've traversed through nodes representing Jasmine, Ada and Dylan already, we wouldn't add them back to the stack. So when Allison is removed from the top of the stack, no one else is added.

So now our stack looks like this:

stack = ["Lydia", "Lydia", "Rose"];

We are moving onto our next branch! Once we reach the end of a branch, there are no more nodes to traverse. The last item in the stack will now be the most recent sub-branch we haven't explored yet. In this case, it is Lydia. But note that we are exploring Lydia's connection to Ada, not Lydia's connection to Jasmine here.

We remove that from the top of the stack and find Thomas. We've successfully determined the reachability between Thomas and Jasmine. So at this point, if we've been tracking our traversal, we can see that there's a connection going like this: Jasmine -> Ada -> Lydia -> Thomas. So we could say that Thomas is a friend of a friend of a friend.

But if we actually look at our graph, we can quickly see that there is a closer connection. After all, we normally call them friends of friends if we can - not friends of friends of friends. If we wanted to determine not just the reachability of a node but the shortest path, our depth-first search would continue.

By the way, here's a very important reason we need to flag nodes as we traverse them. Jasmine is Lydia's friend. If we didn't flag that we'd already traversed through that node, our algorithm could check Jasmine's friends, find Ada, and then check Ada's friends, find Lydia, and then check Lydia's friends, only to find Jasmine, forever and ever in an infinite loop of friends that sounds wonderful in practice but not in programming.

So the key takeaways here, at least from a programming perspective, is that we need to use a stack to walk through the graph with a depth-first search, and also that we need to flag nodes we've already traversed so we don't run into infinite loops or other inefficiencies.

If this is still confusing, don't worry. It should become clearer when we use TDD to write a depth-first search algorithm in the next lesson.
Advantages

    DFSs are great for building games because they allow the algorithm to explore winning and losing possibilities.
    If we know that the answer we are looking for will be at the bottom of a tree, a DFS will usually be faster than a BFS. For instance, we might be looking for living descendants of a long family tree - and everyone alive will be at the bottom of the tree.
    DFS usually takes less memory than BFS.


## Breadth-First Search

On the other hand, a breadth-first search algorithm searches horizontally. The image below demonstrates this:

As we can see, the algorithm will search each row in the tree completely before moving onto the next one.

In contrast to a depth-first search, a breadth-first search uses a queue instead of a stack. That means that when we add new nodes to the collection, they are added at the end. Meanwhile, we evaluate the node at the beginning of the collection first.

Once again, let's demonstrate how this works first on the perfect binary search tree in the diagram above and then on our graph of friends.

Our queue starts like this:

let queue = [4];

We grab 4 and then add 2 and 6 to the queue.

queue = [2, 6];

2 is first in line, so let's add its children: 1 and 3. Here's the big difference between the stack we used before and the queue we are using now. We add 1 and 3 to the end of the queue - not to the beginning (or "top") of the stack.

So now our queue looks like this:

queue = [6, 1, 3];

We take 6 from the front and add its children (5 and 7) to the queue:

queue = [1, 3, 5, 7];

As we can see, we've finished an entire "row" of the tree - and all that's left to traverse is the bottom row.

In this case, we want to check all of Jasmine's friends before we check her friends' friends.

First, we add all of her friends to an array:

let queue = ["Ada", "Lydia", "Rose"];

The first friend in the queue is Ada. We remove Ada from the queue and get her friends: Dylan and Lydia. We add them to the end of the queue:

queue = ["Lydia", "Rose", "Dylan", "Lydia"];

As we can see, the first few names still on the list are the ones that are Jasmine's friends - we save the names we added (friends of friends) to the end of the list. Remember, nodes added to the end of the list always represent going deeper into the tree. If we want to go depth-first, we add elements to the beginning of the collection. If we want to go breadth-first, we add new elements to the end of the collection.
Advantages

    If we know that an answer will be close to the root node, a BFS is more efficient. For instance, in the family tree example, we might be looking for a direct descendant that lived 50 years later than the ancestor at the root node, not 300 years later.
    BFSs are great for computing the most efficient path between two nodes. For that reason, they are great for GPS and mapping applications and even for finding people near each other in social networks.

##  Depth First Algorithms

In the previous lesson, we covered depth-first search and breadth-first search approaches. There are a lot of benefits of using these algorithms. We could determine the reachability between two nodes or we could determine the shortest path between two nodes, just to name a few use cases.

We'll start with the depth-first algorithm because it is a bit easier to implement. To actually use a TDD approach and test our algorithms in a graph, we are going to need to code that graph first. We will add this graph to our test file.

To keep things separated out, we are going to create a new test file called dfs.test.js and run all tests related to our depth-first search there. Here is the starter code which includes a graph.

__tests__/dfs.test.js

import Graph from '../src/graph.js';

describe('depth-first search', () => {

  let graph = new Graph();
  graph.addNode("Jasmine");
  graph.addNode("Ada");
  graph.addNode("Lydia");
  graph.addNode("Rose");
  graph.addNode("Dylan");
  graph.addNode("Allison");
  graph.addNode("Thomas");
  graph.addNode("Sarah");
  graph.createEdge("Jasmine", "Ada");
  graph.createEdge("Jasmine", "Lydia");
  graph.createEdge("Jasmine", "Rose");
  graph.createEdge("Ada", "Dylan");
  graph.createEdge("Lydia", "Ada");
  graph.createEdge("Dylan", "Allison");
  graph.createEdge("Lydia", "Thomas");
});

Note that we don't put the graph in a beforeEach() or afterEach() block. We aren't actually going to modify this graph, just traverse it, so there's no need to re-instantiate it before or after every single test.

Next, let's include a visual representation of the coded graph above. It's the same one we were using in the last lesson.

There is one small but key difference about our graph that is not included in our illustration. We've also added Sarah, a node that isn't connected to anyone in the graph. We'll need this for one of our tests - which we'll cover in a moment.

Our DFS will just check to see if a node is reachable from another node. In this case, we just want to see if we can reach Thomas via Jasmine. While it's obvious from our graph that all its nodes are reachable from each other (except for Sarah), in a real-world application, that might not be the case.

Let's write our first test. We'll call our method graph.prototype.depthFirstReachable() and it will return a boolean. The reason for the wordy name is because we'll write a method called graph.prototype.breadthFirstReachable() in the next lesson that does the exact same thing as this method - but with a different algorithm.

The simplest implementation of our graph.prototype.depthFirstReachable() method is to return false if a potential node doesn't even exist in the graph. For the sake of brevity, we'll code two tests right now: one to check the starting node, one to check the target node. Here are the tests:

__tests__/dfs.test.js

...
 test('should return false if the target node does not exist', () => {
    expect(graph.depthFirstReachable("Jasmine", "Albert")).toEqual(false);
  });

  test('should return false if the starting node does not exist', () => {
    expect(graph.depthFirstReachable("Albert", "Thomas")).toEqual(false);
  });
...

Now let's get these tests passing:

src/graph.js

...
  depthFirstReachable(startingNode, targetNode) {
    if ((!this.adjacencyList.has(startingNode)) || (!this.adjacencyList.has(targetNode))) {
      return false;
    }
  }
...

Our method will take two arguments: the first, a startingNode, is the node we want to start from. The second is the targetNode, which is the node we want to find. In an undirected graph, these arguments could be interchangeable. In a directed graph, on the other hand, the order would matter.

Our method just checks to see if both nodes exist in the adjacency list. If one or the other doesn't exist, they obviously aren't reachable. Why go to the trouble of doing a DFS, which could be computationally intensive with a big graph, when values don't even exist? Because our adjacency list is a Map, it is super-fast to do this lookup. Meanwhile, doing a DFS to search the whole graph - only to tell us the same thing - would be very inefficient.

Next, let's get to the fun stuff. We will check to see if a direct friend of Jasmine is reachable.

Here's the test:

__tests__/dfs.test.js

...
  test('should check if the first friend in the adjacency list is reachable', () => {
    expect(graph.depthFirstReachable("Jasmine", "Ada")).toEqual(true);
  });
...

Now the code to get the test passing:

src/graph.js

depthFirstReachable(startingNode, targetNode) {
  if ((!this.adjacencyList.has(startingNode)) || (!this.adjacencyList.has(targetNode))) {
    return false;
  }
  let stack = [startingNode];
  let traversedNodes = new Set();
  while (stack.length) {
    const currentNode = stack.shift();
    if (currentNode === targetNode) {
      return true;
    } else {
      traversedNodes.add(currentNode);
      const adjacencyList = this.adjacencyList.get(currentNode);
      adjacencyList.forEach(function(node) {
        if (!traversedNodes.has(node)) {
          stack.unshift(node);
        }
      });
    }
  }
}

Let's focus on just the new code - which is our actual DFS algorithm:

src/graph.js

...
let stack = [startingNode];
let traversedNodes = new Set();
while (stack.length) {
  const currentNode = stack.shift();
  if (currentNode === targetNode) {
    return true;
  } else {
    traversedNodes.add(currentNode);
    const adjacencyList = this.adjacencyList.get(currentNode);
    adjacencyList.forEach(function(node) {
      if (!traversedNodes.has(node)) {
        stack.unshift(node);
      }
    });
  }
}
...

First, we need to create our stack:

let stack = [startingNode];

It's an array with the startingNode as its only element.

Next, we also need some way to flag the nodes that have been traversed. We don't want this to be a property on the node itself because this flag is temporary - we don't want it to persist once the method is finished running.

let traversedNodes = new Set();

This is a perfect use case for a Set. We don't want there to be duplicate values - we just want to know whether a node has been traversed or not. And looking up whether a value in a Set is super-fast - so it won't take too much time. Remember, hash table lookup is O(1) constant time - and a Set uses a hash table under the hood.

At this point, we have collections to store a stack as well as a set of traversed nodes. We are ready to start looping!

In general, when looping through a stack or a queue that will eventually be empty, we can just iterate until the collection's length is zero. At that point, we know we've checked every value in the stack or queue.

That's exactly what we do here:

while (stack.length)

This just states that we should loop through the stack until it is empty. Once the length of the stack becomes zero, stack.length is false - and the loop will end.

Next, we need to take the first element from the stack:

const currentNode = stack.shift();

array.prototype.shift() is a destructive method - that means that we aren't just grabbing the value of the first element in our stack - we are also removing it from the array. This is the currentNode that we are examining. It is a const because the variable itself never changes - it falls out of scope each time through the loop and is created anew.

Next, we have a conditional that checks to see if the currentNode === targetNode.

if (currentNode === targetNode) {
  return true;
}

If they are equal, we've found the target node! The target node is reachable from the starting node - and we can return true.

If it isn't, we have more work to do. Here's the code in our else statement:

...
  else {
    traversedNodes.add(currentNode);
    const adjacencyList = this.adjacencyList.get(currentNode);
    adjacencyList.forEach(function(node) {
      if (!traversedNodes.has(node)) {
        stack.unshift(node);
      }
    });
  }
...

First, we need to "flag" our node to show that it's been traversed:

traversedNodes.add(currentNode);

Here, we use the set.prototype.add() method to add the currentNode to traversedNodes. This way, when we need to look up whether the node has been traversed or not, we'll see that the currentNode has been added. The first time through the loop, this will be the startingNode - also known as Jasmine.

Next, we need to find the adjacency list for the current node. In Jasmine's case, the list is a Set that includes Ada, Lydia, and Rose.

Once we've gotten the adjacency list, we are ready to add it to the stack. To do that, we iterate through the adjacency list, first checking each node in the adjacency list to see if it's in the traversed array - and then adding that node to the top of the stack with array.prototype.unshift(), which adds an element to the beginning of an array. This will loop through the entire adjacency list, updating our stack. After the loop is done, our stack will look like this:

["Rose", "Lydia", "Ada"]

Because stack.length has three elements, it's truthy so the loop continues. Due to the following line const currentNode = stack.shift();, Rose is taken off the top of the stack and becomes the currentNode. Then the entire process continues - Rose is added to the list of traversedNodes and then her adjacency list is added to the stack and so on. Eventually, our DFS will find Ada and recognize that it's equal to the targetNode, meaning the method will return true.

If we update our test to look for Thomas instead of Ada, it will pass. As we can see, in order to find Ada, we had to write an algorithm that would also find Thomas - or anyone else in the graph.

There is one more thing we need to do. What about a node that exists but isn't reachable? That's why we added a node for Sarah. This node isn't connected to any other node in the graph - but it's still part of the graph, which means our initial conditional won't return false.

Here's the test:

__tests__/dfs.test.js

...
  test('should return false if the target node can not be reached from the starting node', () => {
    expect(graph.depthFirstReachable("Jasmine", "Sarah")).toEqual(false);
  });
...

Fortunately, getting this test passing is easy. At some point, stack.length is going to be false (because it's zero). At that point, we know that we've traversed every node in the graph that is reachable from the starting node. So if we've traversed the entire graph and we still haven't found the node we are looking for, we know it's not reachable. Here's the updated code:

src/graph.js

depthFirstReachable(startingNode, targetNode) {
  if ((!this.adjacencyList.has(startingNode)) || (!this.adjacencyList.has(targetNode))) {
    return false;
  }
  let stack = [startingNode];
  let traversedNodes = new Set();
  while (stack.length) {
    const currentNode = stack.shift();
    if (currentNode === targetNode) {
      return true;
    } else {
      traversedNodes.add(currentNode);
      const adjacencyList = this.adjacencyList.get(currentNode);
      adjacencyList.forEach(function(node) {
        if (!traversedNodes.has(node)) {
          stack.unshift(node);
        }
      });
    }
  }
  return false;
}

This is our entire method. All we added is the return false; outside the while loop. As we can see here, we have a nested loop. The Big O of this particular algorithm is something akin to O(AB) - though that's not quite accurate because the B (the adjacency list for each node) is always changing - A (the stack itself) is always changing, too. It should be clear why that initial condition is even more helpful now. Searching the entire graph to determine that a node isn't reachable is a big task - best to check if one of the nodes doesn't exist in the first place.

At this point, if you are feeling any confusion about how this algorithm works (and it's very understandable if you are), the next step is to intentionally break one of the tests and get into Jest

Links to an external site.'s debug mode so you can step through the method and watch how variables change. The GIF below shows this - all the relevant variables have been added to Watch in the left-hand pane. This loop runs until the algorithm discovers that the Thomas node is, in fact, reachable from the Jasmine node.

It can also be useful to look at what would happen if we didn't flag traversed nodes. In other words, if our code looked like this instead:

// This will not work! It will lead to infinite loops because we aren't flagging traversed nodes.

depthFirstReachable(startingNode, targetNode) {
    if ((!this.adjacencyList.has(startingNode)) || (!this.adjacencyList.has(targetNode))) {
      return false;
    }
    let stack = [startingNode];
    while (stack.length) {
      const currentNode = stack.shift();
      if (currentNode === targetNode) {
        return true;
      } else {
        const adjacencyList = this.adjacencyList.get(currentNode);
        adjacencyList.forEach(function(node) {
          stack.unshift(node);
        });
      }
    }
    return false;
  }

Take a look at what happens when we do this:

We bounce back and forth between Jasmine and Rose because Rose is a friend of Jasmine and Jasmine is a friend of Rose back and forth forever and ever. We don't want this in our code.

So that is a depth-first search algorithm in a nutshell. We use a stack to search through each branch. In this lesson, we just focused on finding whether a node is reachable. You will get a chance to practice some other implementations soon as well.

## Breadth First Algorithms

In the last lesson, we used a depth-first algorithm to determine if a target node in our friendship network is reachable from a starting node. In this lesson, we'll test and write a Graph.prototype.breadthFirstReachable() method. This method will do the exact same thing as our Graph.prototype.depthFirstReachable() method. The only difference is how the method will work. Instead of using a depth-first search, the method will use a breadth-first search.

This will feel a little bit different than our usual TDD process because we will use the exact same tests that we used in the last lesson. In other words, our tests are already written for us! Go ahead and create a bfs.test.js file in __tests__ with the exact same tests that are in the dfs.test.js file. Then update the name of the Graph.prototype.depthFirstReachable() method to Graph.prototype.breadthFirstReachable(). Our goal here is just to verify that everything in our method works correctly as we won't get tests passing one at a time. That's because the method itself is extremely similar to the one we wrote in the last lesson.

The only difference is that we'll use a queue instead of a stack. That means each time we add new nodes to traverse, they will go to the end of the queue instead of the top of the stack. We will still evaluate the first element in the collection - but because it's a queue (and new items are being pushed to the end of the queue), this will be first-in, first-out (FIFO) instead of last-in, first-out (LIFO).

Here's the full updated method:

src/bfs.js

breadthFirstReachable(startingNode, targetNode) {
    if ((!this.adjacencyList.has(startingNode)) || (!this.adjacencyList.has(targetNode))) {
      return false;
    }
    let queue = [startingNode];
    let traversedNodes = new Set();
    while (queue.length) {
      const currentNode = queue.shift();
      if (currentNode === targetNode) {
        return true;
      } else {
        traversedNodes.add(currentNode);
        const adjacencyList = this.adjacencyList.get(currentNode);
        adjacencyList.forEach(function(node) {
          if (!traversedNodes.has(node)) {
            queue.push(node);
          }
        });
      }
    }
    return false;
  }

The first three lines in the method look exactly the same as our previous method:

if ((!this.adjacencyList.has(startingNode)) || (!this.adjacencyList.has(targetNode))) {
  return false;
}

This just ensures that both the startingNode and the targetNode exist in the graph and will get our first two tests passing. Once again, we don't want the overhead of doing a search algorithm if we can quickly determine whether a node exists in the list or not (which has O(1) time).

Now let's take a look at the very subtle difference between DFS and BFS:

src/bfs.js

...
  let queue = [startingNode];
  let traversedNodes = new Set();
  while (queue.length) {
    const currentNode = queue.shift();
    if (currentNode === targetNode) {
      return true;
    } else {
      traversedNodes.add(currentNode);
      const adjacencyList = this.adjacencyList.get(currentNode);
      adjacencyList.forEach(function(node) {
        if (!traversedNodes.has(node)) {
          queue.push(node);
        }
      });
    }
  }
  return false;
}
...

Other than the fact that we changed the name of our collection from stack to queue, there is only one small change in the method. Can you find it?

The only difference is inside our inner loop when we do the following:

queue.push(node);

Meanwhile, in the last lesson, we did the following instead:

stack.unshift(node);

In both algorithms, we always traverse the first element in the collection each iteration through the loop. The difference is that when we add new elements to the queue, they go to the end of the collection, which means they will be traversed last (a breadth-first search). Meanwhile, with the stack, they go to the beginning of the collection, which means they will be traversed first (a depth-first search).

That is really the only difference between a DFS and a BFS. Despite that fact, the order at which items are traversed can make a huge impact on the overall efficiency of an algorithm. For instance, if we knew there was a high probability that a node is a friend or a friend of a friend, we probably won't want to do a DFS. Instead, we could just use a BFS to first check all friends and then check all friends of friends.

Now that we've learned the basics of DFS and BFS, we're well on our way to having a basic understanding of graphs and how we can traverse them. We also have some new problem-solving tools to work with. In addition to our knowledge of two new algorithms (DFS and BFS), we can also see how we can use queues and stacks, two different data structures, to solve different kinds of problems. Whether that is using JavaScript's event loop or writing an algorithm, these data structures are absolutely essential.
Practice

Now that we've learned the basics of graph theory, it's time to practice! First, walk through all the examples in the graph theory lessons and code along if you haven't already. Once you are done, you are ready for some additional challenges.

    Create an application that shows connections in a directed graph. To do this, create an application with ten nodes which are all locations. They could be locations for your hometown, for a mythical realm, or for an imaginary city. First, use draw.io 

Links to an external site. to draw your graph. Add nodes that are both one-way and two-way. (There are both one-way and two-way arrows in draw.io
Links to an external site..) Make sure that some nodes are unreachable to others.
Once you have completed your diagram, you will need to make some updates to your code. For instance, the Graph.prototype.addNode() method we wrote is undirected. You can update this method or add additional methods such as Graph.prototype.addDirectedEdge() and Graph.prototype.addUndirectedEdge(). It's up to you to determine how you want to implement a directed graph application. Just make sure to use TDD.
Add a counter to determine how many steps it takes to traverse the entire graph using both BFS and DFS.
More challenging: Try writing recursive versions of your DFS and BFS algorithms.
Difficult: Write a Graph.prototype.shortestPath() method to determine the shortest path between one node and another. The method should return the path (the nodes traversed) to get from the starting node to the targeted node.
Make the edges weighted. Each edge should now have a value associated with it that represents the distance between the two nodes. To do this, create an Edge class. Each instantiated edge should have a distance property. When calculating the shortest path, your Graph.prototype.shortestPath() method should take into account these distances.
Finally, create a Graph.prototype.allPaths() method that returns all paths - similar to what we might see with an application like Google Maps. Once again, this should return the paths between the starting node and the target node. The difference is that all the potential paths can be returned. Sort these paths from shortest to longest.