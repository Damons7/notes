#### **1.两数之和**

**给定 nums = [2, 7, 11, 15], target = 9，因为 nums[0] + nums[1] = 2 + 7 = 所以返回 [0, 1]**

```js
//1暴力
     for(let i = 0; i<_length;i++){
         for(let j = i ;j<_length;j++){
             if(nums[i]+nums[j] == target){
                 return [i,j];
            }
        }
    }
//2（map）
    let arrs =new Map()
    for(let i=0;i<nums.length;i++){
       if(arrs.has(target-nums[i])){
          return[arrs.get(target-nums[i]),i] 
       }
        arrs.set(nums[i],i)
    }

```

#### 2.无重复字符串

给定一个字符串，请你找出其中不含有重复字符的 **最长子串** 的长度。

```js

let substr = '',maxLength=0;
    let l=s.length;
    for(let i = 0;i<l;i++){   
         let findIndex = substr.indexOf(s[i]);
        	//假如碰到重复，则截掉重新拼接
            if(~findIndex){    
                substr = substr.slice(findIndex+1); 
            }    
            substr+=s[i];   
             maxLength = Math.max(maxLength, substr.length);
        }
        return maxLength;
};
```



#### 3.寻找两个正序数组的中位数

给定两个大小为 m 和 n 的正序（从小到大）数组 `nums1` 和 `nums2`。请你找出并返回这两个正序数组的中位数。

输入：nums1 = [1,2], nums2 = [3,4]
输出：2.50000
解释：合并数组 = [1,2,3,4] ，中位数 (2 + 3) / 2 = 2.5

```js
var findMedianSortedArrays = function(nums1, nums2) {
    //合并数组+sort排序，奇数偶数两种情况返回中位数
    const arr = nums1.concat(nums2);
        if(!arr){
            return 0;
        }
    const arr2 = arr.sort((a,b)=>a-b);
    let l =arr2.length;
    let l2 = Math.floor(l/2);
    return l%2==0?(arr2[l2]+arr2[l2-1])/2:arr2[l2]
};
```

#### 4.最长回文子串

给定一个字符串 `s`，找到 `s` 中最长的回文子串。你可以假设 `s` 的最大长度为 1000。

**示例 1：**

```
输入: "babad"
输出: "bab"
注意: "aba" 也是一个有效答案。
```

```js
//Manacher算法（中心扩散法的优化）
var longestPalindrome = function(s) {
    if(s.length<=1) return s;
    const newS = s.replace(/(.){1}/g,'#$1')+'#'; //将字符串每个字符前面加#，字符串最后也加一个#，主要用于避免奇偶数判断
    const len =newS.length;
    let max =1,res='';
    for(let i =1; i < len; i++){
        let max2=0;
        let temp =newS[i];
        for(let j =0 ;j < i; j++){
            //取此点，然后判断左右两边是否相同，（从中心向两边扩散）
            if(newS[i-j-1]==newS[i+j+1]){
                temp = newS[i-j-1]+temp+newS[i+j+1]; //拼接每次的结果
                max2 =temp.length;  //记录此时长度
            }
            else{
                break;
            }
        }
        //如果长度大于上一次保存，则更新最新的回文
        if(temp.length>max){
            max=max2;
            res = temp;
        }
    }
    //去除#
    return res.replace(/[#]/g,'');
    
   
    //暴力破解
    // const len = s.length;
    // let max = 0;
    // let res='';
    // for(let i =0;i<len;i++){
    //     let str = '';
    //     for(let j =i ;j<len ;j++){
    //         str+=s[j];
    //         if( str == str.split('').reverse().join('')){
    //             if(str.length>max){
    //                 res =str;
    //                 max = str.length
    //             }
    //         }
    //     }
    // }
    // return res
};
```

#### 6.Z字型变换

将一个给定字符串根据给定的行数，以从上往下、从左到右进行 Z 字形排列。

比如输入字符串为 "LEETCODEISHIRING" 行数为 3 时，排列如下：

L   C   I   R
E T O E S I I G
E   D   H   N

之后，你的输出需要从左往右逐行读取，产生出一个新的字符串，比如：`"LCIRETOESIIGEDHN"`。

**示例 1:**

```
输入: s = "LEETCODEISHIRING", numRows = 3
输出: "LCIRETOESIIGEDHN"
```

**思路：**

- 以 V 字型为一个循环, 循环周期为 n = (2 * numRows - 2) （2倍行数 - 头尾2个）。
- 对于字符串索引值 \color{red}ii，计算 x = i % n 确定在循环周期中的位置。
- 则行号 \color{blue}yy = min(x, n - x)。以 numRows = 4 为例，则 n = 6， 规律如下：



![image-20201229161745118](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201229161745118.png)

