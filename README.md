# leetcode1
Given an array of integers nums and an integer target, return indices of the two numbers such that they add up to target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

You can return the answer in any order.

 

Example 1:

Input: nums = [2,7,11,15], target = 9
Output: [0,1]
Explanation: Because nums[0] + nums[1] == 9, we return [0, 1].
Example 2:

Input: nums = [3,2,4], target = 6
Output: [1,2]
Example 3:

Input: nums = [3,3], target = 6
Output: [0,1]
 

Constraints:

2 <= nums.length <= 104
-109 <= nums[i] <= 109
-109 <= target <= 109
Only one valid answer exists.
 
//CODE
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        
       
        for(int i=0;i<nums.size();i++)
            {
            for(int j=i+1;j<nums.size();j++){
                if(nums[i]+nums[j]==target)
                {
                return{i,j};
                }}
        
    }
        return{};
        }
};



LEETCODE2 //ADD TWO NUMBERS
You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order, and each of their nodes contains a single digit. Add the two numbers and return the sum as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

 

Example 1:


Input: l1 = [2,4,3], l2 = [5,6,4]
Output: [7,0,8]
Explanation: 342 + 465 = 807.
Example 2:

Input: l1 = [0], l2 = [0]
Output: [0]
Example 3:

Input: l1 = [9,9,9,9,9,9,9], l2 = [9,9,9,9]
Output: [8,9,9,9,0,0,0,1]
 

Constraints:

The number of nodes in each linked list is in the range [1, 100].
0 <= Node.val <= 9
//CODE
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode temp(0);

    ListNode* curr = &temp;

    int carry = 0;

    while (l1 != nullptr || l2 != nullptr || carry > 0) {

      if (l1 != nullptr) {

        carry += l1->val;

        l1 = l1->next;

      }

      if (l2 != nullptr) {

        carry += l2->val;

        l2 = l2->next;

      }

      curr->next = new ListNode(carry % 10);

      carry /= 10;

      curr = curr->next;

    }

    return temp.next;
    }

};

LEETCODE3 //LONGEST SUBSTRING WITHOUT REPEATING CHARACTERS
Given a string s, find the length of the longest substring without duplicate characters.

 

Example 1:

Input: s = "abcabcbb"
Output: 3
Explanation: The answer is "abc", with the length of 3.
Example 2:

Input: s = "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.
Example 3:

Input: s = "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3.
Notice that the answer must be a substring, "pwke" is a subsequence and not a substring.
 
//CODE

class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        int maxLength = 0;

    int left = 0;

    unordered_map<char, int> charIndex;

    for (int right = 0; right < s.length(); right++) {

        // Check if the current character is already in the map

        if (charIndex.find(s[right]) != charIndex.end() && charIndex[s[right]] >= left) {

            // If so, update the left pointer to the next position of the repeated character

            left = charIndex[s[right]] + 1;

        }

        // Update the index of the current character in the map

        charIndex[s[right]] = right;

        // Update the maximum length

        maxLength = max(maxLength, right - left + 1);

    }

    return maxLength;

}
    
};


LEETCODE4//MEDIAN OF TWO SORTED ARRAY
Given two sorted arrays nums1 and nums2 of size m and n respectively, return the median of the two sorted arrays.

The overall run time complexity should be O(log (m+n)).

 

Example 1:

Input: nums1 = [1,3], nums2 = [2]
Output: 2.00000
Explanation: merged array = [1,2,3] and median is 2.
Example 2:

Input: nums1 = [1,2], nums2 = [3,4]
Output: 2.50000
Explanation: merged array = [1,2,3,4] and median is (2 + 3) / 2 = 2.5.
 
//CODE
//BRUTE FORCE(RUN TIME ERROR)
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        int n=nums1.size(),m=nums2.size();
        vector<int>merged;
        int i=0,j=0;
        while(i<n && j<m)
        {
            if(nums1[i]<nums2[j])
            merged.push_back(nums1[i++]);
            else
            merged.push_back(nums2[j++]);
        }
        while(i<n)merged.push_back(nums1[i++]);
        while(j<m)merged.push_back(nums2[j++]);
        int total=n+m;
        if(total%2==1)
        {
            return merged[total/2];
        }
        else
        {
            return (merged[total/-1]+merged[total/2])/2.0;
        }
    }
};

//OPTIMISE SOLUTION
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
    if(nums1.size()>nums2.size())return findMedianSortedArrays(nums2,nums1);
    int n1=nums1.size();
    int n2=nums2.size();
    int low=0,high=n1;
    while(low<=high)
    {
        int cut1=(low+high)/2;
        int cut2=(n1+n2+1)/2-cut1;
        int l1=(cut1==0)?INT_MIN:nums1[cut1-1];
        int l2=(cut2==0)?INT_MIN:nums2[cut2-1];
        int r1=(cut1==n1)?INT_MAX:nums1[cut1];
        int r2=(cut2==n2)?INT_MAX:nums2[cut2];
        if(l1<=r2 && l2<=r1)
        {
            if((n1+n2)%2==0)
            return (max(l1,l2)+min(r1,r2))/2.0;
            else 
            return max(l1,l2);
        }
        else if(l1>r2)
        {
            high=cut1-1;
        }
        else
        {
            low=cut1+1;
        }
    }    
    return 0.0;
    }
};



LEETCODE5// LONGEST PALINDROMIC SUBSTRING   //dp 
Given a string s, return the longest palindromic substring in s.

Example 1:

Input: s = "babad"
Output: "bab"
Explanation: "aba" is also a valid answer.
Example 2:

Input: s = "cbbd"
Output: "bb"

 //CODE
 class Solution {
public:
    string longestPalindrome(string s) {
        int n = s.size();

        vector<vector<bool>> dp(n, vector<bool>(n));

        array<int, 2> ans = {0, 0};

        for (int i = 0; i < n; ++i) {

            dp[i][i] = true;

        }

        for (int i = 0; i < n - 1; ++i) {

            if (s[i] == s[i + 1]) {

                dp[i][i + 1] = true;

                ans = {i, i + 1};

            }

        }

        for (int diff = 2; diff < n; ++diff) {

            for (int i = 0; i < n - diff; ++i) {

                int j = i + diff;

                if (s[i] == s[j] && dp[i + 1][j - 1]) {

                    dp[i][j] = true;

                    ans = {i, j};

                }

            }

        }

        int i = ans[0];

        int j = ans[1];

        return s.substr(i, j - i + 1);

    
    }
};


LEETCODE6//ZIGZAG CONVERSION

The string "PAYPALISHIRING" is written in a zigzag pattern on a given number of rows like this: (you may want to display this pattern in a fixed font for better legibility)

P   A   H   N
A P L S I I G
Y   I   R
And then read line by line: "PAHNAPLSIIGYIR"

Write the code that will take a string and make this conversion given a number of rows:

string convert(string s, int numRows);
 

Example 1:

Input: s = "PAYPALISHIRING", numRows = 3
Output: "PAHNAPLSIIGYIR"
Example 2:

Input: s = "PAYPALISHIRING", numRows = 4
Output: "PINALSIGYAHRPI"
Explanation:
P     I    N
A   L S  I G
Y A   H R
P     I
Example 3:

Input: s = "A", numRows = 1
Output: "A"

//CODE
class Solution {
public:
    string convert(string s, int numRows) {
        vector<string>ans(numRows);
        if(numRows==1)
            {
            return s;
            }
        bool flag=false;
        int i=0;
        for(auto ch:s)
            {
   

       ans[i]+=ch;
 if(i==0 || i==numRows-1)
     {
     flag=!flag;
     }
       if(flag)
           {
           i+=1;
           }
       else
           {
           i-=1;
           }
       }
            
       string zigzag="";
       for(auto str:ans)
           {
           zigzag+=str;
           }
       return zigzag;
    }
        
};


LEETCODE7//REVERSE INTEGER

Given a signed 32-bit integer x, return x with its digits reversed. If reversing x causes the value to go outside the signed 32-bit integer range [-231, 231 - 1], then return 0.

Assume the environment does not allow you to store 64-bit integers (signed or unsigned).

 

Example 1:

Input: x = 123
Output: 321
Example 2:

Input: x = -123
Output: -321
Example 3:

Input: x = 120
Output: 21

 //CODE
 class Solution {
public:
    int reverse(int x) {
        int ans=0;
        while(x!=0)
            {
        int digit=x%10;
 if((ans>INT_MAX/10) || (ans<INT_MIN/10))
     {
     return 0;
     }
            ans=(ans*10)+digit;
            x=x/10;
            }
        return ans;
    }
};


LEETCODE8//STRING TO INTEGER(ATOI)
Implement the myAtoi(string s) function, which converts a string to a 32-bit signed integer.

