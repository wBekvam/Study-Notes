## (LeeCode)13、罗马数字转整数

## 题目描述

罗马数字包含以下七种字符

| 字符 | 数值 |
| ---- | ---- |
| I    | 1    |
| V    | 5    |
| X    | 10   |
| L    | 50   |
| C    | 100  |
| D    | 500  |
| M    | 1000 |

例如， 罗马数字 2 写做 II ，即为两个并列的 1。12 写做 XII ，即为 X + II 。 27 写做  XXVII, 即为 XX + V + II 。

通常情况下，罗马数字中小的数字在大的数字的右边。但也存在特例，例如 4 不写做 IIII，而是 IV。数字 1 在数字 5 的左边，所表示的数等于大数 5 减小数 1 得到的数值 4 。同样地，数字 9 表示为 IX。这个特殊的规则只适用于以下六种情况：

- I 可以放在 V (5) 和 X (10) 的左边，来表示 4 和 9。
- X 可以放在 L (50) 和 C (100) 的左边，来表示 40 和 90。 
- C 可以放在 D (500) 和 M (1000) 的左边，来表示 400 和 900。

给定一个罗马数字，将其转换成整数。输入确保在 1 到 3999 的范围内。

**示例：**

| 输入  | 输出 |
| ----- | ---- |
| III   | 3    |
| IV    | 4    |
| LVIII | 58   |

## 解答

### 1、思路

按照题目描述：就是**把一个较小的值放在左边，就做减法，否则做加法。**

代码实现思路：往后多看一位，对比与当前位的大小关系，从而确定是加还是减。

### 2、Java

> 使用`HashMap`存储值，存在的问题是占用内存过大

```java
class Solution {
    public int romanToInt(String s) {
        HashMap<String, Integer> map = new HashMap<>();
        map.put("I", 1);
        map.put("V", 5);
        map.put("X", 10);
        map.put("L", 50);
        map.put("C", 100);
        map.put("D", 500);
        map.put("M", 1000);
        int sum = 0;
        int preSum = map.get(String.valueOf(s.charAt(0)));
        for (int i = 1; i < s.length(); i++) {
            int next = map.get(String.valueOf(s.charAt(i)));
            if (preSum < next) {
                sum -= preSum;
            } else {
                sum += preSum;;
            }
            preSum = next;
        }
        sum += preSum;
        return sum;
    }
}
```

> 量小的时候，使用`Switch`更高效
>
> 此方法借鉴而来：作者：donespeak

```java
class Solution {
    public int romanToInt(String s) {
        int sum = 0;
        int preNum = getValue(s.charAt(0));
        for(int i = 1;i < s.length(); i ++) {
            int num = getValue(s.charAt(i));
            if(preNum < num) {
                sum -= preNum;
            } else {
                sum += preNum;
            }
            preNum = num;
        }
        sum += preNum;
        return sum;
    }
    
    private int getValue(char ch) {
        switch(ch) {
            case 'I': return 1;
            case 'V': return 5;
            case 'X': return 10;
            case 'L': return 50;
            case 'C': return 100;
            case 'D': return 500;
            case 'M': return 1000;
            default: return 0;
        }
    }
}
```

### 3、JavaScript

```javascript
const romanInt = function (s) {
    let o = {
        'I': 1,
        'V': 5,
        'X': 10,
        'L': 50,
        'C': 100,
        'D': 500,
        'M': 1000
    }
    let num = 0;
    let preNum = o[s.charAt(0)]
    for (let i = 1; i < s.length; i++) {
        const next = o[s.charAt(i)]
        if (preNum < next) {
            num -= preNum;
        } else {
            num += preNum;
        }
        preNum = next;
    }
    num += preNum;
    return num;
}
```