```js
var convert = function(s, numRows) {
    if (numRows === 1) return s;
        const rows = new Array(numRows).fill("");
        const n = 2 * numRows - 2;  //规律周期：2*行数-头尾两个
    for(let i = 0; i < s.length; i++) {
        const x = i % n;
        rows[Math.min(x, n - x)] += s[i];
    }
    return rows.join("");
  	  /* return numRows === 1 ? s : s.split('').reduce((p, 	v, i, a, l = numRows * 2 - 2) => (p[Math.min(i % l, l 	- i % l)] += s[i]) && p, new 		Array(numRows).fill('')).join(''); */
};
```

#### 7.整数反转

给出一个 32 位的有符号整数，你需要将这个整数中每位上的数字进行反转。

则其数值范围为 [−2的31,次 ，2的31次 − 1]

**示例 :**

```
输入: 123
输出: 321

输入: -123
输出: -321

输入: 120
输出: 21
```

```js
var reverse = function(x) {;
    const absVal = Number(`${Math.abs(x)}`.split('').reverse().join(''));  //取绝对值，转数组颠倒再转回字符串，最后Number转回数字
                           
  //判断是否大于2的31次和判断是否小于0和是否小于-2的32次
                           //2的31次幂写法：
                           //1.  2**31 
                           //2.  Math(2,31)
                           //3.  -(1<<31)
    return absVal > 2**31 ? 0     
        : x < 0 ? -absVal 
        : absVal;
};
```

#### 8.字符串转换整数 (atoi)

首先，该函数会根据需要丢弃无用的开头空格字符，直到寻找到第一个非空格的字符为止。接下来的转化规则如下：

- 如果第一个非空字符为正或者负号时，则将该符号与之后面尽可能多的连续数字字符组合起来，形成一个有符号整数。

- 假如第一个非空字符是数字，则直接将其与之后连续的数字字符组合起来，形成一个整数。

- 该字符串在有效的整数部分之后也可能会存在多余的字符，那么这些字符可以被忽略，它们对函数不应该造成影响。

  

  注意：假如该字符串中的第一个非空格字符不是一个有效整数字符、字符串为空或字符串仅包含空白字符时，则你的函数不需要进行转换，即无法进行有效转换。

在任何情况下，若函数不能进行有效的转换时，请返回 0 。

提示：

本题中的空白字符只包括空格字符 ' ' 。
假设我们的环境只能存储 32 位大小的有符号整数，那么其数值范围为 [−2的31,  2的31 − 1]。如果数值超过这个范围，请返回  INT_MAX (2的31 − 1) 或 INT_MIN (−2的31) 。

**示例 1:**

```
输入: "42"
输出: 42

输入: "   -42"
输出: -42
解释: 第一个非空白字符为 '-', 它是一个负号。
     我们尽可能将负号与后面所有连续出现的数字组合起来，最后得到 -42 。

输入: "4193 with words"
输出: 4193
解释: 转换截止于数字 '3' ，因为它的下一个字符不为数字。

输入: "words and 987"
输出: 0
解释: 第一个非空字符是 'w', 但它不是数字或正、负号。
     因此无法执行有效的转换。
     
输入: "-91283472332"
输出: -2147483648
解释: 数字 "-91283472332" 超过 32 位有符号整数范围。 
     因此返回 INT_MIN (−231) 。

```

```js
var myAtoi = function(s) {
   const number = parseInt(s, 10);
	//核心是parseInt的实现
    if(isNaN(number)) {
        return 0;
    } else if (number < 1<<31 || number > -(1<<31) - 1) {
        return number < 1<<31 ? 1<<31 :-(1<<31)- 1;
    } else {
        return number;
    }
};
```

#### 9.回文数

判断一个整数是否是回文数。回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。

**示例 1:**

```
输入: 121
输出: true

输入: -121
输出: false
解释: 从左向右读, 为 -121 。 从右向左读, 为 121- 。因此它不是一个回文数。

输入: -121
输出: false
解释: 从左向右读, 为 -121 。 从右向左读, 为 121- 。因此它不是一个回文数。
```

```js
var isPalindrome = function(x) {
    //颠倒顺序再比较即可
    return x == x.toString().split('').reverse().join('');
};
```

#### 11. 盛最多水的容器

给你 n 个非负整数 a1，a2，...，an，每个数代表坐标中的一个点 (i, ai) 。在坐标内画 n 条垂直线，垂直线 i 的两个端点分别为 (i, ai) 和 (i, 0) 。找出其中的两条线，使得它们与 x 轴共同构成的容器可以容纳最多的水。

![image-20201229203152769](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201229203152769.png)

```js
/*解题思路
	i队头，j队尾，向中间逼近
	左边低，i右移
	右边低，j左移
*/

/*双指针法
（一句话解释就是说矮柱子选取后如果移动高柱子的话面积是一定会减小的，因为长度距离在变小的时候，此时高度只能小于或等于矮的柱子。因此只能移动矮的柱子这边才有可能使得高度比矮柱子大）*/
var maxArea = function(height) {
    var i = 0, j = height.length - 1, max = 0
    while (i < j) {
        max = Math.max(max, (j - i) * (height[i] > height[j] ? height[j--]: height[i++]))
    }
    return max
};

```