The algorithm for myAtoi(string s) is as follows:

Whitespace: Ignore any leading whitespace (" ").
Signedness: Determine the sign by checking if the next character is '-' or '+', assuming positivity if neither present.
Conversion: Read the integer by skipping leading zeros until a non-digit character is encountered or the end of the string is reached. If no digits were read, then the result is 0.
Rounding: If the integer is out of the 32-bit signed integer range [-231, 231 - 1], then round the integer to remain in the range. Specifically, integers less than -231 should be rounded to -231, and integers greater than 231 - 1 should be rounded to 231 - 1.
Return the integer as the final result.

 

Example 1:

Input: s = "42"

Output: 42

Explanation:

The underlined characters are what is read in and the caret is the current reader position.
Step 1: "42" (no characters read because there is no leading whitespace)
         ^
Step 2: "42" (no characters read because there is neither a '-' nor '+')
         ^
Step 3: "42" ("42" is read in)
           ^
Example 2:

Input: s = " -042"

Output: -42

Explanation:

Step 1: "   -042" (leading whitespace is read and ignored)
            ^
Step 2: "   -042" ('-' is read, so the result should be negative)
             ^
Step 3: "   -042" ("042" is read in, leading zeros ignored in the result)
               ^
Example 3:

Input: s = "1337c0d3"

Output: 1337

Explanation:

Step 1: "1337c0d3" (no characters read because there is no leading whitespace)
         ^
Step 2: "1337c0d3" (no characters read because there is neither a '-' nor '+')
         ^
Step 3: "1337c0d3" ("1337" is read in; reading stops because the next character is a non-digit)
             ^
Example 4:

Input: s = "0-1"

//CODE

class Solution {
public:
    int myAtoi(string s) {
      int sign = 1, base = 0, i = 0;

    while (s[i] == ' ') { i++; }

    if (s[i] == '-' || s[i] == '+') {

        sign = 1 - 2 * (s[i++] == '-'); 

    }

    while (s[i] >= '0' && s[i] <= '9') {

        if (base >  INT_MAX / 10 || (base == INT_MAX / 10 && s[i] - '0' > 7)) {

            if (sign == 1) return INT_MAX;

            else return INT_MIN;

        }

        base  = 10 * base + (s[i++] - '0');

    }

    return base * sign;

  
    }
};



LEETCODE9//PALINDROME NUMBER

Given an integer x, return true if x is a palindrome, and false otherwise.

 

Example 1:

Input: x = 121
Output: true
Explanation: 121 reads as 121 from left to right and from right to left.
Example 2:

Input: x = -121
Output: false
Explanation: From left to right, it reads -121. From right to left, it becomes 121-. Therefore it is not a palindrome.
Example 3:

Input: x = 10
Output: false
Explanation: Reads 01 from right to left. Therefore it is not a palindrome.


 //CODE

class Solution {
public:
    bool isPalindrome(int x) {
        if (x < 0)

      return false;

    long reversed = 0;

    int y = x;

    while (y > 0) {

      reversed = reversed * 10 + y % 10;

      y /= 10;

    }

    return reversed == x;

  }

};
        


LEETCODE10 //REGULAR EXPRESSION MATCHING (DP)

Given an input string s and a pattern p, implement regular expression matching with support for '.' and '*' where:

'.' Matches any single character.​​​​
'*' Matches zero or more of the preceding element.
The matching should cover the entire input string (not partial).

 

Example 1:

Input: s = "aa", p = "a"
Output: false
Explanation: "a" does not match the entire string "aa".
Example 2:

Input: s = "aa", p = "a*"
Output: true
Explanation: '*' means zero or more of the preceding element, 'a'. Therefore, by repeating 'a' once, it becomes "aa".
Example 3:

Input: s = "ab", p = ".*"
Output: true
Explanation: ".*" means "zero or more (*) of any character (.)".
 
//CODE

class Solution {
public:
    bool isMatch(string s, string p) {
        const int m = s.length();

    const int n = p.length();

    // dp[i][j] := true if s[0..i) matches p[0..j)

    vector<vector<bool>> dp(m + 1, vector<bool>(n + 1));

    dp[0][0] = true;

    auto isMatch = [&](int i, int j) -> bool {

      return j >= 0 && p[j] == '.' || s[i] == p[j];

    };

    for (int j = 0; j < p.length(); ++j)

      if (p[j] == '*' && dp[0][j - 1])

        dp[0][j + 1] = true;

    for (int i = 0; i < m; ++i)

      for (int j = 0; j < n; ++j)

        if (p[j] == '*') {

          // The minimum index of '*' is 1.

          const bool noRepeat = dp[i + 1][j - 1];

          const bool doRepeat = isMatch(i, j - 1) && dp[i][j + 1];

          dp[i + 1][j + 1] = noRepeat || doRepeat;

        } else if (isMatch(i, j)) {

          dp[i + 1][j + 1] = dp[i][j];

        }

    return dp[m][n];

  }
    
};

LEETCODE11//CONTAINER WITH MOST WATER

You are given an integer array height of length n. There are n vertical lines drawn such that the two endpoints of the ith line are (i, 0) and (i, height[i]).

Find two lines that together with the x-axis form a container, such that the container contains the most water.

Return the maximum amount of water a container can store.

Notice that you may not slant the container.

 

Example 1:


Input: height = [1,8,6,2,5,4,8,3,7]
Output: 49
Explanation: The above vertical lines are represented by array [1,8,6,2,5,4,8,3,7]. In this case, the max area of water (blue section) the container can contain is 49.
Example 2:

Input: height = [1,1]
Output: 1
 
//CODE
class Solution {
public:
    int maxArea(vector<int>& height) {
        int left=0;
        int right=height.size()-1;
        int maxArea=0;
        while(left<right)
            {
  int width=right-left;
 int minHeight=min(height[left],height[right]);
int area=width*minHeight;
    maxArea=max(maxArea,area);
 if(height[left]<height[right])
     {
     left++;
     }
            else
                {
                right--;
                }
            }
        return maxArea;
    }
};


LEETCODE12//INTEGER TO ROMAN

Seven different symbols represent Roman numerals with the following values:

Symbol	Value
I	1
V	5
X	10
L	50
C	100
D	500
M	1000
Roman numerals are formed by appending the conversions of decimal place values from highest to lowest. Converting a decimal place value into a Roman numeral has the following rules:

If the value does not start with 4 or 9, select the symbol of the maximal value that can be subtracted from the input, append that symbol to the result, subtract its value, and convert the remainder to a Roman numeral.
If the value starts with 4 or 9 use the subtractive form representing one symbol subtracted from the following symbol, for example, 4 is 1 (I) less than 5 (V): IV and 9 is 1 (I) less than 10 (X): IX. Only the following subtractive forms are used: 4 (IV), 9 (IX), 40 (XL), 90 (XC), 400 (CD) and 900 (CM).
Only powers of 10 (I, X, C, M) can be appended consecutively at most 3 times to represent multiples of 10. You cannot append 5 (V), 50 (L), or 500 (D) multiple times. If you need to append a symbol 4 times use the subtractive form.
Given an integer, convert it to a Roman numeral.

 

Example 1:

Input: num = 3749

Output: "MMMDCCXLIX"

Explanation:

3000 = MMM as 1000 (M) + 1000 (M) + 1000 (M)
 700 = DCC as 500 (D) + 100 (C) + 100 (C)
  40 = XL as 10 (X) less of 50 (L)
   9 = IX as 1 (I) less of 10 (X)
Note: 49 is not 1 (I) less of 50 (L) because the conversion is based on decimal places
Example 2:

Input: num = 58

Output: "LVIII"

Explanation:

50 = L
 8 = VIII
Example 3:

Input: num = 1994

Output: "MCMXCIV"

Explanation:

1000 = M

//CODE
class Solution {
public:
    string intToRoman(int num) {
    const vector<pair<int, string>> valueSymbols{

        {1000, "M"}, {900, "CM"}, {500, "D"}, {400, "CD"}, {100, "C"},

        {90, "XC"},  {50, "L"},   {40, "XL"}, {10, "X"},   {9, "IX"},

        {5, "V"},    {4, "IV"},   {1, "I"}};

    string ans;

    for (const auto& [value, symbol] : valueSymbols) {

      if (num == 0)

        break;

      while (num >= value) {

        num -= value;

        ans += symbol;

      }

    }

    return ans;    
    }
};

LEETCODE13//ROMAN TO INTEGER

Roman numerals are represented by seven different symbols: I, V, X, L, C, D and M.

Symbol       Value
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
For example, 2 is written as II in Roman numeral, just two ones added together. 12 is written as XII, which is simply X + II. The number 27 is written as XXVII, which is XX + V + II.

