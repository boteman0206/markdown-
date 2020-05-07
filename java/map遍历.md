#### 1： 使用entrySet()

````java
Map<String, Integer> items = new HashMap<>();
items.put("A", 10);
items.put("B", 20);
items.put("C", 30);
items.put("D", 40);
items.put("E", 50);
items.put("F", 60);
for (Map.Entry<String, Integer> entry : items.entrySet()) {
    System.out.println("Item : " + entry.getKey() + " Count : " + entry.getValue());
}

````





#### 2:  使用forEach + lambda表达式

````java
Map<String, Integer> items = new HashMap<>();
items.put("A", 10);
items.put("B", 20);
items.put("C", 30);
items.put("D", 40);
items.put("E", 50);
items.put("F", 60);
items.forEach((k,v)->System.out.println("Item : " + k + " Count : " + v));
items.forEach((k,v)->{
    System.out.println("Item : " + k + " Count : " + v);
    if("E".equals(k)){
        System.out.println("Hello E");
    }
});

````

``namesMap.entrySet().forEach(entry -> System.out.println(entry.getKey() + " " + entry.getValue()));``



#### 3: keySet和values

````JAVA
Map<Integer, Integer> map = new HashMap<Integer, Integer>(); 
//遍历map中的键 
for (Integer key : map.keySet()) { 
  System.out.println("Key = " + key); 
} 
//遍历map中的值 
for (Integer value : map.values()) { 
  System.out.println("Value = " + value); 
}

````

#### 4：Iterator

````java
Map<Integer, Integer> map = new HashMap<Integer, Integer>(); 
Iterator<Map.Entry<Integer, Integer>> entries = map.entrySet().iterator(); 
while (entries.hasNext()) { 
  Map.Entry<Integer, Integer> entry = entries.next(); 
  System.out.println("Key = " + entry.getKey() + ", Value = " + entry.getValue()); 
}

````

#### 5： **通过键找值遍历（效率低）** 

````java
Map<Integer, Integer> map = new HashMap<Integer, Integer>(); 
for (Integer key : map.keySet()) { 
  Integer value = map.get(key); 
  System.out.println("Key = " + key + ", Value = " + value);
  }

````

