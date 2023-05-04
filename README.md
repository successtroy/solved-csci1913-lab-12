Download Link: https://assignmentchef.com/product/solved-csci1913-lab-12
<br>
We claim to believe that everyone has equal rank. In other cultures, however, some people are thought to have higher rank than others. For example, the following system of ranks is similar to that used in Nineteenth Century Victorian England—and in some modern fantasy and romance novels.

<table width="204">

 <tbody>

  <tr>

   <td width="51"><strong>  R</strong><strong>ANK  </strong></td>

   <td width="153"><strong>D</strong><strong>ESCRIPTION</strong></td>

  </tr>

  <tr>

   <td width="51">0</td>

   <td width="153">  King, Queen</td>

  </tr>

  <tr>

   <td width="51">1</td>

   <td width="153">  Prince, Princess</td>

  </tr>

  <tr>

   <td width="51">2</td>

   <td width="153">  Duke, Duchess</td>

  </tr>

  <tr>

   <td width="51">3</td>

   <td width="153">  Marquess, Marchioness</td>

  </tr>

  <tr>

   <td width="51">4</td>

   <td width="153">  Count, Countess</td>

  </tr>

  <tr>

   <td width="51">5</td>

   <td width="153">  Knight, Dame</td>

  </tr>

  <tr>

   <td width="51">6</td>

   <td width="153">  Gentleman, Gentlewoman</td>

  </tr>

  <tr>

   <td width="51">7</td>

   <td width="153">  Commoner</td>

  </tr>

 </tbody>

</table>

We can denote each rank by a nonnegative integer, with lower integers corresponding to higher ranks. For example, a King has higher rank that a Prince, because 0 &lt; 1. Similarly, a Prince has higher rank than a Count, because 1 &lt; 4. And everyone has higher rank than a Commoner, except another Commoner.

In the lectures, we used a queue to model a line of people waiting to enter a movie theater. As people arrived at the theater, they joined the queue at the rear, and left the queue at the front. This makes sense where people don’t have ranks. If there are ranks, however, then things work differently: people arriving at the theater enter in order of rank. For example, if a Commoner and a Prince arrive at the theater, then the Prince would enter first. And if a King arrives, he would enter before either of them. Commoners would enter last of all.       We can model the situation just described by using a different data structure from an ordinary queue, called a <em>priority queue.</em> In a

priority queue, objects (perhaps representing people) arrive at the rear in the usual way. However, instead of leaving at the front, they leave in order of their ranks. Objects with higher ranks (and lower numbers) leave before objects with lower ranks (and higher numbers).       Priority queues have other uses besides enforcing antiquated dominance hierarchies. For example, a computer operating system might work by sharing the central processor among many different programs. If some programs are more important than others, then the operating system might use a priority queue to decide which program will run first.

<ol>

 <li><strong> Theory.</strong></li>

</ol>

We can implement something like a priority queue using a binary search tree (BST) that associates a set of <em>keys</em> with a set of <em>values.</em> The BST’s values are objects. Its keys are nonnegative integers that denote the ranks of those objects. Unlike the BST’s discussed in the lectures, the nodes in each left subtree have ranks <em>less than or equal</em> to the rank at the root. The nodes in each right subtree have ranks <em>greater than</em> the rank at the root. This allows two or more nodes to have the same rank.

When we enqueue a new object, we use the BST addition algorithm discussed in class. If there are <em>n</em> nodes in the tree, then the enqueueing algorithm can work in <em>O</em>(log <em>n</em>) time, assuming that the objects are added in random order of their ranks.

When we dequeue an object, we first find a node with the minimum key, again using an algorithm discussed in class. We then delete this node from the tree. This is easy to do, because it will always have at least one empty child, so we don’t need a complex deletion algorithm that can delete any node. We finally return the object at the removed node. The dequeueing algorithm can also work in <em>O</em>(log <em>n</em>) time.

The BST used to implement the priority queue must have a head node, to eliminate special cases when enqueueing and dequeueing. It should also use above and below pointers. BST’s with head nodes were discussed in the lectures, and code that uses them is available on Canvas—but it is not the same as what you will write here.

By the way, what we’ve just described isn’t really a priority queue! This is because objects of equal rank aren’t always deleted in the order they are added. To keep the assignment simple, we’ll ignore that problem: we don’t care about the order in which objects with equal rank are deleted. All we care about is that no object with low rank (like a Commoner) is deleted before an object of high rank (like a King).

<ol start="2">

 <li><strong> Implementation.</strong></li>

</ol>

Write a class called PriorityQueue that implements a priority queue using a BST. The priority queue must be able to hold arbitrary ranked objects of type Base, so it looks like this.

class PriorityQueue&lt;Base&gt;

{    private class Node

{

private Base object;      private int  rank;      private Node left;

private Node right;




private Node(Base object, int rank)

{

this.object = object;        this.rank = rank;        left = null;        right = null;

}

}




private Node root;  //  Root node of the BST.




⋮

}

The class PriorityQueue must have at least the following methods. You may need to write others.

public PriorityQueue()

Constructor. Make a new, empty priority queue. You must set root to a head node, to simplify insertion and deletion by the other methods. You must decide what the rank slot of the head node should be. Whatever you choose, it should minimize the number of special cases needed by the other methods. Don’t use special cases to add the first node, or to delete the last node!

public Base dequeue()

If the priority queue is empty, then throw an IllegalStateException. Otherwise, remove the highest ranked object (with the lowest rank number) from the priority queue, using the algorithm described in the previous section. Resolve ties in rank arbitrarily. Return the removed object.

public void enqueue(Base object, int rank)

If rank is negative, then throw an IllegalArgumentException. Otherwise, add object to the priority queue with the given rank, using the algorithm described in the previous section.

public boolean isEmpty()

Return true if the priority queue is empty. Return false otherwise.