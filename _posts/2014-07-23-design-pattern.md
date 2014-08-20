---
layout: post
title:  "Design Pattern 总结"
date:   2014-07-23 00:30:29
categories: design pattern
---

##Design Pattern 总结

### Singleton

[WIKI][SINGLETON]

 a design pattern that restricts the instantiation of a class to one object. This is useful when exactly one object is needed to coordinate actions across the system


Eager initialization

~~~
public class Singleton {
    private static final Singleton INSTANCE = new Singleton();
 
    private Singleton() {}
 
    public static Singleton getInstance() {
        return INSTANCE;
    }
}
~~~

Lazy initialization

This method uses double-checked locking, which should not be used prior to J2SE 5.0, as it is vulnerable to subtle bugs. The problem is that an out-of-order write may allow the instance reference to be returned before the Singleton constructor is executed.

~~~~~
public class SingletonDemo {
    private static volatile SingletonDemo instance = null;
    private SingletonDemo() { }
    public static SingletonDemo getInstance() {
        if (instance == null) {
            synchronized (SingletonDemo.class) {
                if (instance == null) {
                    instance = new SingletonDemo();
                }
            }
        }
        return instance;
    }
}
~~~~~

Initialization On Demand Holder Idiom

~~~
public class Singleton {
	// Private constructor prevents instantiation from other classes
	private Singleton() { }
 
	/**
	* SingletonHolder is loaded on the first execution of Singleton.getInstance() 
	* or the first access to SingletonHolder.INSTANCE, not before.
	*/
	private static class SingletonHolder { 
		private static final Singleton INSTANCE = new Singleton();
	}
 
	public static Singleton getInstance() {
		return SingletonHolder.INSTANCE;
	}
}
~~~

The Enum way

In the second edition of his book Effective Java, Joshua Bloch claims that "a single-element enum type is the best way to implement a singleton" for any Java that supports enums. The use of an enum is very easy to implement and has no drawbacks regarding serializable objects, which have to be circumvented in the other ways.

~~~
public enum Singleton {
    INSTANCE;
    public void execute (String arg) {
        // Perform operation here 
    }
}
~~~

### Dependency Injection
[WIKI][DEPENDENCY]

a software design pattern that implements inversion of control and allows a program design to follow the dependency inversion principle. 

### Decorator Pattern
a design pattern that allows behavior to be added to an individual object, either statically or dynamically, without affecting the behavior of other objects from the same class

First example (window/scrolling scenario)[edit]
The following Java example illustrates the use of decorators using the window/scrolling scenario.

~~~
// The Window interface class
public interface Window {
    public void draw(); // Draws the Window
    public String getDescription(); // Returns a description of the Window
}
 
// Extension of a simple Window without any scrollbars
class SimpleWindow implements Window {
    public void draw() {
        // Draw window
    }
 
    public String getDescription() {
        return "simple window";
    }
}
~~~

The following classes contain the decorators for all Window classes, including the decorator classes themselves.

~~~
// abstract decorator class - note that it implements Window
abstract class WindowDecorator implements Window {
    protected Window windowToBeDecorated; // the Window being decorated
 
    public WindowDecorator (Window windowToBeDecorated) {
        this.windowToBeDecorated = windowToBeDecorated;
    }
    public void draw() {
        windowToBeDecorated.draw(); //Delegation
    }
    public String getDescription() {
        return windowToBeDecorated.getDescription(); //Delegation
    }
}
 
// The first concrete decorator which adds vertical scrollbar functionality
class VerticalScrollBarDecorator extends WindowDecorator {
    public VerticalScrollBarDecorator (Window windowToBeDecorated) {
        super(windowToBeDecorated);
    }
 
    @Override
    public void draw() {
        super.draw();
        drawVerticalScrollBar();
    }
 
    private void drawVerticalScrollBar() {
        // Draw the vertical scrollbar
    }
 
    @Override
    public String getDescription() {
        return super.getDescription() + ", including vertical scrollbars";
    }
}
 
// The second concrete decorator which adds horizontal scrollbar functionality
class HorizontalScrollBarDecorator extends WindowDecorator {
    public HorizontalScrollBarDecorator (Window windowToBeDecorated) {
        super(windowToBeDecorated);
    }
 
    @Override
    public void draw() {
        super.draw();
        drawHorizontalScrollBar();
    }
 
