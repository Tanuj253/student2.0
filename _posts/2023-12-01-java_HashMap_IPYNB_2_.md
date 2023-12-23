---
layout: post
title: Java, SQL | HashMap | Collection
description: Usage of HashMap, HashSet, Collection and combining definitions and usage with SQL
courses: {'csa': {'week': 15}}
type: ccc
---

## Lesson Ideas
Add to the basics of HasMap and HashSet below.  Go over storing elements in SQL like HashMap and Collection.  

- Include SQL elements JSONB with HashMap
- Include SQL ManyToMany relation with Collection

Review addition materials like Roles and Activity Tracker <https://nighthawkcoders.github.io/APCSA/1.b/2022/11/17/AP-frq2.html>

Here are some SQL definitions in Person.java.

```java
@ManyToMany(fetch = EAGER)
private Collection<PersonRole> roles = new ArrayList<>();

/* HashMap is used to store JSON for daily "stats"
"stats": {
    "2022-11-13": {
        "calories": 2200,
        "steps": 8000
    }
}
*/
@JdbcTypeCode(SqlTypes.JSON)
@Column(columnDefinition = "jsonb")
private Map<String,Map<String, Object>> stats = new HashMap<>(); 
```


## HashMap
> Below is an example using a HashMap and listed are some key things to consider when using this data structure.

1. Hashing: HashMap uses a hash function to map keys to their corresponding buckets. The hash function is used to compute the index of the array where the key-value pair should be stored. A good hash function should generate a unique hash code for each key, but collisions (i.e., two keys with the same hash code) can still occur.  Hash map in Java does not maintain insertion order either by key or by the order inserted.

2. Performance: HashMap provides constant-time performance (O(1)) for get() and put() operations, as long as the hash function is well-distributed and there are no hash collisions. However, in the worst case, the performance of a HashMap can degrade to O(n), where n is the number of elements in the map.

3. Key and value types: HashMap allows any non-null object as a key, and any object (including null) as a value. However, to use a class as a key, it must implement the equals() and hashCode() methods. HashMap uses the equals() method to check if two keys are equal, and the hashCode() method to generate the hash code for the key.

4. Iteration: HashMap provides several ways to iterate over the key-value pairs in the map, including using keySet(), values(), and entrySet(). The entrySet() method returns a Set view of the key-value pairs in the map, which can be used to iterate over the pairs and modify the map as you go.

5. Thread safety: HashMap is not thread-safe, which means that if multiple threads access the same HashMap instance concurrently and at least one thread modifies the map structurally, the behavior is undefined. To make a HashMap thread-safe, you can use the ConcurrentHashMap class instead, which provides concurrent access and is designed for high concurrency. In a Full Stack project it would be best to use a NoSQL database to avoid concurrency issues.


```Java

public class Pet  {
    // Instance data
	private final String name;
	private final int age;
	private final String color;

	/* constructor
	 *
	 */
	public Pet(String name, int age, String color)
	{
		this.name = name;
		this.age = age;
		this.color = color;
	}

	public String getName() { return this.name; }
	public int getAge() { return this.age; }
	public String getColor() { return this.color; }

}
```


