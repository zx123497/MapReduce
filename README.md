# MapReduce
> A solution for distributed system.
> [維基百科 for MapReduce](https://zh.wikipedia.org/zh-tw/MapReduce)
## Why Use MapReduce
- In distributed systems, executing multi-machine tasks can be risky, becuase some of the machines may crash and get incorrect results.
- MapReduce can manage the tasks in every single machine and ensure the result are right (fault tolerance).
- Almost all kinds of multi-machine tasks can be implemented with MapReduce.
## Concept
- There are two functions in MapReduce:

### 1. Map()
- Map Function will handle the task for every single machine, and generate key pairs.
- Example (we are going to do wordcount tasks from  files in many machines.)
    ```
    // input: a file with an english assay.
    // output: list of key pairs (word, 1) for every word occur in the essay.

    Map(File file){
        string[] content = file.readFile();
        KeyPair[] result = [];

        for word in content:
            result += new KeyPair(word, 1);

        return result;
    }
    ```
- So, every machine will run Map(), and return a list of key pair
    - e.g. 
    ```
    keyPair = [("I", 1), ("am", 1), ("an", 1), ("apple",1),...]
    ```
### 1. Reduce()
- Reduce function will calculate the result using the keyPairs generate from multiple machines.
- Example
```
// input: keypairslist from machines (aggregated into a intermediate data: keypairlist[])
// output: print wordcount for all word occurance.

Reduce(keyPairList[] list){
    
    string[] wordBank = [];
    int[] count = [];
    
    for list in keyPairList:
        for keyPair in list:
            int index = wordBank.search(keyPair[0]);
            if index != -1:
                count[index] += 1;
            else:
                wordBank.append(keyPair[0]);
                index = wordBank.search(keyPair[0]);
                count[index] += 1;
                
    for i in arange(count.length):
        print(wordBank[i] + ": " + count[i]);
}
```
- Reduce() will use the key in keyPairs to implement calculations.

## Implement Method
- There will be a **"Master"** controlling all the **map()** and **reduce()**
![](https://i.imgur.com/ZvyvWOd.png)
- The **Master** will assign **map()** or **reduce()** to each machine, giving them **limited time** to finish tasks. (e.g. 10 seconds)
- If time up but not finish (maybe because the **machine crash** or **slow performance**), the master will backup the input in the slow or crashed machine to **another machine** and **rerun** functions.
- In this way, we can ensure the task run **fast** and get **correct** results.

## Sum up
- It help people **easy to implement** distributed system services.
- It can ensure the **accuracy** of the result and the **performance** of the entire task.