    private void drawHorizontalScrollBar() {
        // Draw the horizontal scrollbar
    }
 
    @Override
    public String getDescription() {
        return super.getDescription() + ", including horizontal scrollbars";
    }
}
~~~

Here's a test program that creates a Window instance which is fully decorated (i.e., with vertical and horizontal scrollbars), and prints its description:

~~~
public class DecoratedWindowTest {
    public static void main(String[] args) {
        // Create a decorated Window with horizontal and vertical scrollbars
        Window decoratedWindow = new HorizontalScrollBarDecorator (
                new VerticalScrollBarDecorator (new SimpleWindow()));
 
        // Print the Window's description
        System.out.println(decoratedWindow.getDescription());
    }
}
~~~

### Iterator

a design pattern in which an iterator is used to traverse a container and access the container's elements. The iterator pattern decouples algorithms from containers; in some cases, algorithms are necessarily container-specific and thus cannot be decoupled

Example 1: Tree iterator

~~~
/**
 * An iterator that iterates through a tree using in-order tree traversal
 * allowing a sorted sequence.
 *
 */
public class Iterator {

    private Stack<Node> stack = new Stack<>();
    private Node current;

    private Iterator(Node argRoot) {
        current = argRoot;
    }

    public Node next() {
        while (current != null) {
            stack.push(current);
            current = current.left;
        }


        current = stack.pop();
        Node node = current;
        current = current.right;

        return node;
    }

    public boolean hasNext() {
        return (!stack.isEmpty() || current != null);
    }

    public static Iterator iterator(Node root) {
        return new Iterator(root);
    }
}
~~~

Example 2: Merge iterator

~~~
public interface Iterator {
  /**
   * @return True if the there is a next element
   */
  boolean hasNext();
  /**
   * @return next Highest number and removes the items from the stream.
   */
  int getNextHighestNumber();
 }

class MergedIterator {
    List<Iterator> _listIters;
    PriorityQueue<Pair<?,?>> maxHeap;
  /**
   * Construct MergedIterator.
   * @param iterators list of iterator
   */
  MergedIterator(List<Iterator> iterators) {
      if (iterators == null || iterators.size() <= 0) throw new Exception("");
     _listIters = iterators;
     maxHeap = new PriorityQueue<Pair<Int, Iterator>, >(new MyCompartor());
     for (Iterator iter: _listIters) {
          if (iter.hasNext())
              maxHeap.insert(new Pair(iter.getNextHighestNumber(), iter));
      }
  }

  /**
   * @return true if there is a next element.
   */
  boolean hasNext() {
    return maxHeap.size() > 0;
 }

  /**
   * @return next max element across all the iterators.
   */
  int getNextHighestNumber() {
      if (!hasNext()) throw new Exception("Empty");
      Pair pair = maxHeap.pop();
      int num = pair.getFirst();
      Iterator iter = pair.getSecond();
      if (iter.hasNext())
        maxHeap.insert(new Pair(iter.getNextHighestNumber(), iter));
      return num;

  }

}
~~~

Example 3: Deep Iterator

~~~
class DeepIterator implements Iterator<Object> {
    Stack<Iterator<Object>> itrs;
    Object curValue;
    public DeepIterator(ArrayList<Object> inputList) {
        itrs = new Stack<Iterator<Object>>();
        itrs.push(inputList.iterator());
    }
    
    @Override
    public boolean hasNext() {
       if (itrs.isEmpty()) {
           return false;
       }
       Iterator<Object> curIter = itrs.peek();
       while (curIter.hasNext()) {
           Object nextObj = curIter.next();
           if (obj instanceof ArrayList) {
               if ((ArrayList(obj)).size() == 0) {
                   continue;
               } else {
                   itrs.push((ArrayList(obj)).iterator());
                   return hasNext();
               }
           } else {
               curValue = (Object)nextObj;
               return true;
           }
           
       }
       itrs.pop();
       return hasNext();
    }

    @Override
    public Object next() {
        if (hasNext()) {
            return curValue;
        } else {
            return null;
        }
        
  
    }
}
~~~

### Scalable System Design Pattern

http://horicky.blogspot.com/2010/10/scalable-system-design-patterns.html

http://horicky.blogspot.com/2008/02/scalable-system-design.html

[SINGLETON]: http://en.wikipedia.org/wiki/Singleton_pattern
[DEPENDENCY]: http://en.wikipedia.org/wiki/Dependency_injection