```Java
import java.util.HashMap;

public class Pets {
    // create a new HashMap
    HashMap<String, Pet> names = new HashMap<>();

    /* Add Pets
     * 
     */
    public Pets() {
        // add some key-value pairs to the HashMap
        names.put("Leo", new Pet("Lion", 8, "Gold"));
        names.put("Porky", new Pet("Pig", 3, "Pink"));
        names.put("Ro-Ro", new Pet("Robin", 7, "Red"));
        names.put("Midnight", new Pet("Cat", 10, "Black"));
        names.put("Hobbes", new Pet("Kitty", 1, "Calico"));
        names.put("Duke", new Pet("Dog", 14, "Brown"));
    } 

    /* Remove Pet
     * 
     */
    public Pet remove(String key) {
        // check if a key exists in the HashMap then remove
        Pet pet = null;
        if (names.containsKey(key)) {
            pet = names.get(key);
            names.remove(key);
        }
        return pet;
    }

    /* Print Pets
     * 
     */
    public void print() {
        // iterate over the keys in the HashMap
        for (String name: names.keySet()) {
            Pet obj = names.get(name);
            System.out.println(name + " is a " + obj.getColor() + " " + obj.getName() + " and is " + obj.getAge() + " years old.");
        }
        System.out.println();
    }

    /* Tester Method
     * 
     */
    public static void main(String[] args) {

        // intialize Pets
        Pets pets = new Pets();
        pets.print();
        
        // remove a Pet
        String key = "Hobbes";
        Pet pet = pets.remove(key);
        if (pet == null) {
            System.out.println(key + " not found");
        } else {
            System.out.println("Removed: " + key + ", " + pet);
        }
        pets.print();

    }
}
Pets.main(null);
```

    Hobbes is a Calico Kitty and is 1 years old.
    Leo is a Gold Lion and is 8 years old.
    Porky is a Pink Pig and is 3 years old.
    Ro-Ro is a Red Robin and is 7 years old.
    Duke is a Brown Dog and is 14 years old.
    Midnight is a Black Cat and is 10 years old.
    
    Removed: Hobbes, REPL.$JShell$34$Pet@5f7d8bd3
    Leo is a Gold Lion and is 8 years old.
    Porky is a Pink Pig and is 3 years old.
    Ro-Ro is a Red Robin and is 7 years old.
    Duke is a Brown Dog and is 14 years old.
    Midnight is a Black Cat and is 10 years old.
    


## HashSet
> Below is an example using a java.util.Set and listed are some key things to consider when using this data structure.  A Set works similarly to a key in a HashMap, a Set is just the Key.

1. No duplicates: A Set does not allow duplicate elements. If you try to add an element that already exists in the Set, the add() method will return false and the Set will not be modified.  Duplicate add is shown in example.

2. Unordered: A Set does not maintain the insertion order of elements. The order of elements in a Set may change as elements are added or removed.

3. Equality: Two Sets are considered equal if they have the same elements, regardless of their order. The equals() method is used to test for Set equality.

4. Implementation classes: Java provides several implementation classes for the Set interface, including HashSet, LinkedHashSet, and TreeSet. Each implementation has different performance characteristics and is optimized for different use cases.

5. Iterator: The iterator() method can be used to iterate over the elements in a Set. The order in which elements are returned by the iterator is not defined and may change over time as elements are added or removed from the Set. The forEach() method is another way to iterate over the elements in a Set, and it allows you to pass a lambda expression to process each element in the Set.  Lambda expression is shown in example.


```Java
import java.util.HashSet;
import java.util.Set;

public class petset {
    public static void main(String[] args) {
        // create a new HashSet
        Set<String> pets = new HashSet<>();

        // add some elements to the Set
        pets.add("lion");
        pets.add("dog");
        pets.add("cat");

        // print out the Set
        System.out.println(pets);

        // check if an element is in the Set
        boolean hasLion = pets.contains("lion");
        System.out.println("Has lion: " + hasLion);

        // remove an element from the Set
        pets.remove("lion");
        System.out.println("Removed lion");

        // print out the Set
        System.out.println(pets);

        // add duplicate
        System.out.println("add duplicate dog");
        pets.add("dog");  // no action
        System.out.println(pets);
        // add duplicate
        System.out.println("add pig");
        pets.add("pig");
        System.out.println(pets);

        // using forEach() method with a lambda expression
        pets.forEach(animal -> {
            String message = "I ";
            // ternary operation for like, don't like
            message += animal.equals("dog") ? "like" : "don't like";
            // complete sentance
            message += " " + animal + "s " + "for pets";
            System.out.println(message);
        });

    }
}
petset.main(null);

```

    [cat, dog, lion]
    Has lion: true
    Removed lion
    [cat, dog]
    add duplicate dog
    [cat, dog]
    add pig
    [cat, dog, pig]
    I don't like cats for pets
    I like dogs for pets
    I don't like pigs for pets


## Hacks
Build your own Hashmap and HashSet.
