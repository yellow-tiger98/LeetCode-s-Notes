# LeetCode-字符串

## 1.最长公共前缀

编写一个函数来查找字符串数组中的最长公共前缀。

如果不存在公共前缀，返回空字符串 `""`。

> **示例 1:**
>
> ```
> 输入: ["flower","flow","flight"]
> 输出: "fl"
> ```

> **示例 2:**
>
> ```
> 输入: ["dog","racecar","car"]
> 输出: ""
> 解释: 输入不存在公共前缀。
> ```

```java
public class Test06 {
    public String longestCommonPrefix(String[] strs) {
      	// 获取字符串数组的长度
        int count = strs.length;
      	// 初始化公共前缀
        String prefix = "";
       	// 如果字符串数组长度大于0 则就拿首个字符串与后面的字符串进行前缀比对
        if(count != 0){
            prefix = strs[0];
        }
      	// 遍历字符串数组，找出最长的公共前缀
        for(int i=1; i<count; i++){
          	// startsWith("")返回的是true
            // substring(begin,end) 前开后闭，所以每次截取，实际取到都会少一个字符
          	// substring(0,0)返回的是""
            while(!strs[i].startsWith(prefix)){ 
                prefix = prefix.substring(0, prefix.length()-1);
            }
        }
      	// 返回最长公共前缀
        return prefix;
    }
}
```

## 2.最长回文子串

给定一个字符串 `s`，找到 `s` 中最长的回文子串。你可以假设 `s` 的最大长度为 1000。

> **示例 1：**
>
> ```
> 输入: "babad"
> 输出: "bab"
> 注意: "aba" 也是一个有效答案。
> ```

> **示例 2：**
>
> ```
> 输入: "cbbd"
> 输出: "bb"
> ```

自己的解法（超出时间限制）：

```java
/**
 * @description 自己的解法 思路：分为 偶数情况 和 奇数情况 将所有的回文子串放入一个list集合，通过自定义排序器获取最长的
 * @param str
 * @return
 */
public static String getLongestCommon(String str){
        if(str.length()<2){
            return str;
        }
        List<String> list = new ArrayList<>();
        for(int i = 0;i<str.length();i++){
            char index = str.charAt(i);
            if(i != str.length()-1){
                for(int j = i+1;j <str.length();j++){
                    if(String.valueOf(index).equals(String.valueOf(str.charAt(j)))) {
                        int first = i;
                        int last = j;
                        boolean istrue = false;
                        if((last-first+1)%2==0){ //偶数
                            if(last-first+1==2){
                                list.add(str.substring(i,j+1));
                            }else{
                                while(last-first!=1){
                                    if(String.valueOf(str.charAt(first+1)).equals(String.valueOf(str.charAt(last-1)))){
                                        istrue = true;
                                    }else{
                                        istrue = false;
                                    }
                                    if (istrue==false){
                                        break;
                                    }
                                    first++;
                                    last--;
                                }
                                if(istrue){
                                    list.add(str.substring(i,j+1));
                                }
                            }
                        }else{ // 奇数
                            if(last-first+1==3){
                                list.add(str.substring(i,j+1));
                            }else{
                                while(first+1!=last-1){
                                    if(String.valueOf(str.charAt(first+1)).equals(String.valueOf(str.charAt(last-1)))){
                                        istrue = true;
                                    }else{
                                        istrue = false;
                                    }
                                    if (istrue==false){
                                        break;
                                    }
                                    first++;
                                    last--;
                                }
                                if(istrue){
                                    list.add(str.substring(i,j+1));
                                }
                            }
                        }
                    }
                }
            }
        }
        if (list.size()>0){
            Collections.sort(list, new Comparator<String>() {
                @Override
                public int compare(String o1, String o2) {
                    return o2.length()-o1.length();
                }
            });
            return list.get(0);
        }
        return String.valueOf(str.charAt(0));
    }
```

暴力解法：

```java
/**
 * @description 暴力解法，两次循环遍历找出最长的回文子串
 * @param str
 * @return
 */
public String getLongestCommon(String str){
  int len = str.length();
  // 长度小于2 表示回文子串只可能是自身了
  if( len < 2){
    return str;
  }
  // substring(index , last) 前开后必，最大长度默认值设为1，如果没有最长回文子串，则就取字符串首个了
  int maxLen = 1;
  // 从字符串首位字符开始
  int begin = 0;
  // 将字符串转化为字符数组，因为charAt每次都会去判断是否下标越界
  char[] strArr = str.toCharArr(); 
  // 外层循环，一个一个遍历字符数组中，排除最后一个，因为最后一个，不可能构成回文子串了
  for(int i = 0; i < len - 1; i++ ){
    // 内层循环，遍历的是排在外层循环当前所遍历的字符之后的字符
    for(int j = i+1; j < len; j++){
      // j-1之后+1是因为下标与实际长度相差1 >maxLength用于取最长回文子串
      // validPalindromic方法是用于判断i到j之间的数是否构成回文子串
      if(j-i+1 > maxLen && validate(strArr , i , j)){
        // 判断通过后，maxLength和begin都进行更新
        maxLen = j-i+1;
        begin = i;
      }
    }
  }
  return str.substring(begin,begin+maxLen);
}

/**
 * @description 用于验证是否构成回文子串
 * @param arr
 * @param left
 * @param right
 * @return
 */
public boolean validate(char[] arr , int left , int right){
  // 基于我上面的解法，应该想到，不管是奇数还是偶数，都符合用left < right作条件 
  while(left < right){
    if(arr[left] != arr[right]){
      return false;
    }
    left ++;
    right --;
  }
  return true;
}
```

动态规划解法（头痛）：

```java
// 万事开头难
```