Roman numerals are usually written largest to smallest from left to right. However, the numeral for four is not IIII. Instead, the number four is written as IV. Because the one is before the five we subtract it making four. The same principle applies to the number nine, which is written as IX. There are six instances where subtraction is used:

I can be placed before V (5) and X (10) to make 4 and 9. 
X can be placed before L (50) and C (100) to make 40 and 90. 
C can be placed before D (500) and M (1000) to make 400 and 900.
Given a roman numeral, convert it to an integer.

 

Example 1:

Input: s = "III"
Output: 3
Explanation: III = 3.
Example 2:

Input: s = "LVIII"
Output: 58
Explanation: L = 50, V= 5, III = 3.
Example 3:

Input: s = "MCMXCIV"
Output: 1994
Explanation: M = 1000, CM = 900, XC = 90 and IV = 4.
 

//CODE
class Solution {
public:
    int romanToInt(string s) {
        int ans = 0;

    vector<int> roman(128);

    roman['I'] = 1;

    roman['V'] = 5;

    roman['X'] = 10;

    roman['L'] = 50;

    roman['C'] = 100;

    roman['D'] = 500;

    roman['M'] = 1000;

    for (int i = 0; i + 1 < s.length(); ++i)

      if (roman[s[i]] < roman[s[i + 1]])

        ans -= roman[s[i]];

      else

        ans += roman[s[i]];

    return ans + roman[s.back()];
    }
};


LEETCODE14//LONGEST COMMON PREFIX
Write a function to find the longest common prefix string amongst an array of strings.

If there is no common prefix, return an empty string "".

 

Example 1:

Input: strs = ["flower","flow","flight"]
Output: "fl"
Example 2:

Input: strs = ["dog","racecar","car"]
Output: ""
Explanation: There is no common prefix among the input strings.
 
//CODE

class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        string ans="";
  for(int i=0;i<strs[0].length();i++)
      {
      char ch=strs[0][i];
      bool match=true;
      for(int j=1;j<strs.size();j++)
          {
          if(ch!=strs[j][i])
              {
              match=false;
              break;
              }
          }
      if(match==false)
          break;
      else
          ans.push_back(ch);
      }
        return ans;
    }
};


LEETCODE15//3SUM

Given an integer array nums, return all the triplets [nums[i], nums[j], nums[k]] such that i != j, i != k, and j != k, and nums[i] + nums[j] + nums[k] == 0.

Notice that the solution set must not contain duplicate triplets.

 

Example 1:

Input: nums = [-1,0,1,2,-1,-4]
Output: [[-1,-1,2],[-1,0,1]]
Explanation: 
nums[0] + nums[1] + nums[2] = (-1) + 0 + 1 = 0.
nums[1] + nums[2] + nums[4] = 0 + 1 + (-1) = 0.
nums[0] + nums[3] + nums[4] = (-1) + 2 + (-1) = 0.
The distinct triplets are [-1,0,1] and [-1,-1,2].
Notice that the order of the output and the order of the triplets does not matter.
Example 2:

Input: nums = [0,1,1]
Output: []
Explanation: The only possible triplet does not sum up to 0.
Example 3:

Input: nums = [0,0,0]
Output: [[0,0,0]]
Explanation: The only possible triplet sums up to 0.

 //CODE

class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
  vector<vector<int>>ans;
 sort(nums.begin(),nums.end());
 for(int i=0;i<nums.size();i++)
    {
 if(i>0&&nums[i]==nums[i-1])continue;
     int j=i+1;
     int k=nums.size()-1;
     while(j<k)
           {
  int sum=nums[i]+nums[j]+nums[k];
               if(sum<0)
                   {
                   j++;
                   }
  else if(sum>0)
      {
      k--;
      }
               else
                   {
  vector<int>temp={nums[i],nums[j],nums[k]};
   ans.push_back(temp);
                   j++;
                   k--;
 while(j<k&&nums[j]==nums[j-1])j++;
 while(j<k&&nums[k]==nums[k+1])k--;
                   }
               }
           
               }
           return ans;
              
    }
};



LEETCODE16//3SUM CLOSEST

Given an integer array nums of length n and an integer target, find three integers in nums such that the sum is closest to target.

Return the sum of the three integers.

You may assume that each input would have exactly one solution.

 

Example 1:

Input: nums = [-1,2,1,-4], target = 1
Output: 2
Explanation: The sum that is closest to the target is 2. (-1 + 2 + 1 = 2).
Example 2:

Input: nums = [0,0,0], target = 1
Output: 0
Explanation: The sum that is closest to the target is 0. (0 + 0 + 0 = 0).
 
//CODE

class Solution {
public:
    int threeSumClosest(vector<int>& nums, int target) {
     
        sort(nums.begin(),nums.end());
        int closestSum=nums[0]+nums[1]+nums[2];
        for(int i=0;i<nums.size()-2;i++)
        {
            int j=i+1;
            int k=nums.size()-1;
            while(j<k)
            {
                int sum=nums[i]+nums[j]+nums[k];
                if(abs(sum-target)<abs(closestSum-target))
                {
                    closestSum=sum;
                }
                if(sum<target)
                {
                    j++;
                }
                else if(sum>target)
                {
                    k--;
                }
                else
                {
                    return target;
                }
            }
        }
        return closestSum;
    }
};



LEETCODE17//LETTER COMBINATION OF A PHONE NUMBER

Given a string containing digits from 2-9 inclusive, return all possible letter combinations that the number could represent. Return the answer in any order.

A mapping of digits to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.


 

Example 1:

Input: digits = "23"
Output: ["ad","ae","af","bd","be","bf","cd","ce","cf"]
Example 2:

Input: digits = ""
Output: []
Example 3:

Input: digits = "2"
Output: ["a","b","c"]


 //CODE
 class Solution {
public:
    vector<string> letterCombinations(string digits) {
        if (digits.empty())

      return {};

    vector<string> ans{""};

    const vector<string> digitToLetters{"",    "",    "abc",  "def", "ghi",

                                        "jkl", "mno", "pqrs", "tuv", "wxyz"};

    for (const char d : digits) {

      vector<string> temp;

      for (const string& s : ans)

        for (const char c : digitToLetters[d - '0'])

          temp.push_back(s + c);

      ans = std::move(temp);

    }

    return ans;

  
    }
};


LEEETCODE18//4SUM

Given an array nums of n integers, return an array of all the unique quadruplets [nums[a], nums[b], nums[c], nums[d]] such that:

0 <= a, b, c, d < n
a, b, c, and d are distinct.
nums[a] + nums[b] + nums[c] + nums[d] == target
You may return the answer in any order.

 

Example 1:

Input: nums = [1,0,-1,0,-2,2], target = 0
Output: [[-2,-1,1,2],[-2,0,0,2],[-1,0,0,1]]
Example 2:

Input: nums = [2,2,2,2,2], target = 8
Output: [[2,2,2,2]]
 
//CODE
class Solution {
public:
    vector<vector<int>> fourSum(vector<int>& nums, int target) {
           int n = nums.size(); //size of the array
    vector<vector<int>> ans;

    // sort the given array:
    sort(nums.begin(), nums.end());

    //calculating the quadruplets:
    for (int i = 0; i < n; i++) {
        // avoid the duplicates while moving i:
        if (i > 0 && nums[i] == nums[i - 1]) continue;
        for (int j = i + 1; j < n; j++) {
            // avoid the duplicates while moving j:
            if (j > i + 1 && nums[j] == nums[j - 1]) continue;

            // 2 pointers:
            int k = j + 1;
            int l = n - 1;
            while (k < l) {
                long long sum = nums[i];
                sum += nums[j];
                sum += nums[k];
                sum += nums[l];
                if (sum == target) {
                    vector<int> temp = {nums[i], nums[j], nums[k], nums[l]};
                    ans.push_back(temp);
                    k++; l--;

                    //skip the duplicates:
                    while (k < l && nums[k] == nums[k - 1]) k++;
                    while (k < l && nums[l] == nums[l + 1]) l--;
                }
                else if (sum < target) k++;
                else l--;
            }
        }
    }

    return ans;
    }
};


LEETCODE19//REMOVE NTH NODE FROM END OF THE LIST

Given the head of a linked list, remove the nth node from the end of the list and return its head.

 

Example 1:


Input: head = [1,2,3,4,5], n = 2
Output: [1,2,3,5]
Example 2:

Input: head = [1], n = 1
Output: []
Example 3:

Input: head = [1,2], n = 1
Output: [1]

 //CODE
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
      



 

    ListNode* slow = head;

    ListNode* fast = head;

    while (n-- > 0)

      fast = fast->next;

    if (fast == nullptr)

      return head->next;

    while (fast->next != nullptr) {

      slow = slow->next;

      fast = fast->next;

    }

    slow->next = slow->next->next;

    return head;

  }
  
};


