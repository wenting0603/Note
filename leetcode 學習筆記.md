# leetcode 學習筆記
## EAZY
### 1. Two Sum
找出陣列中的兩數其總和為target的index
![](https://i.imgur.com/NYfAIwH.png)
solve:

```bash=
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        for i in range(len(nums)):
            for j in range(i+1, len(nums)):
            ###2個for迴圈的巢狀結構，使每一個array中的數值都能互相比較
            ###可視為nums[0]與nums[1:]跑完後
            ###會接續跑nums[1]與nums[2:] 
            ###直到nums這個array全部跑完迴圈
                if nums[i] + nums[j] == target:
                ###因每個職都會倆倆被抓出來，所以就可以知道哪兩個鄉嘉惠等於target
                    return [i,j]
```
![](https://i.imgur.com/wnyYS2U.png)


### 9. Palindrome Number
判斷是否為回文
![](https://i.imgur.com/EMdPZpr.png)

solve:

```bash=
class Solution:
    def isPalindrome(self, x: int) -> bool:
       return False if x < 0 else x == int(str(x)[::-1])
       #先判斷 input有內容
       #後判斷是否為回文
          ###用array倒過來是否等於原本array來辨別
          ###array[::-1]是將array倒過來
          ###先將nums的性質為字串(字串可適用array)，倒過來之後
          #再化為數值來做比較
```
![](https://i.imgur.com/YBuiRDU.png)


### 13. Roman to Integer
轉換為羅馬數字

![](https://i.imgur.com/C7df13n.png)

```bash=
class Solution:
    def romanToInt(self, s: str) -> int:
        dic= {'I':1,'V':5,'X':10,'L':50,'C':100,'D':500,'M':1000}
        #做一個比照表
        num = 0
        #處理I can be placed before V (5) and X (10) to make 4 and 9.的規則
        #想法1:前面的數字比後面的小，就會造成後面的數字減小
            # IV  I =1<V =5  則轉換為4(5-1) 
            # XC  X=10<C=100 則轉換為90(100-10)
        #想法2.:前面的數字比後面的大，就會與後面的數字相加
            ##VI  V =5>I=1  則轉換為6(5+1)
            # CX  C=100>X=10 則轉換為110(100+10)
        for i in range(len(s)-1):
            if dic[s[i]]>= dic[s[i+1]]:
                num += dic[s[i]]
                
            if dic[s[i]]< dic[s[i+1]]:
                num -= dic[s[i]] 
        num += dic[s[-1]] ##最後一個數值沒被放入迴圈中，需另外加上       
        return num    
```


### 14. Longest Common Prefix
找出最長的相似字串

![](https://i.imgur.com/F2hpQ2Z.png)

```bash=
class Solution:
    def longestCommonPrefix(self, strs: List[str]) -> str:
        #縮小範圍
        smaller = min(strs, key = len)
        
        for i in range(len(smaller),-1,-1):#range(start, stop, step]) 
        #即為從最後一個數字開始依次遞減1
        #原本會從0開始，現在倒過來，是為了找到最大相似字串，若從最小開始則會於有相同字串時就停止
            if all(s.startswith(smaller[:i])for s in strs):
            #str.startswith(str, beg=0,end=len(string))
            #for 迴圈將strs中的每一組字串取出
            #再利用staetswith檢查字符串是否是以指定子字符串開頭
            #all用來確認都是true
                    return smaller[:i]
        else:
            return ''
```
![](https://i.imgur.com/b5gqohI.png)


### 20. Valid Parentheses
給定一個s僅包含字符'(', ')', '{', '}', '['and']'的字符串，確定輸入字符串是否有效。

輸入字符串在以下情況下有效：

1.開括號必須被同類型的括號封閉。
2.左括號必須以正確的順序關閉。
![](https://i.imgur.com/Fj6Vham.png)

solve:

```bash=
class Solution:
    def isValid(self, s: str) -> bool:
        dic = {'(':')','{':'}','[':']'}
        stack = []
        
        for char in s:
            if char in dic:
                stack.append(char)
                #先將右邊存起來
            elif (not stack) or (char != dic[stack.pop()]):
                
                #1.一定要有有編，若無則not  stack->False
                #2.來看左邊是否配對
                    #char != dic[stack.pop()] 不配對 ->False
                    #stack.pop()會把top拿掉，這樣全部跑完後stack == [] (裡面沒東西了)
                
                return False
       
        return stack == [] #當 stack為空->TRUE
```
![](https://i.imgur.com/qmEGZFF.png)
https://ithelp.ithome.com.tw/articles/10217603
![](https://i.imgur.com/vc0WBnX.png)

### 21. Merge Two Sorted Lists
將兩個list 合併且按照順序排
#### 目的:了解Linked List
資料與資料連接的方式是單向的，
通常從第一個節點(node)往後連接到最後一個節點(node)。
每個節點上會存放一個(或一組)資料以及連結(link)，
每個連結會指向到下一個節點的位置(記憶體位址)。
https://ithelp.ithome.com.tw/articles/10213265

![](https://i.imgur.com/Eu4xURH.png)

solve:
 遞迴(recursion) 

```bash=
class Solution:
    def mergeTwoLists(self, l1: Optional[ListNode], l2: Optional[ListNode]) -> Optional[ListNode]:
        if not l1 and not l2:
            return None
        if not l1 or not l2:
            return l1 or l2
        #確認兩個list存在，都不存在就回復NONE 只有其一存在則回復其
        if l1.val<l2.val:
            return ListNode(val = l1.val, next=self.mergeTwoLists(l1.next,l2))
            #l1.val較小時(排在前面) l1.next 就要呼叫自己 MergeTwoLists(l1.next, l2) 去比較 l1.next 及 l2 的大小並串接
        else:
            return ListNode(val = l2.val, next=self.mergeTwoLists(l2.next,l1))   
```
![](https://i.imgur.com/HPzQrqG.png)


### 26. Remove Duplicates from Sorted Array
去除array中重複的值後，回復其大小

![](https://i.imgur.com/5B2n3pF.png)

```bash=
class Solution:
    def removeDuplicates(self, nums: List[int]) -> int:
        nums_set = set(nums)#set集合不會包含重複的資料
        nums.clear()#把nums清空，因為題目要求要返回整理完的nums
        nums += list(nums_set)#把已經無重複值的nums_set加回去
        nums.sort()#按照大小排
        
        return len(nums)    
```


### 27. Remove Element
將指定的元素拿掉後返回其array大小

![](https://i.imgur.com/5lMbbeK.png)

1.
```bash=
class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        while val in nums:
            nums.remove(val)
            
        return len(nums)
```

2.
```bash=
class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        i = 0
        l = len(nums)
        
        while i < l:
            if nums[i] == val:
                nums[i] = nums[l-1]
                l -= 1
            else:
                i +=1
        return l
```

### 155. Min Stack
設計一個class 可以 push, pop, top和回復最小值的stack

solve

```bash=
class MinStack:

    def __init__(self):
        self.A = []#儲存stack中的元素
        self.M = []#追蹤最小值，因stack的特性是加上和取出都是從最上面(top)，因此要另外開一個stack來保證最小值一直在top
        

    def push(self, val: int) -> None:#在stack加上值
        self.A.append(val)#直接加上去
        M = self.M
        M.append(val if not M else min(val,M[-1]))#要確保M stack最上面的值是最小
            
        

    def pop(self) -> None:#刪除stack最上面的值並返回
        self.A.pop()
        self.M.pop()
        
    def top(self) -> int:#回復stack最上面的值
        return self.A[-1]
        

    def getMin(self) -> int::#回復stack最小的值
        return self.M[-1]
        


# Your MinStack object will be instantiated and called as such:
# obj = MinStack()
# obj.push(val)
# obj.pop()
# param_3 = obj.top()
# param_4 = obj.getMin()
```

### 167. Two Sum II - Input Array Is Sorted
在生序的array裡找到兩個相加等於target的index並將其以List的方式呈現
相較於第1題，array的size大很多
因此不能使用巢狀迴圈效能過低的方式來解決
![](https://i.imgur.com/ZJ2PwOf.png)

與第一題相同解法造成效能過低:
```bash=
class Solution:
    def twoSum(self, numbers: List[int], target: int) -> List[int]:
        for i in range(len(numbers)-1):
            for j in range(i+1,len(numbers)):
                if numbers[i]+numbers[j]==target:
                    return [i+1,j+1]
        
```
![](https://i.imgur.com/sBGNxJU.png)


solve1:
將兩個for迴圈拆解開來
```bash=
class Solution:
    def twoSum(self, numbers: List[int], target: int) -> List[int]:
        left,right =0,len(numbers)-1#一個從頭開始，一個從尾開始
        
        while left<right:#確保是前後相加
            num = numbers[left]+numbers[right]#看相加為多少去跟target比較
            
            if num ==target:
                return [left+1,right+1]
            elif num > target: #若比較大就讓後面的往前加
                right -=1
            else:#num < target: 若比較小就讓前面的往後移
                left +=1
        #while迴圈會跑到相加等於targe (第一個if達成)且保持前加後(因相交後就會停止)
```
![](https://i.imgur.com/nscz9sT.png)
解法簡單但效能偏低


solve2:
使用到enumerate():
```bash=
>>>seasons = ['Spring', 'Summer', 'Fall', 'Winter']
>>> list(enumerate(seasons)#,star=0)
[(0, 'Spring'), (1, 'Summer'), (2, 'Fall'), (3, 'Winter')]
```
為了提取idex以及內容的組合
```bash=
class Solution:
    def twoSum(self, numbers: List[int], target: int) -> List[int]:
        d = {v:i for i,v in enumerate(numbers)}
        #建立一個字典用來對照用key去找value
        #v為numbers中的內容 在此作為d的key
        #i為numbers的index 在此作為d的value
        for i in range(len(numbers)):
            if target-numbers[i] in d:#若d的key中有與相符的值
                if i != d[target-numbers[i]]:#且不是自己加自己 d[target-numbers[i]]numbers的等於index
                    return [i+1,d[target-numbers[i]]+1]#則可以返回答案
                    
        #兩個if會增加號實彈減少記憶體
        # if target-numbers[i] in d and  i != d[target-numbers[i]]:
        #Runtime: 68 ms, faster than 41.60% 
        #Memory Usage: 14.5 MB, less than 87.14%     
```

![](https://i.imgur.com/rfMYsBE.png)

### 168. Excel Sheet Column Title
將數字轉換成英文

![](https://i.imgur.com/EUm45wK.png)

#### 概念是看26的幾次方是數字幾
#### AB = A×26¹＋B ＝ 1×26¹＋2
#### ABCD＝A×26³＋B×26²＋C×26¹＋D＝1×26³＋2×26²＋3×26¹＋4
因此就要不斷的除26看餘數是多少

solve1: 使用list創建A-Z的表
```bash=
class Solution:
	def convertToTitle(self, columnNumber: int) -> str:
		sheet = [chr(i) for i in range(ord("A"), ord("Z")+1)]
        #ord() 函數是chr() 函數（對於8位的ASCII字符串）的配對函數,會返回對應的ASCII數值
        #chr()返回一個對應的ASCII字符
        #這樣建立A-Z的表
		res = []
		while columnNumber > 0:
        #columnNumber-1 以便餘數範圍從 (0,25) 給我們我們需要的 26 個字母
        #從最後一個餘數開始轉換
			res.append(sheet[(columnNumber-1) % 26])
			columnNumber = (columnNumber-1)//26#不斷的迴圈來看此數值是26的幾次方
		return "".join(res[::-1])
        #從最後一個開始轉換，因此需整個倒過來
```
![](https://i.imgur.com/ubIzJOW.png)

solve2: 使用ASCII轉換為字符
```bash=        
class Solution:
    def convertToTitle(self, columnNumber: int) -> str:
        s= ""
        while columnNumber > 0 :
            columnNumber,rem = divmod(columnNumber-1,26)
            #divmod ( a , b )) 函數把除數和余數運算結果結合起來
            #a/b 返回(商,餘數)
            s += chr(65+rem)
            #將餘數+65 因為ASCII 65=A
        return s[::-1]
```
![](https://i.imgur.com/DMLy4n9.png)
##### 前兩種方法記憶體使用率沒有差很多


solve3:使用遞迴
```bash=  
class Solution:
    def convertToTitle(self, columnNumber: int) -> str:
        
        if columnNumber==0:
            return ''
        
        q,r=divmod(columnNumber-1,26)
        
        return self.convertToTitle(q)+chr(r+ord('A'))
        ##持續地回直到沒有26的次方倍(columnNumber==0)就不會繼續加了
```
![](https://i.imgur.com/f6zEdOr.png)
記憶體使用率降低

### 169. Majority Element
給定一個nums大小的數組n，返回多數元素。

多數元素是出現⌊n / 2⌋多次的元素。您可以假設多數元素始終存在於數組中。

#### 因為出現最多次的元素次數會是 array的一半大小，所以表示在升序的排列下，第n / 2 個後都是該元素
```bash=  
class Solution:
    def majorityElement(self, nums: List[int]) -> int:
        nums.sort()
        return nums[len(nums)//2]
                
```
![](https://i.imgur.com/zNvbKDa.png)


### 171. Excel Sheet Column Number
將英文轉為數字與 #168 相似
#### AB = A×26¹＋B ＝ 1×26¹＋2
#### ABCD＝A×26³＋B×26²＋C×26¹＋D＝1×26³＋2×26²＋3×26¹＋4

![](https://i.imgur.com/WRHBsQP.png)


```bash= 
class Solution:
    def titleToNumber(self, columnTitle: str) -> int:
        ans = 0
        for i,c in enumerate(reversed(columnTitle)):
        #把columnTitle倒過來之後i作為26的幾次方 c作為數字的乘積
            ans += (ord(c)-64)*(26**i)
            #ord(c)將英文轉為ASIII的數字 但要減去64 因A=65
            
        return ans
```
效能滿差的，應再去尋找更好的解法
![](https://i.imgur.com/X5QM9AO.png)

### (SQL)175. Combine Two Tables
用MYSQL將兩個TABLE合併
![](https://i.imgur.com/7vP5iLR.png)
用personID將 Person和 Adress table合併 並顯示 first name, last name, city, and state資訊,若無state則顯示null

#### table1 left join table2 on table1.key =table2.key
以key作為合併的標準

```bash= 
# Write your MySQL query statement below
select firstName, lastName, city, state 
from Person left join Address 
on Person.personId = Address.personId
```
![](https://i.imgur.com/vndCNq9.png)

### (SQL)181. Employees Earning More Than Their Managers
找出table中下屬薪水比主管高的
![](https://i.imgur.com/1uN1a42.png)

需要條件判斷
##### where
1.先確定有主管
2.再比較薪水

```bash= 
# Write your MySQL query statement below
SELECT a.Name as Employee 
FROM Employee as a, Employee as b
WHERE a.ManagerId = b.Id 
  AND a.Salary > b.Salary;
```
![](https://i.imgur.com/pJoiVxE.png)

### (SQL)182. Duplicate Emails
找出重複的email

![](https://i.imgur.com/SZveFOY.png)
```bash= 
# Write your MySQL query statement below
select Email
from Person
group by Email
having count(Email) >1
```
![](https://i.imgur.com/wTV081P.png)


### (SQL)183. Customers Who Never Order
找出未點餐的客人

```bash=
# Write your MySQL query statement below
select Customers.name as customers
from Customers left join Orders
on Customers.id = Orders.customerId
where Orders.customerId is null 
```

![](https://i.imgur.com/P5MViHr.png)

```bash=
# Write your MySQL query statement below

select customers.name as 'Customers'
from customers
where customers.id not in
(
    select customerid from orders
);
```
![](https://i.imgur.com/tvR3mQp.png)

### 190. Reverse Bits
給一個32 bits的int整數，反轉整數的bits
範例： 整數43261596 轉換成bits = 00000010100101000001111010011100，將bit反轉00111001011110000010100101000000再轉成整數964176192回傳

solve:
```bash=
class Solution:
    def reverseBits(self, n: int) -> int:
        ans =0
        for _ in range(32):
            ans, n = ans*2+n%2, n>>1
        return ans
```
![](https://i.imgur.com/cV0L3oj.png)


### 191. Number of 1 Bits
找到二進位中有1的次數
![](https://i.imgur.com/oWdVA2A.png)

##### 視為字串，使用count去尋找特定自符
solve:
```bash=
class Solution:
    def hammingWeight(self, n: int) -> int:        
        return bin(n).count("1")#bin()是返回一個整數in二進位的字符串
        
```
![](https://i.imgur.com/hDc0xel.png)




### (SQL)196. Delete Duplicate Emails
將重複的EMAIL刪除並保留數字小的ID
![](https://i.imgur.com/QMT6JmZ.png)

#### 刪除重複值＆較大的ＩＤ
solve:
```bash=
# Write your MySQL query statement below
Delete p1 
From Person p1,Person p2
Where p1.Email = p2.Email 
And p1.id > p2.id
        
```

![](https://i.imgur.com/DQOAFfi.png)


### (SQL)197. Rising Temperature

列出氣溫比前一天高的ID
![](https://i.imgur.com/Z9effHP.png)

solve1:
#### DATEDIFF() 會返回 expr1 − expr2，即兩個日期相減差幾天
#### 以差1天的日期作為key，合併起來後比大小
```bash=
# Write your MySQL query statement below
SELECT w1.id AS 'id'
FROM weather w1
JOIN weather w2 
ON DATEDIFF(w1.recordDate,w2.recordDate)=1
AND w1.temperature > w2.temperature
;
```

![](https://i.imgur.com/2rXFoCg.png)

solve2:
#### INTERVAL 1 DAY 作為時間間格
#### 用條件判斷式
```bash=
# Write your MySQL query statement below
SELECT w1.id
FROM weather w1, weather w2 
WHERE  w1.recordDate = w2.recordDate + INTERVAL 1 DAY
AND w1.temperature > w2.temperature
;
```
![](https://i.imgur.com/We3eyLN.png)
運行時間較久

### 202. Happy Number
若是n的個別數字拆解開的平方和最後等於1則為happy number

![](https://i.imgur.com/nPaoKPu.png)

solve1:
#### 先定義一個將平方和相加的function
#### 再來判斷循環最後是否會等於1
##### 平方和後的數字不能相同，不然就進入死循環

```bash=

class Solution:
    def isHappy(self, n: int) -> bool:
        ##定義數字平方和    
        def get_next(n):  
            sum = 0
            while n >0:
                n,d =divmod(n,10)
                sum += d**2
            return sum
        ##
        
        seen = set()
        ##在N 不等於一時不斷循環
        ##但若是數字有重複出線就停止
        while n != 1 and n not in seen:
            seen.add(n)
            n = get_next(n)
            
        return n ==1
```
![](https://i.imgur.com/21wWMCd.png)

solve2:
#### 使用遞迴
#### 將N視為字串個別計算相加
```bash=

class Solution:
    def isHappy(self, n: int) -> bool:
        new_n = 0  
        for i in str(n):        
            new_n += int(i)**2  
        if new_n == 1: return 1 
		
        return self.isHappy(new_n) if new_n != 4 else False
        ###當new_n == 4時會進入死循環，因此要替除掉，讓循環能跑到等於1獲釋返回false
```
### 205. Isomorphic Strings
判斷兩個字串的結構是不是一樣
![](https://i.imgur.com/AYL2gmC.png)
solve:
#### 不重複的字符數量也要一樣，且配對的數量也要一樣
#### Zip()會將對像中對應的元素打包成一個個元組
a =[1,2,3]
b = [5,7,9]
list(zip(a,b)=[(1, 5), (2, 7), (3, 9)]
```bash=
class Solution:
    def isIsomorphic(self, s: str, t: str) -> bool:
        d = set(zip(s, t))#將倆的字串配對，並去除重複的
        return len(d) == len(set(s)) == len(set(t))
        #若是配對與兩個不重複字串長度相同，表示其結構一致
```
![](https://i.imgur.com/xaPL9l6.png)


### 217. Contains Duplicate

判斷array中是否有重複的值

solve1:
建立一個字典存放array中的值
若有對應相同的值->TRUE

```bash=
class Solution:
    def containsDuplicate(self, nums: List[int]) -> bool: 
        d = {}
        for i in nums:
            if i in d:
                return True
            else:
                d[i] = 1#無對應就存入字典中
        return False  
```
![](https://i.imgur.com/WNHt3fn.png)

solve2:
將array set 成為一個不重複的新陣列
比較前後的長度是否相同

```bash=
class Solution:
    def containsDuplicate(self, nums: List[int]) -> bool: 
        array = set(nums)
        if len(array)!=len(nums):
            return True
        else:
            return False
```
![](https://i.imgur.com/ghSQIpF.png)

### 219. Contains Duplicate II
判斷array是否重複
且兩個重複值的index小於K
![](https://i.imgur.com/f7FSBeM.png)

solve:
用長度判斷:
```bash=
class Solution:
    def containsNearbyDuplicate(self, nums: List[int], k: int) -> bool:
        if len(nums)==len(set(nums)):
            return False
        for i in range(len(nums)):
            if len(nums[i:i+k+1]) != len(set(nums[i:i+k+1])):
                return True
        return False
```
![](https://i.imgur.com/osJfYAb.png)

### 225. Implement Stack using Queues
把LIST用字定義的function化為stack的形式

![](https://i.imgur.com/xlyN3ju.png)


#### collections模塊中的deque雙端隊列結構
##### deque 是 double-ended queue的縮寫，類似於 list，不過提供了在兩端插入和刪除的操作。

appendleft 在列表左側插入
popleft 彈出列表左側的值
extendleft 在左側擴展

```bash=
class MyStack:

    def __init__(self):
  
        self.q = collections.deque()
        #將q化為雙端列隊結構 類似stack
    

    def push(self, x: int) -> None:
        self.q.append(x)
    

    def pop(self) -> int:

        for i in range(len(self.q)-1):
            self.q.append(self.q.popleft())
        return self.q.popleft()
	
            #or just do
			
    #return self.q.pop()
    

    def top(self) -> int:

        return self.q[-1]
    

    def empty(self) -> bool:

        return len(self.q)==0


# Your MyStack object will be instantiated and called as such:
# obj = MyStack()
# obj.push(x)
# param_2 = obj.pop()
# param_3 = obj.top()
# param_4 = obj.empty()
```
![](https://i.imgur.com/R3AgqAV.png)

### 228. Summary Ranges
將array中有連續的數字用->連接 若無則呈現原本數字

![](https://i.imgur.com/XCwASMG.png)
#### f-strings 格式化字串
###### 當變數要換為字串時的好方法
可以讓字串直接在f""中直接連接變數並以字串的方式顯示
###### f"字串{變數}字串"

```bash=
>>> name = "Eric"
>>> age = 74
>>> f"Hello, {name}. You are {age}."
'Hello, Eric. You are 74.
```

solve:
用兩個變數，一個做為內部變動的數值，一個作為外部的變數
這樣可以在一個保持原狀(外部)，另一個往下(內部)的狀態來比較是否連續。如:
1->2， 1->3， 1->4 ....

```bash=
class Solution:
    def summaryRanges(self, nums: List[int]) -> List[str]:
        if not nums:
            return []
        
        ans = []
        i = 0##i作為外部變數
        while i < len(nums):
            j=i##j作為內部變數
            while j+1 < len(nums) and nums[j+1]-nums[j] ==1:
                j+=1#當是連續的數值時J會往下加
                
            if j == i:
                ans.append(f"{nums[i]}")
                ##不連續時則覆蓋上原本的數值
            else:
                ans.append(f"{nums[i]}->{nums[j]}")
            
            i = j+1
            #外部變數再判斷完是否連續後，才會往部不連續的數值再繼續判讀
            
        return ans
```

![](https://i.imgur.com/RjcUzzk.png)

### 231. Power of Two
判斷是否為２的倍數

![](https://i.imgur.com/X5TzAyo.png)


solve1:
用數學邏輯推算:
能被二一直整除的就是2的倍數
因此使用while n除2的餘數為零來進行

```bash=
class Solution:
    def isPowerOfTwo(self, n: int) -> bool:
        if n <= 0:
            return False
        while n % 2 ==0: n/=2
        # n除2的餘數為零時 將N除2後的數值再存入N
        return n ==1#若為2的倍數除到最後一定是1 否則就不是2的倍數
```
![](https://i.imgur.com/s9KOdUB.png)
效能超低

solve2:
拆開來寫
使用while n除2的餘數不為零來判斷

```bash=
class Solution:
    def isPowerOfTwo(self, n: int) -> bool:
        if n <= 0:
            return False
        elif n ==1:
            return True
        else:
            while n>1:
                if n % 2 != 0:
                    return False
                n = n //2
        return True
```
![](https://i.imgur.com/vOnbQ9t.png)
效能略為上升

### 290. Word Pattern
給定一個模式與一格字串
判斷字串格式是否與給定之模式相同
![](https://i.imgur.com/ssFsRca.png)

solve
首先要建立字典，才能做key-value的對應比較
```bash=
class Solution:
    def wordPattern(self, pattern: str, s: str) -> bool:
        sp = s.split(" ")#將字串化為List
        pl = len(pattern)
        di = {}
        if pl != len(sp):return False
        for i in range(pl):
            if pattern[i] not in di:
                if sp[i] not in di.values():
                    di[pattern[i]] = sp[i]
                    '''若字典中沒有存在key-value的配對
                    則將此配對存入'''
                else:
                    return False
                    #若是不存在該key但有該value就表示模式不配對
            else:#存在此key就須看是否為正確配對的value
                if di[pattern[i]] != sp[i]:
                    return False
        return True
```

