# Content
- [deque](https://github.com/vanikk06/Data-structures-and-Algorithms/tree/master/week_12#deque)
- [Dict & Set](https://github.com/vanikk06/Data-structures-and-Algorithms/tree/master/week_12#dict--set)
- [Level-Order Traversal](https://github.com/vanikk06/Data-structures-and-Algorithms/tree/master/week_12#level-order-traversal)
- [Graph](https://github.com/vanikk06/Data-structures-and-Algorithms/tree/master/week_12#graph)
- [Breadth-First Search](https://github.com/vanikk06/Data-structures-and-Algorithms/tree/master/week_12#breadth-first-search)
- [Adjustment of Design BFS](https://github.com/vanikk06/Data-structures-and-Algorithms/tree/master/week_12#adjustment-of-design-bfs)

# deque
 > python collections.deque(x, maxlen=None)
 >> double-ended queue（雙端隊列結構）
 
類似於list，提供在**兩端**加入和刪除的操作
 > 優化：在兩端的操作皆為O(1)
 >> 原本list在`pop(0)`與`insert(0, v)`其為O(n)\
 因其內存移動的操作，改變底層data表達的大小和位置

```python
from collections import deque

queue = deque('123244') #格式須為字串
queue
```
若直接在建立物件時輸入資料，會採用`extend`將元素拆包
```python
deque(['1', '2', '3', '2', '4', '4'])
```

繼承list所有功能：
 > input的data皆為**字串**格式
 - `len()`：長度
 - `queue[index]`：索引查詢
    > -1：倒數第一個
 - `remove('x')`：刪除X元素
    > 若找不到，則出現`ValueError`
 - `append('x')`：從最右邊（後面）增加x元素
    > 不拆包
 - `extend('x')`：從最右邊（後面）增加x元素
    > 拆包
 - `pop()`：取出最右邊的值
    > 移除並且返回元素
    >> 若無元素，則會出現`IndexError`
 - `clear()`：移除所有元素，使其長度為0
 - `copy()`：複製備份
 - `count('x')`：計算deque中等於x元素的個數
 - `index('x', start, stop)`：返回第x個元素（從start到stop），返回第一個匹配index
    > 若沒有找到，則會出現` ValueError`
 - `insert(index, 'x')`：在index的位置插入一個x元素
    > 若插入導致deque超出maxlen限定的長度，則會出現`IndexError`
 - `reverse()`：將deque逆序排列

新增功能：
 - `deque(x, maxlen=None)`：deque例項化
     - `maxlen`：制定deque的最大尺寸，預設為None
        > 當限定長度滿足時，當新元素增加時（append/extend），相同數量的元素會從另一端刪除
 - `appendleft('x')`：從最左邊（前面）增加x元素
    > 不拆包
 - `extendleft('x')`：從最左邊（前面）增加x元素
    > 拆包
 - `popleft()`：取出最左邊的值
    > 移除並且返回元素
     >> 若無元素，則會出現`IndexError`
 - `rotate(n)`：
     - n為正數：向右旋轉移動n步
       > 當n = 1，等價於`queue.appendleft(queue.pop())`
     - n為負數：向左旋轉移動n步
       > 當n = -1，等價於`queue.append(queue.popleft())`
 ```python
 queue = deque('123244')
 
 right_rotate = queue.rotate(1)
 left_rotate = queue.rotate(-1)
 
 print(right_rotate)
 print(left_rotate)
 ```
 ```python
 deque(['4', '1', '2', '3', '2', '4'])
 deque(['2', '3', '2', '4', '4', '1'])
 ```

#### Source
[deque 对象](https://docs.python.org/zh-cn/3/library/collections.html#deque-objects)

[詳解Python的collections模塊中的deque雙端隊列結構](https://www.itread01.com/articles/1476167111.html)

[📝](https://github.com/vanikk06/Data-structures-and-Algorithms/tree/master/week_12#content)


# Dict & Set
 > 字典（dictionary）與集合
 
- [Dict](https://github.com/vanikk06/Data-structures-and-Algorithms/tree/master/week_12#dict)
- [Set](https://github.com/vanikk06/Data-structures-and-Algorithms/tree/master/week_12#set)

兩者差別在於「是否有儲存對應的value」

## Dict
 > 字典：關聯式陣列 or hash table
 >> {'key'：'value'}：映射類型\
 用空間換取時間
 
一個key僅能對應一個value，有極快的查找速度（不會隨著字典大小增加而變慢），但會使用大量的記憶體，內存浪費多

☆ key必須是**不可變對象**，因為dict根據key來計算value的存儲位置
 > Hash：通過key計算位置的演算法
 >> list是可變的，不能作為key，但tuple可以，因其不可更改
 
- `.update(x)`：合併 
- 使用`['key']`：進行key之查詢，回傳其對應的value
- `.keys()`：取所有鍵（key）
- `.values()`：取所有值（value）
- `.items()`：取所有鍵值對（key, value）
- `pop(key)`：刪除一個key，其對應的value也會隨之刪除
- 判斷key是否存在
   > 若key不存在，會出現`KeyError`
   
   - `in`
   - `.get('key')`：若key不存在，則回傳None或指定的值

## Set
 > 集合：建立一個無序、不重複的元素集
 >> 想像成僅留下key的dict

與tuple、list類似，但不同的是set內的元素是**不包含重複值＆無序的**

使用時機：
 1. 去除重複值
 2. 關係測試
     > e.g. 交集、聯集...
 3. 判斷元素是否已存在
     > 使用hsah的方式，查詢速度不會隨著set變大而變慢
     >> O(1)
     

- 宣告＆建立集合
  - `set1 = set()`：建立空的集合
    > ☆ 建立空集合的方法是`set1 = set()`而非`set1 = {}`
    >> {}表示空的dict
  - `set2 = {1, 2, 3, 4}`
- 基本操作
  - `.add()`：新增元素
  - `.remove()`：刪除元素
  - `in` ＆ `not in`：判斷元素是否已存在於集合中 
    > tuple、list皆可使用
  - `.copy()`：複製集合的副本
  - `.clear()`：刪除集合內所有元素
  - ☆ 無法使用[index]索引值來擷取特定元素
    > 集合元素是無序的，所以無法通過數字進行索引獲取某一個元素
- 關係判斷    
  - 聯集：A、B集合的所有元素
    - `.union()`
       > e.g. setA.union(setB)
    - `|`
       > e.g. setA|setB
  - 交集：A、B集合的共有元素
    - `.intersection()`
       > setA.intersection(setB)
    - `&`
       > setA & setB
  - 差集：A集合扣掉與B集合的共有元素
    - `.difference()`
       > setA.difference(setB)
    - `-`
       > setA - setB
  - 對稱差集：A、B集合的獨有元素，去掉兩合集共同部分
    - `.symmetric_difference()`：聯集-交集
       > setA.symmetric_difference(setB)
    - `^ `
       > setA ^ setB
  - 子集合：判斷A集合是否是B集合的子集
    - `.issubset()`
       > setA.issbuset(setB)
         - True：setA是setB的子集合
         - False：setA不是setB的子集合
  - 超集合：判斷A集合是否是B集合的父/母集
    - `.issuperset()`
       > setA.issuperset(setB)
         - True：setA是setB的超集合
         - False：setA不是setB的超集合
  - 互斥集合：判斷A集合與B集合是否互斥（沒有交集，沒有任何元素是一樣的）
    - `.isdisjoint()`
       > setA.isdisjoint(setB)
       >> disjoint：互斥的概念
         - True：setA與setB互斥
         - False：setA不與setB互斥
       



#### Source
[Python 學習使用集合 (Set)](https://wenyuangg.github.io/posts/python3/python-set.html)

[用 Python 自學程式設計：list、tuple、dict and set](https://blog.kdchang.cc/2017/08/15/learning-programming-and-coding-with-python-list-tuple-dict-set/)

[帶你搞清楚python中的dict和set的用法](https://kknews.cc/zh-tw/news/m29zo56.html)

[Python中集合(set)的基本操作以及一些常見的用法](https://blog.51cto.com/10616534/1944841)

[Python set(集合) 這一定是最全的介紹集合的博文](https://blog.csdn.net/Solo95/article/details/78753265)

[🖌](https://github.com/vanikk06/Data-structures-and-Algorithms/tree/master/week_12#content)

# Level-Order Traversal

各個node相對於root有其對應的level，按照**level由小到大**的順序對node進行走訪

![](https://github.com/vanikk06/Data-structures-and-Algorithms/blob/master/week_12/image/1576400035124.jpg)

#### Source
[Binary Tree: Traversal(尋訪) - Level-Order Traversal](http://alrightchiu.github.io/SecondRound/binary-tree-traversalxun-fang.html#level)

[✏](https://github.com/vanikk06/Data-structures-and-Algorithms/tree/master/week_12#content)

# Graph
  > 圖：由**點＆線**組成
  
在「排程」、「派送」、「設基地台」等相關問題，就會需要graph走訪
  
- 定義  
  graph比tree更加廣義，不限制結構裡的node/vertex只能有唯一的parent
    > 優點：能以**關係**表示先後
    
  graph為所有的vertex/node與edge所組成之集合
    - vertex/node：資料節點 
    - edge：線段、連結
      > 實際上，是用一對vertex表示edge
      >> e.g.(v1, v2)：即為連結v1與v2的edge
    

      根據edge是否具有**方向性**
      - directed graph：edge具有方向性，及vertexA與vertexB之關係為**單向的**
        > 有向圖
      - undirected graph：edge不具有方向性，及vertexA與vertexB之關係為**雙向的**
        > 無向圖

- graph vs. tree

    與tree不同，可以有loop（迴路）
      > loop：可以繞回原點（root）

  - tree是graph的子集合
    - graph：由**點**跟**線**組成，即為graph
    - 在graph中砍掉loop的線，並且點有上跟下的關係，即為tree
      > tree被蘊含在graph中

![](https://github.com/vanikk06/Data-structures-and-Algorithms/blob/master/week_12/image/1576310235203.jpg)

紀錄graph方式：使用array＆linked list
  - array：點
    >　長度：所有的點數
  - linked list：鄰邊
    > 以線連接到的其他點
    
    > 不唯一，依據起點的不同，會有不同的linked list
    >> 鄰邊必須可以還原graph
    
#### Source
[Graph: Breadth-First Search(BFS，廣度優先搜尋)](http://alrightchiu.github.io/SecondRound/graph-breadth-first-searchbfsguang-du-you-xian-sou-xun.html)

[Graph: Intro(簡介)](http://alrightchiu.github.io/SecondRound/graph-introjian-jie.html)

[🖋](https://github.com/vanikk06/Data-structures-and-Algorithms/tree/master/week_12#content)

# Breadth-First Search
 > BFS：廣度優先搜尋，使用Level-Order Traversal
 >> 由起點開始，根據level一層一層走訪
 

 > traversal（走訪）graph之方法：關心**點**的問題
 >> search（搜尋）的必要功能
 
 從起點開始，遍歷完整個graph，按照**就近原則**進行，距離起點相同的vertex其訪問順序是相近的
 
 在edge是沒有weight的情況下，BFS可以確保vertexA到vertexB是最短路徑

 
 
#### § Step §

從某一vertex出發，先走訪所有**相鄰**的vertex，再依序走訪與已走訪過的vertex相鄰但尚未走訪的vertex，直到所有相鄰的vertex都被走訪

![](https://buracagyang.github.io/2019/07/14/breadth-depth-first-search/BFS.gif)

- Step1：從頂點開始，即Level 0
- Step2：查找頂點以單線連接到的所有其他點，這些點即為Level 1
- Step3：假設所有Level 1的點為頂點，查找所有與Level 1以單線相連的其他點，這些點即為Level 2
- Step4：重複上述步驟，直到所有點都被走訪
  
#### § Practice §
 > by Queue
 
 > 當起點與鄰邊表相同時，走訪方式唯一

![](https://github.com/vanikk06/Data-structures-and-Algorithms/blob/master/week_12/image/1576316336720.jpg)
> 無向圖：雙向，無指定特別方向，只要有線連結就要記錄

> graph資料結構：塞入資料就好
>> 無規定由大到小或由小到大：不唯一

使用queue紀錄print出的點所連接的其他點

![](https://github.com/vanikk06/Data-structures-and-Algorithms/blob/master/week_12/image/output_aMJs9Q.gif)
> Queue中灰色部分，表示當次被提取的值

使用兩個array，一個處理queue，一個紀錄print出的順序

#### Source
[圖片來源](https://buracagyang.github.io/2019/07/14/breadth-depth-first-search/)

[Breadth First Search (BFS) Algorithm](https://www.javatpoint.com/breadth-first-search-algorithm)

[Breadth first search](https://www.programiz.com/dsa/graph-bfs)

[橫向優先搜尋 (breadth-first search)](http://nthucad.cs.nthu.edu.tw/~yyliu/personal/nou/04ds/bfs.html)

[BFS & DFS 學習整理](https://codertw.com/%E7%A8%8B%E5%BC%8F%E8%AA%9E%E8%A8%80/102866/#outline__1_1)

[🖊](https://github.com/vanikk06/Data-structures-and-Algorithms/tree/master/week_12#content)

# Adjustment of Design BFS
 > 優化作業五BFS程式碼，

原始BFS程式碼  [👉🏼HERE👈🏼](https://github.com/vanikk06/Data-structures-and-Algorithms/blob/master/week_12/Design%20BFS.py)

在原始程式碼中
- 使用4個array
- 判斷vertex是否已進入處理（已走訪 or 在queue中待處理）：是否已存在array中

#### Code
調整後BFS程式碼 [👉🏽HERE👈🏽](https://github.com/vanikk06/Data-structures-and-Algorithms/blob/master/week_12/Adjustment%20of%20Design%20BFS.py)

將重複對鄰邊表查詢的步驟放在一起，簡化程式碼，並將儲存已處理的vertex之資料類別改為set

- 使用3個array、1個set、1個變數
- 判斷vertex是否已進入處理（已走訪 or 在queue中待處理）：是否已存在set中

#### 差異

 進行「是否已存在」的判斷時，使用set會比array快
   - set：使用hsah的方式
     > 時間複雜度為O(1)
   - array：與array內的元素一個個比較
     > 時間複雜度為O(n)，n為array內元素個數
 
P.S.此處沒有使用`defaultdict`套件，當input的key不存在字典內時，會出現`KeyError` 

#### Source
[BFS和DFS算法（第2讲）](https://www.youtube.com/watch?v=bD8RT0ub--0&t=)

[Python判斷值是否在list或set中的效能對比分析](https://codertw.com/%E7%A8%8B%E5%BC%8F%E8%AA%9E%E8%A8%80/369163/)

[🖍](https://github.com/vanikk06/Data-structures-and-Algorithms/tree/master/week_12#content)