LEETCODE20//VALID PARENTHESES

Given a string s containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.

An input string is valid if:

Open brackets must be closed by the same type of brackets.
Open brackets must be closed in the correct order.
Every close bracket has a corresponding open bracket of the same type.
 

Example 1:

Input: s = "()"

Output: true

Example 2:

Input: s = "()[]{}"

Output: true

Example 3:

Input: s = "(]"

Output: false

Example 4:

Input: s = "([])"

Output: true

 //CODE

 class Solution {
public:
    bool isValid(string s) {
        stack<char> st ; 

        for (int i = 0 ;  i< s.length() ; i++)

        {

            char ch = s[i];

            // if opening bracket then push into the stack 

            if (ch == '(' || ch == '{' || ch == '[')

            {

                st.push(ch) ; 

            }

            else {

   if (!st.empty())

                {

                    char top = st.top() ;

                    if ((ch == ')' && top == '(') || 

                        (ch == '}' && top == '{') ||

                        (ch == ']' && top == '[')) 

                        { 

                            st.pop() ;

                        }

                        else 

                        {

                            return false ; 

                        }

                }

                else 

               {

                    return false ;

                }

            }

        }

        if (st.empty())

        {

            return true ; 

        }

        return false ;

    }
    
};
//2ND SOLUTION
class Solution {
public:
    bool isValid(string s) {
        stack<char> stack;

    unordered_map<char, char> mapping = {

        {')', '('},

        {']', '['},

        {'}', '{'}

    };

    for (char c : s) {

        if (c == '(' || c == '[' || c == '{') {

            stack.push(c);

        } else if (c == ')' || c == ']' || c == '}') {

            if (stack.empty() || stack.top() != mapping[c]) {

                return false;

            }

            stack.pop();

        }

    }

    return stack.empty();

}
    
};
//3RD SOLUTION
class Solution {
public:
    bool isValid(string s) {
    stack<char>st;
        for(auto c:s)
            {
   if(st.empty())
       {
       st.push(c);
       }
    else if((st.top()=='(' && c==')') || (st.top()=='[' && c==']') || (st.top()=='{' && c=='}'))
        {
        st.pop();
        }
            else
                {
                st.push(c);
                }
            }
        if(st.size()==0)
            return true;
        return false;
    }
};


LEETCODE21//MERGE TWO SORTED LIST

You are given the heads of two sorted linked lists list1 and list2.

Merge the two lists into one sorted list. The list should be made by splicing together the nodes of the first two lists.

Return the head of the merged linked list.

 

Example 1:


Input: list1 = [1,2,4], list2 = [1,3,4]
Output: [1,1,2,3,4,4]
Example 2:

Input: list1 = [], list2 = []
Output: []
Example 3:

Input: list1 = [], list2 = [0]
Output: [0]
 

//CODE
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {
       

  
    if (!list1 || !list2)
      return list1 ? list1 : list2;
    if (list1->val > list2->val)
      swap(list1, list2);
    list1->next = mergeTwoLists(list1->next, list2);
    return list1;
  

    }

};



LEETCODE22//GENERATE PARENTHESES(DP)

Given n pairs of parentheses, write a function to generate all combinations of well-formed parentheses.

 
Example 1:

Input: n = 3
Output: ["((()))","(()())","(())()","()(())","()()()"]
Example 2:

Input: n = 1
Output: ["()"]
 
//CODE
class Solution {
public:
    vector<string> generateParenthesis(int n) {
        vector<string> ans;
        function<void(int, int, string)> dfs = [&](int l, int r, string t) {
            if (l > n || r > n || l < r) return;
            if (l == n && r == n) {
                ans.push_back(t);
                return;
            }
            dfs(l + 1, r, t + "(");
            dfs(l, r + 1, t + ")");
        };
        dfs(0, 0, "");
        return ans;
    }
};


LEETCODE23//MERGE K SORTED LISTS

You are given an array of k linked-lists lists, each linked-list is sorted in ascending order.

Merge all the linked-lists into one sorted linked-list and return it.

 

Example 1:

Input: lists = [[1,4,5],[1,3,4],[2,6]]
Output: [1,1,2,3,4,4,5,6]
Explanation: The linked-lists are:
[
  1->4->5,
  1->3->4,
  2->6
]
merging them into one sorted list:
1->1->2->3->4->4->5->6
Example 2:

Input: lists = []
Output: []
Example 3:

