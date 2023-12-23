---
layout: post
title: Collectable Lesson
description: Usage of compareTo and toString in Collectable framework. This example uses lots of abstraction and is able to
courses: {'csa': {'week': 14}}
type: ccc
---

## POM File Dependencies
This project requires Jackson for JSON building.  The loadFromPOM cell must be oaded before proceeding.


```java
%%loadFromPOM
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.12.4</version> <!-- Use the latest version -->
</dependency>
```

## Abstract Collectable

Establish class heirarchy for Collectable.  The class is used to manage a catalog of items.

- abstract class required child definitions in order to instantiate
- abstract methods help enforce definitions
- implements defines method compareTo to enable things like sorting


```java
import com.fasterxml.jackson.databind.ObjectMapper;


/* This is parent class, objectives...
   - Push functionality into parent Class 
   - Enforce consistent definition of child Class

 */
public abstract class Collectable implements Comparable <Collectable> {
	public final String masterType = "Collectable";
	private String type;	// extender should define their data type

	/* Enumerated interface of key types 
	 * an interface named KeyTypes is declared with a single method name(). 
	 * the Collectable class contains an abstract method getKey(), 
	 * which must be implemented by its subclasses. 
	 * must provide a method that returns an object implementing the KeyTypes interface.
	*/ 
	public interface KeyTypes {
		String name();
	}
	protected abstract KeyTypes getKey();
	protected abstract KeyTypes getSortKey();
	protected abstract String getSortKeyValue();

	// getter
	public String getMasterType() {
		return masterType;
	}

	// getter
	public String getType() {
		return type;
	}

	// setter
	public void setType(String type) {
		this.type = type;
	}
	
	/* This method is used to build JSON using Jackson
	 * Object mapper add this attributes to JSON
	 * Exeption returns empty JSON
	*/  
    protected String toJson() {
        try {
            ObjectMapper objectMapper = new ObjectMapper();
            return objectMapper.writeValueAsString(this);
        } catch (Exception e) {
            e.printStackTrace();
            return "{}";
        }
    }

	/* 'Collectable' requires toString override for compareTo
	 * JSON data is used and formed with keys: key, data 
	 */
	@Override
	public String toString()
	{
		return "{"
			+ "\"key\":\"" + getSortKeyValue() + "\","
			+ "\"data\":\"" + toJson() + 
			"}";
	}

	/* This method is used to compare toString of objects
	 * the compareTo method is implemented from the Comparable interface
	 * it compares the string representations of two Collectable objects 
	 * using their toString methods
	*/
	public int compareTo(Collectable obj) {
		return this.toString().compareTo(obj.toString());
	}


	/* This method outputs a Collectable Array to console
	 * the object uses toString to show contents
	*/
	public static void print(Collectable[] objs) {
		// print "Collectable: Objects'
		for(Object o : objs)	// observe that type is Opaque
			System.out.println(o);

		System.out.println();
	}
}
```


