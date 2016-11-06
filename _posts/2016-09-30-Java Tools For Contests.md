---
layout: post
title:  "Java Tools For Contests"
date:   2016-09-30 15:50:03 -0400
categories: Contests
---
**Libraries**
{% highlight java %}
import java.util.*;
{% endhighlight %}


**Nested Lists**

{% highlight java %}
List<List<String>> stringArray = new ArrayList<List<String>>();
stringArray.add(new ArrayList<String>());
stringArray.get(0).add("A new string");
{% endhighlight %}


**Hash Map**

{% highlight java %}
import java.util.*;

HashMap hm = new HashMap();
hm.put("Zara", new Double(3434.34));
Set set = hm.entrySet(); //set of keys
Iterator itr = set.iterator();
while(i.hasNext()) {
   Map.Entry me = (Map.Entry)i.next();
   System.out.print(me.getKey()); //prints a key
   System.out.println(me.getValue()); //prints a value
}
hm.get("key");
{% endhighlight %}


**Set**
- https://www.tutorialspoint.com/java/java_set_interface.htm

{% highlight java %}
import java.util.*;
Set<Integer> set = new HashSet<Integer>();
set.add(0);
{% endhighlight %}