Input: lists = [[]]
Output: []

 //CODE
 /**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* mergeKLists(vector<ListNode*>& lists) {
     





    ListNode dummy(0);

    ListNode* curr = &dummy;

    auto compare = [](ListNode* a, ListNode* b) { return a->val > b->val; };

    priority_queue<ListNode*, vector<ListNode*>, decltype(compare)> minHeap(

        compare);

    for (ListNode* list : lists)

      if (list != nullptr)

        minHeap.push(list);

    while (!minHeap.empty()) {

      ListNode* minNode = minHeap.top();

      minHeap.pop();

      if (minNode->next)

        minHeap.push(minNode->next);

      curr->next = minNode;

      curr = curr->next;

    }

    return dummy.next;

  


    }

};



LEETCODE24//SWAP NODES IN PAIRS

Given a linked list, swap every two adjacent nodes and return its head. You must solve the problem without modifying the values in the list's nodes (i.e., only nodes themselves may be changed.)

 

Example 1:

Input: head = [1,2,3,4]

Output: [2,1,4,3]

Explanation:



Example 2:

Input: head = []

Output: []

Example 3:

Input: head = [1]

Output: [1]

Example 4:

Input: head = [1,2,3]

Output: [2,1,3]

 //CODE

 /**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        const int length = getLength(head);

    ListNode dummy(0, head);

    ListNode* prev = &dummy;

    ListNode* curr = head;

    for (int i = 0; i < length / 2; ++i) {

      ListNode* next = curr->next;

      curr->next = next->next;

      next->next = prev->next;

      prev->next = next;

      prev = curr;

      curr = curr->next;

    }

    return dummy.next;

  }

 private:

  int getLength(ListNode* head) {

    int length = 0;

    for (ListNode* curr = head; curr; curr = curr->next)

      ++length;

    return length;

  
    }
};


LEETCODE25//REVERSE NODES IN K-GROUP

Given the head of a linked list, reverse the nodes of the list k at a time, and return the modified list.

k is a positive integer and is less than or equal to the length of the linked list. If the number of nodes is not a multiple of k then left-out nodes, in the end, should remain as it is.

You may not alter the values in the list's nodes, only nodes themselves may be changed.

 

Example 1:


Input: head = [1,2,3,4,5], k = 2
Output: [2,1,4,3,5]
Example 2:


Input: head = [1,2,3,4,5], k = 3
Output: [3,2,1,4,5]

//CODE
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* reverseKGroup(ListNode* head, int k) {
       




    if (head == nullptr)

      return nullptr;

    ListNode* tail = head;

    for (int i = 0; i < k; ++i) {

      // There are less than k nodes in the list, do nothing.

      if (tail == nullptr)

        return head;

      tail = tail->next;

    }

    ListNode* newHead = reverse(head, tail);

    head->next = reverseKGroup(tail, k);

    return newHead;

  }

 private:

  // Reverses [head, tail).

  ListNode* reverse(ListNode* head, ListNode* tail) {

    ListNode* prev = nullptr;

    ListNode* curr = head;

    while (curr != tail) {

      ListNode* next = curr->next;

      curr->next = prev;

      prev = curr;

      curr = next;

    }

    return prev;

  }


    
};


LEETCODDE26//REMOVE DUPLICATES FROM SORTED ARRAY

Given an integer array nums sorted in non-decreasing order, remove the duplicates in-place such that each unique element appears only once. The relative order of the elements should be kept the same. Then return the number of unique elements in nums.

Consider the number of unique elements of nums to be k, to get accepted, you need to do the following things:

Change the array nums such that the first k elements of nums contain the unique elements in the order they were present in nums initially. The remaining elements of nums are not important as well as the size of nums.
Return k.
Custom Judge:

The judge will test your solution with the following code:

int[] nums = [...]; // Input array
int[] expectedNums = [...]; // The expected answer with correct length

int k = removeDuplicates(nums); // Calls your implementation

assert k == expectedNums.length;
for (int i = 0; i < k; i++) {
    assert nums[i] == expectedNums[i];
}
If all assertions pass, then your solution will be accepted.

 

Example 1:

Input: nums = [1,1,2]
Output: 2, nums = [1,2,_]
Explanation: Your function should return k = 2, with the first two elements of nums being 1 and 2 respectively.
It does not matter what you leave beyond the returned k (hence they are underscores).
Example 2:

Input: nums = [0,0,1,1,1,2,2,3,3,4]
Output: 5, nums = [0,1,2,3,4,_,_,_,_,_]
Explanation: Your function should return k = 5, with the first five elements of nums being 0, 1, 2, 3, and 4 respectively.
It does not matter what you leave beyond the returned k (hence they are underscores).
 
//CODE
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        int i=0;
        
        for(int j=i+1;j<nums.size();j++)
            {
            if(nums[j]!=nums[i])
                {
                
                nums[i+1]=nums[j];
                i++;
                }
            }
        return i+1;
           
            
    }
};



LEETCODE27//REMOVE ELEMENTS

Given an integer array nums and an integer val, remove all occurrences of val in nums in-place. The order of the elements may be changed. Then return the number of elements in nums which are not equal to val.

Consider the number of elements in nums which are not equal to val be k, to get accepted, you need to do the following things:

Change the array nums such that the first k elements of nums contain the elements which are not equal to val. The remaining elements of nums are not important as well as the size of nums.
Return k.
Custom Judge:

The judge will test your solution with the following code:

int[] nums = [...]; // Input array
int val = ...; // Value to remove
int[] expectedNums = [...]; // The expected answer with correct length.
                            // It is sorted with no values equaling val.

int k = removeElement(nums, val); // Calls your implementation

assert k == expectedNums.length;
sort(nums, 0, k); // Sort the first k elements of nums
for (int i = 0; i < actualLength; i++) {
    assert nums[i] == expectedNums[i];
}
If all assertions pass, then your solution will be accepted.

 

Example 1:

Input: nums = [3,2,2,3], val = 3
Output: 2, nums = [2,2,_,_]
Explanation: Your function should return k = 2, with the first two elements of nums being 2.
It does not matter what you leave beyond the returned k (hence they are underscores).
Example 2:

Input: nums = [0,1,2,2,3,0,4,2], val = 2
Output: 5, nums = [0,1,4,0,3,_,_,_]
Explanation: Your function should return k = 5, with the first five elements of nums containing 0, 0, 1, 3, and 4.
Note that the five elements can be returned in any order.
It does not matter what you leave beyond the returned k (hence they are underscores).
 
//CODE

class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int i=0;
        for(const int num:nums)
            if(num!=val)
                nums[i++]=num;
        return i;
    }
};



LEETCODE28//FIND THE FIRST OCCURENCE IN A STRING

Given two strings needle and haystack, return the index of the first occurrence of needle in haystack, or -1 if needle is not part of haystack.

 

Example 1:

Input: haystack = "sadbutsad", needle = "sad"
Output: 0
Explanation: "sad" occurs at index 0 and 6.
The first occurrence is at index 0, so we return 0.
Example 2:

Input: haystack = "leetcode", needle = "leeto"
Output: -1
Explanation: "leeto" did not occur in "leetcode", so we return -1.

 //CODE
class Solution {
public:
    int strStr(string haystack, string needle) {
        int n=haystack.size();
        int m=needle.size();
        if(m==0)
            {
            return 0;
    }
        if(n<m)
            {
            return -1;
            }
        for(int i=0;i<n;i++)
            {
            if(haystack[i]==needle[0])
                {
                if(haystack.substr(i,m)==needle)
                    {
                    return i;
                    }
                }
            }
        return -1;
        }
};


LEETCODE29//DIVIDE TWO INTEGERS

Given two integers dividend and divisor, divide two integers without using multiplication, division, and mod operator.

The integer division should truncate toward zero, which means losing its fractional part. For example, 8.345 would be truncated to 8, and -2.7335 would be truncated to -2.

Return the quotient after dividing dividend by divisor.

Note: Assume we are dealing with an environment that could only store integers within the 32-bit signed integer range: [−231, 231 − 1]. For this problem, if the quotient is strictly greater than 231 - 1, then return 231 - 1, and if the quotient is strictly less than -231, then return -231.

 

Example 1:

Input: dividend = 10, divisor = 3
Output: 3
Explanation: 10/3 = 3.33333.. which is truncated to 3.
Example 2:

Input: dividend = 7, divisor = -3
Output: -2
Explanation: 7/-3 = -2.33333.. which is truncated to -2.
 

Constraints:

-231 <= dividend, divisor <= 231 - 1
divisor != 0
//CODE

class Solution {
public:
    int divide(int dividend, int divisor) {
      

 

  

    // -2^{31} / -1 = 2^31 will overflow, so return 2^31 - 1.

    if (dividend == INT_MIN && divisor == -1)

      return INT_MAX;

    const int sign = dividend > 0 ^ divisor > 0 ? -1 : 1;

    long ans = 0;

    long dvd = labs(dividend);

    long dvs = labs(divisor);

    while (dvd >= dvs) {

      long k = 1;

      while (k * 2 * dvs <= dvd)

        k *= 2;

      dvd -= k * dvs;

      ans += k;

    }

    return sign * ans;

  


    }
};



LEETCODE30//SUBSTRING WITH CONCATENTION WITH ALL WORDS

You are given a string s and an array of strings words. All the strings of words are of the same length.

A concatenated string is a string that exactly contains all the strings of any permutation of words concatenated.

For example, if words = ["ab","cd","ef"], then "abcdef", "abefcd", "cdabef", "cdefab", "efabcd", and "efcdab" are all concatenated strings. "acdbef" is not a concatenated string because it is not the concatenation of any permutation of words.
Return an array of the starting indices of all the concatenated substrings in s. You can return the answer in any order.

 

Example 1:

Input: s = "barfoothefoobarman", words = ["foo","bar"]

Output: [0,9]

Explanation:

The substring starting at 0 is "barfoo". It is the concatenation of ["bar","foo"] which is a permutation of words.
The substring starting at 9 is "foobar". It is the concatenation of ["foo","bar"] which is a permutation of words.

Example 2:

Input: s = "wordgoodgoodgoodbestword", words = ["word","good","best","word"]

Output: []

Explanation:

There is no concatenated substring.

Example 3:

Input: s = "barfoofoobarthefoobarman", words = ["bar","foo","the"]

Output: [6,9,12]

Explanation:

The substring starting at 6 is "foobarthe". It is the concatenation of ["foo","bar","the"].
The substring starting at 9 is "barthefoo". It is the concatenation of ["bar","the","foo"].
The substring starting at 12 is "thefoobar". It is the concatenation of ["the","foo","bar"].

 

Constraints:

1 <= s.length <= 104
1 <= words.length <= 5000
1 <= words[i].length <= 30
s and words[i] consist of lowercase English letters.

//CODE
class Solution {
public:
    vector<int> findSubstring(string s, vector<string>& words) {
       



    

        unordered_map<string, int> cnt;

        for (auto& w : words) {

            ++cnt[w];

        }

        int m = s.size(), n = words.size(), k = words[0].size();

        vector<int> ans;

        for (int i = 0; i < k; ++i) {

            unordered_map<string, int> cnt1;

            int l = i, r = i;

            int t = 0;

            while (r + k <= m) {

                string w = s.substr(r, k);

                r += k;

                if (!cnt.count(w)) {

                    cnt1.clear();

                    l = r;

                    t = 0;

                    continue;

                }

                ++cnt1[w];

                ++t;

                while (cnt1[w] > cnt[w]) {

                    string remove = s.substr(l, k);

                    l += k;

                    --cnt1[remove];

                    --t;

                }

                if (t == n) {

                    ans.push_back(l);

                }

            }

        }

        return ans;

    }


    
};


LEETCODE31//NEXT PERMMUTATIONS

A permutation of an array of integers is an arrangement of its members into a sequence or linear order.

For example, for arr = [1,2,3], the following are all the permutations of arr: [1,2,3], [1,3,2], [2, 1, 3], [2, 3, 1], [3,1,2], [3,2,1].
The next permutation of an array of integers is the next lexicographically greater permutation of its integer. More formally, if all the permutations of the array are sorted in one container according to their lexicographical order, then the next permutation of that array is the permutation that follows it in the sorted container. If such arrangement is not possible, the array must be rearranged as the lowest possible order (i.e., sorted in ascending order).

For example, the next permutation of arr = [1,2,3] is [1,3,2].
Similarly, the next permutation of arr = [2,3,1] is [3,1,2].
While the next permutation of arr = [3,2,1] is [1,2,3] because [3,2,1] does not have a lexicographical larger rearrangement.
Given an array of integers nums, find the next permutation of nums.

The replacement must be in place and use only constant extra memory.

 

Example 1:

Input: nums = [1,2,3]
Output: [1,3,2]
Example 2:

Input: nums = [3,2,1]
Output: [1,2,3]
Example 3:

Input: nums = [1,1,5]
Output: [1,5,1]
 

Constraints:

1 <= nums.length <= 100
0 <= nums[i] <= 100

//CODE
class Solution {
public:
    void nextPermutation(vector<int>& nums) {
      int n=nums.size();
      int i=n-2;
      while(i>=0 && nums[i]>=nums[i+1])
      {
        i--;
      }  
      if(i>=0)
      {
        int j=n-1;
        while(nums[j]<=nums[i])
        {
            j--;
        }
        swap(nums[i],nums[j]);
      }
      reverse(nums.begin()+i+1,nums.end());
    }
};



LEETCODE32//LONGEST VALID PARENTHESIS

Given a string containing just the characters '(' and ')', return the length of the longest valid (well-formed) parentheses substring.

 

Example 1:

Input: s = "(()"
Output: 2
Explanation: The longest valid parentheses substring is "()".
Example 2:

Input: s = ")()())"
Output: 4
Explanation: The longest valid parentheses substring is "()()".
Example 3:

Input: s = ""
Output: 0
 

Constraints:

0 <= s.length <= 3 * 104
s[i] is '(', or ')'.

//CODE
class Solution {
public:
    int longestValidParentheses(string s) {
   stack<int>st;
        st.push(-1);
        int maxlen=0;
        for(int i=0;i<s.length();i++)
            {
            char ch=s[i];
            if(ch=='(')
            {
              st.push(i);
                }
            else
                {
                st.pop();
                if(st.empty())
                    {
                    st.push(i);
                    }
                else
                    {
      int len=i-st.top();
     maxlen=max(len,maxlen);
                }
            }
        }
    return maxlen;
    }
};



LEETCODE33//SEARCH IN ROTATED SORTED ARRAY

There is an integer array nums sorted in ascending order (with distinct values).

Prior to being passed to your function, nums is possibly rotated at an unknown pivot index k (1 <= k < nums.length) such that the resulting array is [nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]] (0-indexed). For example, [0,1,2,4,5,6,7] might be rotated at pivot index 3 and become [4,5,6,7,0,1,2].

Given the array nums after the possible rotation and an integer target, return the index of target if it is in nums, or -1 if it is not in nums.

You must write an algorithm with O(log n) runtime complexity.

 

Example 1:

Input: nums = [4,5,6,7,0,1,2], target = 0
Output: 4
Example 2:

Input: nums = [4,5,6,7,0,1,2], target = 3
Output: -1
Example 3:

Input: nums = [1], target = 0
Output: -1
 

Constraints:

1 <= nums.length <= 5000
-104 <= nums[i] <= 104
All values of nums are unique.
nums is an ascending array that is possibly rotated.
-104 <= target <= 104


//CODE
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int l = 0;

    int r = nums.size() - 1;

    while (l <= r) {

      const int m = (l + r) / 2;

      if (nums[m] == target)

        return m;

      if (nums[l] <= nums[m]) {  // nums[l..m] are sorted.

        if (nums[l] <= target && target < nums[m])

          r = m - 1;

        else

          l = m + 1;

      } else {  // nums[m..n - 1] are sorted.

        if (nums[m] < target && target <= nums[r])

          l = m + 1;

        else

          r = m - 1;

      }

    }

    return -1;
    }
};



LEETCODE34//FIND FIRST AND LAST POSITION OF ELEMENT IN SORTED ARRAY

Given an array of integers nums sorted in non-decreasing order, find the starting and ending position of a given target value.

If target is not found in the array, return [-1, -1].

You must write an algorithm with O(log n) runtime complexity.

 

Example 1:

Input: nums = [5,7,7,8,8,10], target = 8
Output: [3,4]
Example 2:

Input: nums = [5,7,7,8,8,10], target = 6
Output: [-1,-1]
Example 3:

Input: nums = [], target = 0
Output: [-1,-1]
 

Constraints:

0 <= nums.length <= 105
-109 <= nums[i] <= 109
nums is a non-decreasing array.
-109 <= target <= 109


//CODE
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
        int l=lower_bound(nums.begin(),nums.end(),target)-nums.begin();
        int r=lower_bound(nums.begin(),nums.end(),target+1)-nums.begin();
        if(l==r)
        {
            return{-1,-1};
        }
        return {l,r-1};
    }
};



LEETCODE35//SEARCH INSERT POSITION

Given a sorted array of distinct integers and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.

You must write an algorithm with O(log n) runtime complexity.

 

Example 1:

Input: nums = [1,3,5,6], target = 5
Output: 2
Example 2:

Input: nums = [1,3,5,6], target = 2
Output: 1
Example 3:

Input: nums = [1,3,5,6], target = 7
Output: 4
 

Constraints:

1 <= nums.length <= 104
-104 <= nums[i] <= 104
nums contains distinct values sorted in ascending order.
-104 <= target <= 104

//CODE
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        int l=0;
        int r=nums.size();
        while(l<r)
            {
            const int m=(l+r)/2;
            if(nums[m]==target)
                
                return m;
                
            if(nums[m]<target)
                
                l=m+1;
                
            else
                
                r=m;
                
    }
        return l;
            
            }
};



LEETCODE36//VALID SUDOKU

Determine if a 9 x 9 Sudoku board is valid. Only the filled cells need to be validated according to the following rules:

Each row must contain the digits 1-9 without repetition.
Each column must contain the digits 1-9 without repetition.
Each of the nine 3 x 3 sub-boxes of the grid must contain the digits 1-9 without repetition.
Note:

A Sudoku board (partially filled) could be valid but is not necessarily solvable.
Only the filled cells need to be validated according to the mentioned rules.
 

Example 1:


Input: board = 
[["5","3",".",".","7",".",".",".","."]
,["6",".",".","1","9","5",".",".","."]
,[".","9","8",".",".",".",".","6","."]
,["8",".",".",".","6",".",".",".","3"]
,["4",".",".","8",".","3",".",".","1"]
,["7",".",".",".","2",".",".",".","6"]
,[".","6",".",".",".",".","2","8","."]
,[".",".",".","4","1","9",".",".","5"]
,[".",".",".",".","8",".",".","7","9"]]
Output: true
Example 2:

Input: board = 
[["8","3",".",".","7",".",".",".","."]
,["6",".",".","1","9","5",".",".","."]
,[".","9","8",".",".",".",".","6","."]
,["8",".",".",".","6",".",".",".","3"]
,["4",".",".","8",".","3",".",".","1"]
,["7",".",".",".","2",".",".",".","6"]
,[".","6",".",".",".",".","2","8","."]
,[".",".",".","4","1","9",".",".","5"]
,[".",".",".",".","8",".",".","7","9"]]
Output: false
Explanation: Same as Example 1, except with the 5 in the top left corner being modified to 8. Since there are two 8's in the top left 3x3 sub-box, it is invalid.
 

Constraints:

board.length == 9
board[i].length == 9
board[i][j] is a digit 1-9 or '.'.

//CODE
class Solution {
public:
    bool isValidSudoku(vector<vector<char>>& board) {
        int rowcase[9][9]={0};
        int colcase[9][9]={0};
        int gridcase[9][9]={0};
 for(int i=0;i<board.size();i++)
     {
for(int j=0;j<board[0].size();j++)
    {
    if(board[i][j]!='.')
        {
 int number=board[i][j]-'0';
        int k=i/3*3+j/3;
 if(rowcase[i][number-1]++||colcase[j][number-1]++||gridcase[k][number-1]++)
     
     return false;
     }
        }
    }
    
     return true;
            
    }
};



LEETCODE37//SUDOKU SOLVER

Write a program to solve a Sudoku puzzle by filling the empty cells.

A sudoku solution must satisfy all of the following rules:

Each of the digits 1-9 must occur exactly once in each row.
Each of the digits 1-9 must occur exactly once in each column.
Each of the digits 1-9 must occur exactly once in each of the 9 3x3 sub-boxes of the grid.
The '.' character indicates empty cells.

 

Example 1:


Input: board = [["5","3",".",".","7",".",".",".","."],["6",".",".","1","9","5",".",".","."],[".","9","8",".",".",".",".","6","."],["8",".",".",".","6",".",".",".","3"],["4",".",".","8",".","3",".",".","1"],["7",".",".",".","2",".",".",".","6"],[".","6",".",".",".",".","2","8","."],[".",".",".","4","1","9",".",".","5"],[".",".",".",".","8",".",".","7","9"]]
Output: [["5","3","4","6","7","8","9","1","2"],["6","7","2","1","9","5","3","4","8"],["1","9","8","3","4","2","5","6","7"],["8","5","9","7","6","1","4","2","3"],["4","2","6","8","5","3","7","9","1"],["7","1","3","9","2","4","8","5","6"],["9","6","1","5","3","7","2","8","4"],["2","8","7","4","1","9","6","3","5"],["3","4","5","2","8","6","1","7","9"]]
Explanation: The input board is shown above and the only valid solution is shown below:


 

Constraints:

board.length == 9
board[i].length == 9
board[i][j] is a digit or '.'.
It is guaranteed that the input board has only one solution.
//CODE

class Solution {
public:
    void solveSudoku(vector<vector<char>>& board) {

    solve(board, 0);
  }

 private:
  bool solve(vector<vector<char>>& board, int s) {
    if (s == 81)
      return true;

    const int i = s / 9;
    const int j = s % 9;

    if (board[i][j] != '.')
      return solve(board, s + 1);

    for (char c = '1'; c <= '9'; ++c)
      if (isValid(board, i, j, c)) {
        board[i][j] = c;
        if (solve(board, s + 1))
          return true;
        board[i][j] = '.';
      }

    return false;
  }

  bool isValid(vector<vector<char>>& board, int row, int col, char c) {
    for (int i = 0; i < 9; ++i)
      if (board[i][col] == c || board[row][i] == c ||
          board[3 * (row / 3) + i / 3][3 * (col / 3) + i % 3] == c)
        return false;
    return true;
    }
};




LEETCODE38//COUNT AND SAY

The count-and-say sequence is a sequence of digit strings defined by the recursive formula:

countAndSay(1) = "1"
countAndSay(n) is the run-length encoding of countAndSay(n - 1).
Run-length encoding (RLE) is a string compression method that works by replacing consecutive identical characters (repeated 2 or more times) with the concatenation of the character and the number marking the count of the characters (length of the run). For example, to compress the string "3322251" we replace "33" with "23", replace "222" with "32", replace "5" with "15" and replace "1" with "11". Thus the compressed string becomes "23321511".

Given a positive integer n, return the nth element of the count-and-say sequence.

 

Example 1:

Input: n = 4

Output: "1211"

Explanation:

countAndSay(1) = "1"
countAndSay(2) = RLE of "1" = "11"
countAndSay(3) = RLE of "11" = "21"
countAndSay(4) = RLE of "21" = "1211"
Example 2:

Input: n = 1

Output: "1"

Explanation:

This is the base case.

 

Constraints:

1 <= n <= 30

//CODE
class Solution {
public:
    string countAndSay(int n) {
        
        if(n==1)
            {
            return "1";
        }
       
            
 string say=countAndSay(n-1);
            
        string result ="";
        
        for(int i=0;i<say.length();i++)
            {
            char ch=say[i];
            int count=1;
            while(i<say.length()-1&&say[i]==say[i+1])
                {
                count++;
                i++;
                }
result += to_string(count) + string(1,ch);
            }
        return result ;
    }
};



LEETCODE39//COMBINATION SUM

Given an array of distinct integers candidates and a target integer target, return a list of all unique combinations of candidates where the chosen numbers sum to target. You may return the combinations in any order.

The same number may be chosen from candidates an unlimited number of times. Two combinations are unique if the frequency of at least one of the chosen numbers is different.

The test cases are generated such that the number of unique combinations that sum up to target is less than 150 combinations for the given input.

 

Example 1:

Input: candidates = [2,3,6,7], target = 7
Output: [[2,2,3],[7]]
Explanation:
2 and 3 are candidates, and 2 + 2 + 3 = 7. Note that 2 can be used multiple times.
7 is a candidate, and 7 = 7.
These are the only two combinations.
Example 2:

Input: candidates = [2,3,5], target = 8
Output: [[2,2,2,2],[2,3,3],[3,5]]
Example 3:

Input: candidates = [2], target = 1
Output: []
 

Constraints:

1 <= candidates.length <= 30
2 <= candidates[i] <= 40
All elements of candidates are distinct.
1 <= target <= 40


//CODE
class Solution {
public:
    vector<int> candidates2;

    vector<vector<int>> result;

    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {

        sort(candidates.begin(),candidates.end());

        vector<int> combine;

        candidates2=candidates;

        combination(target,0, combine);

        return result;

    }

    

    void combination(int target, int start,vector<int> &combine){

        if(target==0){

            result.push_back(combine);

            return;

        }

        for(int i=start;i<candidates2.size();i++){

            if(target<candidates2[i])break;

            combine.push_back(candidates2[i]);

            combination(target-candidates2[i],i,combine);

            combine.pop_back();

        }
        
    }
};



LEETCODE40//COMBINATION SUM2

Given a collection of candidate numbers (candidates) and a target number (target), find all unique combinations in candidates where the candidate numbers sum to target.

Each number in candidates may only be used once in the combination.

Note: The solution set must not contain duplicate combinations.

 

Example 1:

Input: candidates = [10,1,2,7,6,1,5], target = 8
Output: 
[
[1,1,6],
[1,2,5],
[1,7],
[2,6]
]
Example 2:

Input: candidates = [2,5,2,1,2], target = 5
Output: 
[
[1,2,2],
[5]
]
 

Constraints:

1 <= candidates.length <= 100
1 <= candidates[i] <= 50
1 <= target <= 30

//CODE
class Solution {
public:
    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
       




    vector<vector<int>> ans;

    ranges::sort(candidates);

    dfs(candidates, 0, target, {}, ans);

    return ans;

  }

 private:

  void dfs(const vector<int>& A, int s, int target, vector<int>&& path,

           vector<vector<int>>& ans) {

    if (target < 0)

      return;

    if (target == 0) {

      ans.push_back(path);

      return;

    }

    for (int i = s; i < A.size(); ++i) {

      if (i > s && A[i] == A[i - 1])

        continue;

      path.push_back(A[i]);

      dfs(A, i + 1, target - A[i], std::move(path), ans);

      path.pop_back();

    }

  


    }
};



LEETCODE41//FIND MISSING POSITIVE

Given an unsorted integer array nums. Return the smallest positive integer that is not present in nums.

You must implement an algorithm that runs in O(n) time and uses O(1) auxiliary space.

 

Example 1:

Input: nums = [1,2,0]
Output: 3
Explanation: The numbers in the range [1,2] are all in the array.
Example 2:

Input: nums = [3,4,-1,1]
Output: 2
Explanation: 1 is in the array but 2 is missing.
Example 3:

Input: nums = [7,8,9,11,12]
Output: 1
Explanation: The smallest positive integer 1 is missing.
 

Constraints:

1 <= nums.length <= 105
-231 <= nums[i] <= 231 - 1

//CODE
class Solution {
public:
    int firstMissingPositive(vector<int>& nums) {
   int size = nums.size(); // Get the size of the array
      
        // Process each element in the array
        for (int i = 0; i < size; ++i) {
            // Continue swapping elements until the current element is out of bounds
            // or it is in the correct position (value at index i should be i + 1)
            while (nums[i] >= 1 && nums[i] <= size && nums[nums[i] - 1] != nums[i]) {
                // Swap the current element to its correct position
                swap(nums[i], nums[nums[i] - 1]);
            }
        }
      
        // After rearranging, find the first position where the index does not match the value
        for (int i = 0; i < size; ++i) {
            if (nums[i] != i + 1) {
                // If such a position is found, the missing integer is i + 1
                return i + 1;
            }
        }
      
        // If all positions match, the missing integer is size + 1
        return size + 1;
    }

private:
    // Helper function to swap two elements in the array
    void swap(int& a, int& b) {
        int temp = a;
        a = b;
        b = temp;
    }
};

       

LEETCODE42//TRAPPING RAIN WATER

Given n non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it can trap after raining.

 

Example 1:


Input: height = [0,1,0,2,1,0,1,3,2,1,2,1]
Output: 6
Explanation: The above elevation map (black section) is represented by array [0,1,0,2,1,0,1,3,2,1,2,1]. In this case, 6 units of rain water (blue section) are being trapped.
Example 2:

Input: height = [4,2,0,3,2,5]
Output: 9
 

Constraints:

n == height.length
1 <= n <= 2 * 104
0 <= height[i] <= 105

//CODE
class Solution {
public:
    int trap(vector<int>& height) {
       




    if (height.empty())

      return 0;

    int ans = 0;

    int l = 0;

    int r = height.size() - 1;

    int maxL = height[l];

    int maxR = height[r];

    while (l < r)

      if (maxL < maxR) {

        ans += maxL - height[l];

        maxL = max(maxL, height[++l]);

      } else {

        ans += maxR - height[r];

        maxR = max(maxR, height[--r]);

      }

    return ans;

  


    }
};



LEETCODE43//MULTILY STRING

Given two non-negative integers num1 and num2 represented as strings, return the product of num1 and num2, also represented as a string.

Note: You must not use any built-in BigInteger library or convert the inputs to integer directly.

 

Example 1:

Input: num1 = "2", num2 = "3"
Output: "6"
Example 2:

Input: num1 = "123", num2 = "456"
Output: "56088"
 

Constraints:

1 <= num1.length, num2.length <= 200
num1 and num2 consist of digits only.
Both num1 and num2 do not contain any leading zero, except the number 0 itself.


//CODE

class Solution {
public:
    string multiply(string num1, string num2) {
        if(num1=="0" || num2 == "0")
        {
            return "0";
        }
        reverse(num1.begin(),num1.end());
        reverse(num2.begin(),num2.end());
        int N=num1.size() + num2.size();
        string answer(N,'0');
        for(int place2 = 0; place2<num2.size(); place2++)
        {
            int digit2 = num2[place2]-'0';
            for(int place1=0;place1<num1.size();place1++)
            {
                int digit1=num1[place1]-'0';
                int numZeros=place1+place2;
                int carry=answer[numZeros]-'0';
                int multiplication = digit1  * digit2+carry;
                answer[numZeros]=(multiplication % 10) + '0';
                answer[numZeros+1]+=(multiplication/10);

            }
        }
        if(answer.back()=='0')
        {
            answer.pop_back();
        }
        reverse(answer.begin(),answer.end());
        return answer;
    }
};



LEETCODE44//WILDCARD MATCHING(DP)

Given an input string (s) and a pattern (p), implement wildcard pattern matching with support for '?' and '*' where:

'?' Matches any single character.
'*' Matches any sequence of characters (including the empty sequence).
The matching should cover the entire input string (not partial).

 

Example 1:

Input: s = "aa", p = "a"
Output: false
Explanation: "a" does not match the entire string "aa".
Example 2:

Input: s = "aa", p = "*"
Output: true
Explanation: '*' matches any sequence.
Example 3:

Input: s = "cb", p = "?a"
Output: false
Explanation: '?' matches 'c', but the second letter is 'a', which does not match 'b'.
 

Constraints:

0 <= s.length, p.length <= 2000
s contains only lowercase English letters.
p contains only lowercase English letters, '?' or '*'.

//CODE
class Solution {
public:
    bool isMatch(string s, string p) {
    

 



    const int m = s.length();

    const int n = p.length();

    // dp[i][j] := true if s[0..i) matches p[0..j)

    vector<vector<bool>> dp(m + 1, vector<bool>(n + 1));

    dp[0][0] = true;

    auto isMatch = [&](int i, int j) -> bool {

      return j >= 0 && p[j] == '?' || s[i] == p[j];

    };

    for (int j = 0; j < p.length(); ++j)

      if (p[j] == '*')

        dp[0][j + 1] = dp[0][j];

    for (int i = 0; i < m; ++i)

      for (int j = 0; j < n; ++j)

        if (p[j] == '*') {

          const bool matchEmpty = dp[i + 1][j];

          const bool matchSome = dp[i][j + 1];

          dp[i + 1][j + 1] = matchEmpty || matchSome;

        } else if (isMatch(i, j)) {

          dp[i + 1][j + 1] = dp[i][j];

        }

    return dp[m][n];

  


    }
};



LEETCODE46//PERMUTATIONS

Given an array nums of distinct integers, return all the possible permutations. You can return the answer in any order.

 

Example 1:

Input: nums = [1,2,3]
Output: [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
Example 2:

Input: nums = [0,1]
Output: [[0,1],[1,0]]
Example 3:

Input: nums = [1]
Output: [[1]]
 

Constraints:

1 <= nums.length <= 6
-10 <= nums[i] <= 10
All the integers of nums are unique.
Seen this question in a re

//CODE
class Solution {
public:
void backtrack(vector<int>&nums,vector<bool>&used,vector<int>&current,vector<vector<int>>&result)
{
    if(current.size()==nums.size())
    {
        result.push_back(current);
        return;
    }
    for(int i=0;i<nums.size();i++)
    {
        if(used[i])continue;
        used[i]=true;
        current.push_back(nums[i]);
        backtrack(nums,used,current,result);
        used[i]=false;
        current.pop_back();
    }
}
    vector<vector<int>> permute(vector<int>& nums) {
        vector<vector<int>>result;
        vector<int>current;
        vector<bool>used(nums.size(),false);
        backtrack(nums,used,current,result);
        return result;
    }
};



LEETCODE47//permutation2

Given a collection of numbers, nums, that might contain duplicates, return all possible unique permutations in any order.

 

Example 1:

Input: nums = [1,1,2]
Output:
[[1,1,2],
 [1,2,1],
 [2,1,1]]
Example 2:

Input: nums = [1,2,3]
Output: [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
 
//code
class Solution {
public:
void backtrack(vector<int>& nums,vector<bool>& used,vector<int>& temp,vector<vector<int>>& result)
{
    if(temp.size()==nums.size())
    {
        result.push_back(temp);
        return;
    }
    for(int i=0;i<nums.size();i++)
    {
        if(used[i])continue;
        if(i>0 && nums[i]==nums[i-1]&& !used[i-1])continue;
        used[i]=true;
        temp.push_back(nums[i]);
        backtrack(nums,used,temp,result);
        used[i]=false;
        temp.pop_back();
    }
}
    vector<vector<int>> permuteUnique(vector<int>& nums) {
     sort(nums.begin(),nums.end());
     vector<vector<int>>result;
     vector<int>temp;
     vector<bool>used(nums.size(),false); 
     backtrack(nums,used,temp,result);
     return result;  
    }
};



LEETCODE48//ROTATE IMAGE

You are given an n x n 2D matrix representing an image, rotate the image by 90 degrees (clockwise).

You have to rotate the image in-place, which means you have to modify the input 2D matrix directly. DO NOT allocate another 2D matrix and do the rotation.

 

Example 1:


Input: matrix = [[1,2,3],[4,5,6],[7,8,9]]
Output: [[7,4,1],[8,5,2],[9,6,3]]
Example 2:


Input: matrix = [[5,1,9,11],[2,4,8,10],[13,3,6,7],[15,14,12,16]]
Output: [[15,13,2,5],[14,3,4,1],[12,6,8,9],[16,7,10,11]]
 

Constraints:

n == matrix.length == matrix[i].length
1 <= n <= 20
-1000 <= matrix[i][j] <= 1000

//CODE
class Solution {
public:
    void rotate(vector<vector<int>>& matrix) {
        int n=matrix.size();
        for(int i=0;i<n;i++)
        {
            for(int j=i;j<n;j++)
            {
                swap(matrix[i][j],matrix[j][i]);
            }
        }
        for(int i=0;i<n;i++)
        {
            reverse(matrix[i].begin(),matrix[i].end());
        }
    }
};



LEETCODE49//GROUP ANARGAMS

Given an array of strings strs, group the anagrams together. You can return the answer in any order.

 

Example 1:

Input: strs = ["eat","tea","tan","ate","nat","bat"]

Output: [["bat"],["nat","tan"],["ate","eat","tea"]]

Explanation:

There is no string in strs that can be rearranged to form "bat".
The strings "nat" and "tan" are anagrams as they can be rearranged to form each other.
The strings "ate", "eat", and "tea" are anagrams as they can be rearranged to form each other.
Example 2:

Input: strs = [""]

Output: [[""]]

Example 3:

Input: strs = ["a"]

Output: [["a"]]

 

Constraints:

1 <= strs.length <= 104
0 <= strs[i].length <= 100
strs[i] consists of lowercase English letters.

//CODE
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
    vector<vector<string>>ans;
        unordered_map<string,vector<string>>umap;
        for(auto x:strs)
        {
            string temp=x;
            sort(x.begin(),x.end());
            umap[x].push_back(temp);
        }
        for(auto x:umap)
        {
            ans.push_back(x.second);
        }
        return ans;
    }
};



LEETCODE50//POW(X,N)

mplement pow(x, n), which calculates x raised to the power n (i.e., xn).

 

Example 1:

Input: x = 2.00000, n = 10
Output: 1024.00000
Example 2:

Input: x = 2.10000, n = 3
Output: 9.26100
Example 3:

Input: x = 2.00000, n = -2
Output: 0.25000
Explanation: 2-2 = 1/22 = 1/4 = 0.25
 

Constraints:

-100.0 < x < 100.0
-231 <= n <= 231-1
n is an integer.
Either x is not zero or n > 0.

//CODE
class Solution {
public:
    double myPow(double x, int n) {
    long long N=n;
    if(N<0)
    {
        x=1/x;
        N=-N;
    }    
    double result=1;
    while(N)
    {
        if(N%2==1)
        {
            result *=x;
        }
        x *=x;
        N/=2;
    }
    return result;
    }
};



 
