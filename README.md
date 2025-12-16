# Task_1

##Problem 1: The "Frequency Count" (Maps) 
Scenario: You are analyzing a server log. You have a list of IP addresses that 
accessed the server.
##Task: Write a function that takes a List<String> of IP addresses and returns a 
Map<String, Integer> representing the count of how many times each IP appears.
• Constraint: The solution should be efficient ($O(N)$ time complexity).
• Key Concept: HashMap, getOrDefault.

##Code:
package Collections.Maps.HashMap;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.HashMap;
import java.util.List;

public class CountIPAddress {
    public static void main(String[] args) {
        List<String> list = List.of(
                "192.168.1.1",
                "10.0.0.2",
                "192.168.1.1",
                "10.0.0.3",
                "10.0.0.2");
        HashMap<String,Integer> fre = new HashMap<>();
        for(String ip: list){
            fre.put(ip,fre.getOrDefault(ip,0)+1);
        }
        System.out.println(fre);
    }
}

===========================================================================================================================

##Problem 2: The "De-duplication"
Scenario: You are merging two mailing lists. One list contains emails from your 
"Newsletter" subscribers, and the other from your "Product Update" subscribers.
##Task:
1. Create a single list of unique emails that are present in either list.
2. Create a list of emails that are present in both lists (intersection).
• Key Concept: HashSet, addAll, retainAll

package Collections.Set.HashSet;
import java.util.Collections;
import java.util.HashSet;
import java.util.List;

public class MailSubscription {
    public static void main(String[] args) {
        List<String> newsletterMails = List.of(
                "alice@gmail.com",
                "bob@gmail.com",
                "charlie@gmail.com",
                "david@gmail.com",
                "emma@gmail.com"         );
        List<String> productUpdateMails = List.of(
                "charlie@gmail.com",
                "david@gmail.com",
                "frank@gmail.com",
                "george@gmail.com",
                "alice@gmail.com"         );

        HashSet<String> uniqueMails = new HashSet<>(newsletterMails);
        uniqueMails.addAll(productUpdateMails);
        System.out.println("Unique Mails: "+ uniqueMails);

        HashSet<String> commonMails = new HashSet<>(newsletterMails);
        commonMails.retainAll(productUpdateMails);
        System.out.println("Common Mails: " + commonMails);
    }
} 

===========================================================================================================================
##Problem 3: The "Custom Sorting" (Comparators)
 The "Custom Sorting" (Comparators)
Scenario: You are building a leaderboard for a game. You have a Player class with 
fields: name (String), score (int), and level (int).
##Task: Store players in a collection that automatically keeps them sorted.
• Sorting Logic:
1. Higher score comes first.
2. If scores are equal, the player with the higher level comes first.
3. If levels are also equal, sort alphabetically by name.
• Key Concept: TreeSet or Collections.sort, Comparator, thenComparing.

package Collections;
import java.util.*;
class Player{
    String name;
    int score;
    int level;

    public Player(String name, int score, int level) {
        this.name = name;
        this.score = score;
        this.level = level;
    }
    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getScore() {
        return score;
    }

    public void setScore(int score) {
        this.score = score;
    }

    public int getLevel() {
        return level;
    }

    public void setLevel(int level) {
        this.level = level;
    }

    @Override     public String toString() {
        return                 "name='" + name + '\'' +
                ", score=" + score +
                ", level=" + level;
    }
}
public class ComparatorLogicProblem {

    public static void main(String[] args) {
        TreeSet<Player> players = new TreeSet<>(
                Comparator.comparingInt(Player::getScore).reversed()
                        .thenComparing(Comparator.comparing(Player::getLevel).reversed())
                        .thenComparing(Player::getName)
        );
        players.addAll(Arrays.asList(
                new Player("ravi",120,5),
                new Player("anil",150,6),
                new Player("vamsi",150,6),
                new Player("kumar",120,7),
                new Player("suresh",150,5)

        ));
        System.out.println(players);
    }
}

===========================================================================================================================

