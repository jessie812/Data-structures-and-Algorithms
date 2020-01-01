# Content

# Notes

- graph：有起點、終點、cost
- `addEdge`：新增edge，加上權重
    > idea：使用dict紀錄
    - u：起點
    - v：終點
    - w：權重
- 測資皆為正整數

Dijkstra先建一個權重的matrix（index由由0開始，為nxn矩陣）
  > 回傳值：input到每vertex的最短距離
  >> type dictionary

Kruskal建立edge不侷限用defaultdict
   > 回傳值：從最小的weight開始，起點-終點（小-大）

# Error

- TyprError
   > `unsupported operand type(s) for +: 'int' and 'str'`
   >> 類型錯誤：不支持操作類型為整數和字串
   
    `+`：符號操作，可分兩類
    - 數學運算符：在int、float等數字類型資料之間進行加法的操作
    - 字串連接：對str的資料類型進行連接的動作
    
    在`+`出現在**既有數字類型資料又有字串類型資料**的情況下，python會不知道是要進行數字運算還是字串連接的動作

#### Source
[Python初學者錯誤：TypeError: unsupported operand type(s) for +: 'int' and 'str'](https://blog.csdn.net/foryouslgme/article/details/51536882)


 - ⭐assignment⭐
    > 指派、賦值
   
   在python中`=`代表的是一個指派、賦值的動作，讓變數在記憶體中定址\
   當執行兩個變數相互指派的程式碼時，代表的是讓**兩個變數定址在相同的記憶體位置**
   ```python 
   a = [3]
   b = []
   
   b = a
   b.append(7)
   
   print('a = ', a)
   print('b = ', b)
   print( a is b )
   ```
   因為是在相同的記憶體位置，因此對其中一個變數作出變動時，會直接影響到另一個變數
   ```python
   a =  [3, 7]
   b =  [3, 7]
   True
   ```
   如果想要將a變數的值儲存到b變數內，但不想讓對b變動的變動影響到a變數的話，可以使用`.copy()`函式
    ```python 
   a = [3]
   c = []
   
   c = a.copy()
   c.append(8)
   
   print('a = ', a)
   print('c = ', c)
   print( a is c )
   ```
   結果
   ```python
   a =  [3, 7]
   c =  [3, 7, 8]
   False
   ```

# Code

使用`collections.defaultdict`套件

- `__init__`：基本屬性
    - .V：vertex個數
    - .graph：Dijkstra使用之adjacency matrix
        > 各個vertex之間的weight
        >> 0：未連結
    - .graph1：Kruskal使用之字典，資料類型為`defaultdict`，參數為int
    - .graph_matrix：Dijkstra使用之紀錄已檢查過的cost的矩陣
        >必為V x V矩陣
        >> 預設值為-1

- `addEdge`：建立edge，有三個參數
    - u：src，起點
    - v：dest，終點
    - w：weight/cost，權重、成本
- `Dijkstra`：最短路徑
    > 有一個參數：起點
    - min_cost：最小cost，每新增一個vertex就會變動
        > 以此去尋找相對應的index（vertex）
    - min_index：此次最小cost的index，也就是下次要增加的vertex
    - fix_min_index：修復index，排除bug
        > 當cost出現相同值時，因尋找index的方式是採用`list.index()`，它會回傳第一個相符值的index
    - fix_time：修復次數
        > 原始list中的index = fix_min_index + fix_time
        >> 因修復方法為「刪除第一個相符值」讓`list.index()`找到此次應增加的vertex\
        因刪除會使index產生變動，且第一個相符值必定存在於第二個相符值的前面\
        因此，將「修復次數」加上「修復後找到的index」即可得出原始正確的index
    - checked：紀錄已找到最小cost的vertex
    
    先設定一些小工具（min_cost、fix_min_index、fix_time、checked），方便等一下操作
    
    將起點連結到的vertex紀錄到graph_matrix上\
    使用`for`迴圈，一個個查看起點在graph中與其他vertex的連結情況
        > 將自己與自己的cost紀錄為0
        - 如果vertex與起點有連結 且
    