```java
/*
 * Pet class extends Collectable and defines abstract methods
 */
public class Pet extends Collectable {
	// Class data
	public enum KeyType implements KeyTypes {title, name, age, color, catalog}
	private static KeyTypes key = KeyType.title;  // static initializer
	private static KeyTypes sortKey = KeyType.title;
	public static void setOrder(KeyTypes key) { Pet.key = key; }
	public static void sort(Collectable[] c, KeyType key) { 
		Pet.key = key; 
		Pet.sortKey = key; 
		Arrays.sort(c); 
		Pet.key = KeyType.title; 
	}

	// Instance data
	private final String name;
	private final int age;
	private final String color;

	/* constructor
	 *
	 */
	public Pet(String name, int age, String color)
	{
		super.setType("Pet");
		this.name = name;
		this.age = age;
		this.color = color;
	}

	/* 'Collectable' requires getKey to help enforce KeyTypes usage */
	@Override
	protected KeyTypes getKey() { return Pet.key; }
	@Override
	protected KeyTypes getSortKey() { return Pet.sortKey; }

	/* Getters / Accessors
	 * 
	 */
	public String getName() { return this.name; }
	public int getAge() { return this.age; }
	public String getColor() { return this.color; }

	/* 'Collectable' requires getSortKeyValue override
	 * provides key based off of Static Key setting
	 */
	protected String getSortKeyValue() {
        if (KeyType.name.equals(getSortKey())) {
            return getName();
        } else if (KeyType.age.equals(getSortKey())) {
			String zeroPad = "00" + getAge();
            return zeroPad.substring(zeroPad.length() - 2);
        } else if (KeyType.color.equals(getSortKey())) {
            return getColor();
		} else if (KeyType.catalog.equals(getSortKey())) {
			return getName() + " " + getColor() + " " + getAge();
        }
        // Default is Type
        return getType();
    }

	// Test data initializer
	public static Pet[] pets() {
		return new Pet[]{
				new Pet("Lion", 8, "Gold"),
				new Pet("Pig", 3, "Pink"),
				new Pet("Robin", 7, "Red"),
				new Pet("Cat", 10, "Black"),
				new Pet("Kitty", 1, "Calico"),
				new Pet("Dog", 14, "Brown")
		};
	}
	
	/* main to test Pet class
	 * 
	 */
	public static void main(String[] args)
	{
		// print with title
		Pet.setOrder(KeyType.title);
		Collectable[] pets = Pet.pets();

		for (Pet.KeyType key : Pet.KeyType.values()) {
			Pet.sort(pets, key);
			Collectable.print(pets);
		}
	}

}
Pet.main(null);
```

    {"key":"Pet","data":"{"masterType":"Collectable","type":"Pet","name":"Cat","age":10,"color":"Black"}}
    {"key":"Pet","data":"{"masterType":"Collectable","type":"Pet","name":"Dog","age":14,"color":"Brown"}}
    {"key":"Pet","data":"{"masterType":"Collectable","type":"Pet","name":"Kitty","age":1,"color":"Calico"}}
    {"key":"Pet","data":"{"masterType":"Collectable","type":"Pet","name":"Lion","age":8,"color":"Gold"}}
    {"key":"Pet","data":"{"masterType":"Collectable","type":"Pet","name":"Pig","age":3,"color":"Pink"}}
    {"key":"Pet","data":"{"masterType":"Collectable","type":"Pet","name":"Robin","age":7,"color":"Red"}}
    
    {"key":"Cat","data":"{"masterType":"Collectable","type":"Pet","name":"Cat","age":10,"color":"Black"}}
    {"key":"Dog","data":"{"masterType":"Collectable","type":"Pet","name":"Dog","age":14,"color":"Brown"}}
    {"key":"Kitty","data":"{"masterType":"Collectable","type":"Pet","name":"Kitty","age":1,"color":"Calico"}}
    {"key":"Lion","data":"{"masterType":"Collectable","type":"Pet","name":"Lion","age":8,"color":"Gold"}}
    {"key":"Pig","data":"{"masterType":"Collectable","type":"Pet","name":"Pig","age":3,"color":"Pink"}}
    {"key":"Robin","data":"{"masterType":"Collectable","type":"Pet","name":"Robin","age":7,"color":"Red"}}
    
    {"key":"01","data":"{"masterType":"Collectable","type":"Pet","name":"Kitty","age":1,"color":"Calico"}}
    {"key":"03","data":"{"masterType":"Collectable","type":"Pet","name":"Pig","age":3,"color":"Pink"}}
    {"key":"07","data":"{"masterType":"Collectable","type":"Pet","name":"Robin","age":7,"color":"Red"}}
    {"key":"08","data":"{"masterType":"Collectable","type":"Pet","name":"Lion","age":8,"color":"Gold"}}
    {"key":"10","data":"{"masterType":"Collectable","type":"Pet","name":"Cat","age":10,"color":"Black"}}
    {"key":"14","data":"{"masterType":"Collectable","type":"Pet","name":"Dog","age":14,"color":"Brown"}}
    
    {"key":"Black","data":"{"masterType":"Collectable","type":"Pet","name":"Cat","age":10,"color":"Black"}}
    {"key":"Brown","data":"{"masterType":"Collectable","type":"Pet","name":"Dog","age":14,"color":"Brown"}}
    {"key":"Calico","data":"{"masterType":"Collectable","type":"Pet","name":"Kitty","age":1,"color":"Calico"}}
    {"key":"Gold","data":"{"masterType":"Collectable","type":"Pet","name":"Lion","age":8,"color":"Gold"}}
    {"key":"Pink","data":"{"masterType":"Collectable","type":"Pet","name":"Pig","age":3,"color":"Pink"}}
    {"key":"Red","data":"{"masterType":"Collectable","type":"Pet","name":"Robin","age":7,"color":"Red"}}
    
    {"key":"Cat Black 10","data":"{"masterType":"Collectable","type":"Pet","name":"Cat","age":10,"color":"Black"}}
    {"key":"Dog Brown 14","data":"{"masterType":"Collectable","type":"Pet","name":"Dog","age":14,"color":"Brown"}}
    {"key":"Kitty Calico 1","data":"{"masterType":"Collectable","type":"Pet","name":"Kitty","age":1,"color":"Calico"}}
    {"key":"Lion Gold 8","data":"{"masterType":"Collectable","type":"Pet","name":"Lion","age":8,"color":"Gold"}}
    {"key":"Pig Pink 3","data":"{"masterType":"Collectable","type":"Pet","name":"Pig","age":3,"color":"Pink"}}
    {"key":"Robin Red 7","data":"{"masterType":"Collectable","type":"Pet","name":"Robin","age":7,"color":"Red"}}
    



```java
/*
 * Car class extends Collectable and defines abstract methods
 */
public class Car extends Collectable {
	// Class data
	public enum KeyType implements KeyTypes {title, make, model, year, color, catalog}
	private static KeyTypes key = KeyType.title;  // static initializer
	private static KeyTypes sortKey = KeyType.title;
	public static void setOrder(KeyTypes key) { Car.key = key; }
	public static void sort(Collectable[] c, KeyType key) { 
		Car.key = key; 
		Car.sortKey = key; 
		Arrays.sort(c); 
		Car.key = KeyType.title; 
	}

	// Instance data
    private final String make;
    private final String model;
	private final int year;
	private final String color;

	/* constructor
	 *
	 */
	public Car(String make, String model, int year, String color)
	{
		super.setType("Car");
		this.make = make;
        this.model = model;
		this.year = year;
		this.color = color;
	}

	/* 'Collectable' requires getKey to help enforce KeyTypes usage */
	@Override
	protected KeyTypes getKey() { return Car.key; }
	@Override
	protected KeyTypes getSortKey() { return Car.sortKey; }

	/* Getters / Accessors
	 * 
	 */
	public String getMake() { return this.make; }
    public String getModel() { return this.model; }
	public int getYear() { return this.year; }
	public String getColor() { return this.color; }

	/* 'Collectable' requires getSortKeyValue override
	 * toString provides data based off of Static Key setting
	 */
	@Override
    protected String getSortKeyValue() {
        if (KeyType.make.equals(getSortKey())) {
            return getMake();
        } else if (KeyType.model.equals(getSortKey())) {
            return getModel();
        } else if (KeyType.year.equals(getSortKey())) {
            return String.valueOf(getYear());
        } else if (KeyType.color.equals(getSortKey())) {
            return getColor();
		} else if (KeyType.catalog.equals(getSortKey())) {
			return getMake() + " " + getModel() + " " + getYear();
        }
        // Default is Type
        return getType();
    }

	// Test data initializer
	public static Car[] cars() {
		return new Car[]{
				new Car("Ford", "Fusion", 2015, "Guard"),
				new Car("Ford", "Excursion", 2003, "Green"),
                new Car("Ford", "F-350", 1997, "Green"),
                new Car("Cadillac", "Boughman", 1969, "Black"),
                new Car("Acura", "RL", 2006, "Silver")
		};
	}
	
	/* main to test Car class
	 * 
	 */
	public static void main(String[] args)
	{
		// print with title
		Car.setOrder(KeyType.title);
		Collectable[] cars = Car.cars();

		for (Car.KeyType key : Car.KeyType.values()) {
			Car.sort(cars, key);
			Collectable.print(cars);
		}
	}

}
Car.main(null);
```

    {"key":"Car","data":"{"masterType":"Collectable","type":"Car","make":"Acura","model":"RL","year":2006,"color":"Silver"}}
    {"key":"Car","data":"{"masterType":"Collectable","type":"Car","make":"Cadillac","model":"Boughman","year":1969,"color":"Black"}}
    {"key":"Car","data":"{"masterType":"Collectable","type":"Car","make":"Ford","model":"Excursion","year":2003,"color":"Green"}}
    {"key":"Car","data":"{"masterType":"Collectable","type":"Car","make":"Ford","model":"F-350","year":1997,"color":"Green"}}
    {"key":"Car","data":"{"masterType":"Collectable","type":"Car","make":"Ford","model":"Fusion","year":2015,"color":"Guard"}}
    
    {"key":"Acura","data":"{"masterType":"Collectable","type":"Car","make":"Acura","model":"RL","year":2006,"color":"Silver"}}
    {"key":"Cadillac","data":"{"masterType":"Collectable","type":"Car","make":"Cadillac","model":"Boughman","year":1969,"color":"Black"}}
    {"key":"Ford","data":"{"masterType":"Collectable","type":"Car","make":"Ford","model":"Excursion","year":2003,"color":"Green"}}
    {"key":"Ford","data":"{"masterType":"Collectable","type":"Car","make":"Ford","model":"F-350","year":1997,"color":"Green"}}
    {"key":"Ford","data":"{"masterType":"Collectable","type":"Car","make":"Ford","model":"Fusion","year":2015,"color":"Guard"}}
    
    {"key":"Boughman","data":"{"masterType":"Collectable","type":"Car","make":"Cadillac","model":"Boughman","year":1969,"color":"Black"}}
    {"key":"Excursion","data":"{"masterType":"Collectable","type":"Car","make":"Ford","model":"Excursion","year":2003,"color":"Green"}}
    {"key":"F-350","data":"{"masterType":"Collectable","type":"Car","make":"Ford","model":"F-350","year":1997,"color":"Green"}}
    {"key":"Fusion","data":"{"masterType":"Collectable","type":"Car","make":"Ford","model":"Fusion","year":2015,"color":"Guard"}}
    {"key":"RL","data":"{"masterType":"Collectable","type":"Car","make":"Acura","model":"RL","year":2006,"color":"Silver"}}
    
    {"key":"1969","data":"{"masterType":"Collectable","type":"Car","make":"Cadillac","model":"Boughman","year":1969,"color":"Black"}}
    {"key":"1997","data":"{"masterType":"Collectable","type":"Car","make":"Ford","model":"F-350","year":1997,"color":"Green"}}
    {"key":"2003","data":"{"masterType":"Collectable","type":"Car","make":"Ford","model":"Excursion","year":2003,"color":"Green"}}
    {"key":"2006","data":"{"masterType":"Collectable","type":"Car","make":"Acura","model":"RL","year":2006,"color":"Silver"}}
    {"key":"2015","data":"{"masterType":"Collectable","type":"Car","make":"Ford","model":"Fusion","year":2015,"color":"Guard"}}
    
    {"key":"Black","data":"{"masterType":"Collectable","type":"Car","make":"Cadillac","model":"Boughman","year":1969,"color":"Black"}}
    {"key":"Green","data":"{"masterType":"Collectable","type":"Car","make":"Ford","model":"Excursion","year":2003,"color":"Green"}}
    {"key":"Green","data":"{"masterType":"Collectable","type":"Car","make":"Ford","model":"F-350","year":1997,"color":"Green"}}
    {"key":"Guard","data":"{"masterType":"Collectable","type":"Car","make":"Ford","model":"Fusion","year":2015,"color":"Guard"}}
    {"key":"Silver","data":"{"masterType":"Collectable","type":"Car","make":"Acura","model":"RL","year":2006,"color":"Silver"}}
    
    {"key":"Acura RL 2006","data":"{"masterType":"Collectable","type":"Car","make":"Acura","model":"RL","year":2006,"color":"Silver"}}
    {"key":"Cadillac Boughman 1969","data":"{"masterType":"Collectable","type":"Car","make":"Cadillac","model":"Boughman","year":1969,"color":"Black"}}
    {"key":"Ford Excursion 2003","data":"{"masterType":"Collectable","type":"Car","make":"Ford","model":"Excursion","year":2003,"color":"Green"}}
    {"key":"Ford F-350 1997","data":"{"masterType":"Collectable","type":"Car","make":"Ford","model":"F-350","year":1997,"color":"Green"}}
    {"key":"Ford Fusion 2015","data":"{"masterType":"Collectable","type":"Car","make":"Ford","model":"Fusion","year":2015,"color":"Guard"}}
    



```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class CollectionExample {
	
    public static void main(String[] args) {
        // use List with dynamic size capabilities for combining Arrays
        List<Collectable> collection = new ArrayList<>();

        // use List.of to add Arrays to collection List
        collection.addAll(List.of(Pet.pets()));
        collection.addAll(List.of(Car.cars()));

        // set Pet and Car to catalog ordering 
        Pet.setOrder(Pet.KeyType.catalog);
        Car.setOrder(Car.KeyType.catalog);

        // Sort will order based on compareTo
        Collections.sort(collection);

        // Collectable object will output according to toString 
        for (Collectable c : collection) {
            System.out.println(c);
        }
    }
}
CollectionExample.main(null);

```

    {"key":"Acura RL 2006","data":"{"masterType":"Collectable","type":"Car","make":"Acura","model":"RL","year":2006,"color":"Silver"}}
    {"key":"Cadillac Boughman 1969","data":"{"masterType":"Collectable","type":"Car","make":"Cadillac","model":"Boughman","year":1969,"color":"Black"}}
    {"key":"Cat Black 10","data":"{"masterType":"Collectable","type":"Pet","name":"Cat","age":10,"color":"Black"}}
    {"key":"Dog Brown 14","data":"{"masterType":"Collectable","type":"Pet","name":"Dog","age":14,"color":"Brown"}}
    {"key":"Ford Excursion 2003","data":"{"masterType":"Collectable","type":"Car","make":"Ford","model":"Excursion","year":2003,"color":"Green"}}
    {"key":"Ford F-350 1997","data":"{"masterType":"Collectable","type":"Car","make":"Ford","model":"F-350","year":1997,"color":"Green"}}
    {"key":"Ford Fusion 2015","data":"{"masterType":"Collectable","type":"Car","make":"Ford","model":"Fusion","year":2015,"color":"Guard"}}
    {"key":"Kitty Calico 1","data":"{"masterType":"Collectable","type":"Pet","name":"Kitty","age":1,"color":"Calico"}}
    {"key":"Lion Gold 8","data":"{"masterType":"Collectable","type":"Pet","name":"Lion","age":8,"color":"Gold"}}
    {"key":"Pig Pink 3","data":"{"masterType":"Collectable","type":"Pet","name":"Pig","age":3,"color":"Pink"}}
    {"key":"Robin Red 7","data":"{"masterType":"Collectable","type":"Pet","name":"Robin","age":7,"color":"Red"}}


## Hacks
Build your own Collectable class(es), combine them, and sort them.