##Problem 4: The "LRU Cache" (LinkedHashMap)
Scenario: You need to implement a simple "Least Recently Used" (LRU) cache for a 
web browser history. The cache has a fixed size (e.g., 5 URLs).
##Task: Implement a class BrowserHistory that stores URLs.
• When a new URL is visited, add it to the cache.
• If the cache is full, remove the oldest (least recently accessed) URL to make 
space.
• Accessing an existing URL should move it to the "most recently used" 
position.
• Key Concept: LinkedHashMap (specifically the constructor with accessOrder set 
to true and overriding removeEldestEntry).

package Collections;
import java.util.LinkedHashMap;
import java.util.Map;
public class BrowsingHistory {
    int capacity;

    BrowsingHistory(int capacity){
        this.capacity = capacity;
    }
    LinkedHashMap<String ,String> cache = new LinkedHashMap<>(capacity,0.75f,true){
        @Override         protected boolean removeEldestEntry(Map.Entry<String, String> eldest) {
            return size()>capacity;
        }
    };
    public void visit(String url){
        cache.put(url,url);
    }
    public void showHistory(){
        System.out.println(cache.keySet());
    }
    public static void main(String[] args) {
        BrowsingHistory history = new BrowsingHistory(5);
        history.visit("google.com");
        history.visit("youtube.com");
        history.visit("github.com");
        history.visit("StackOverflow.com");
        history.visit("openai.com");

        history.showHistory();
        history.visit("youtube.com");
        history.showHistory();

        history.visit("linkedin.com");
        history.showHistory();
    }
} 

===========================================================================================================================

Problem 5: The "Fail-Safe vs Fail-Fast" (Iterators)
Scenario: You have a list of active orders in a processing queue. A background 
thread is iterating through this list to print report logs.
##Task:
1. Create a loop that iterates through the list.
2. Inside the loop, try to add a new order to the list.
3. Observe the ConcurrentModificationException.
4. Fix it: Replace the collection with one that allows modification during iteration 
without crashing.
• Key Concept: CopyOnWriteArrayList vs ArrayList, Iterator.

package Collections;
import java.util.ArrayList;
import java.util.Iterator;
import java.util.concurrent.CopyOnWriteArrayList;

public class FailSafevsFailFast {
    public static void main(String[] args) {


        //Example for fail-fast
//        ArrayList<String> al = new ArrayList<>();
//        al.add("Order - 101");
//        al.add("Order - 102");
//        al.add("Order - 103");
//
//        Iterator<String> it = al.iterator();
//        System.out.println("Here are the active Orders...");
//        while (it.hasNext()){
//            al.add("Order - 104");
//            System.out.println(it.next());
//        }
//

        //example for fail-safe
        CopyOnWriteArrayList<String> ca = new CopyOnWriteArrayList<>();
        ca.add("Order - 101");
        ca.add("Order - 102");
        ca.add("Order - 103");
        ca.add("Order - 104");

        Iterator<String> it = ca.iterator();
        while(it.hasNext()){
            ca.add("Order - 105");
            System.out.println(it.next());
        }

    }
}

===========================================================================================================================
##Problem 6: The "Grouping" (Streams & Collectors)
Scenario: You have a list of Product objects. Each product has a category (String) 
and a price (double).
##Task: Convert this list into a Map<String, List<Product>> where the key is the 
category and the value is a list of all products belonging to that category.
• Key Concept: Stream, Collectors.groupingBy

package Collections;
import java.util.List;
import java.util.stream.Collectors;

class Product{
    String name;
    String category;
    double price;

    public Product(String name, String category, double price) {
        this.name = name;
        this.category = category;
        this.price = price;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getCategory() {
        return category;
    }

    public void setCategory(String category) {
        this.category = category;
    }

    public double getPrice() {
        return price;
    }

    public void setPrice(double price) {
        this.price = price;
    }

    @Override     public String toString() {
        return "Product{" +
                "name='" + name + '\'' +
                ", category='" + category + '\'' +
                ", price=" + price +
                '}';
    }
}

public class GroupingWithStream {
    public static void main(String[] args) {

        List<Product> productList = List.of(
                new Product("IPhone", "Electronics", 75000),
                new Product("Laptop", "Electronics", 85000),
                new Product("HeadPhones", "Electronics", 7000),
                new Product("Shirt", "Clothing", 1200),
                new Product("Rice", "Grocery", 900)
        );

        System.out.println( productList.stream().collect(Collectors.groupingBy(Product::getCategory)));


    }
}







			



