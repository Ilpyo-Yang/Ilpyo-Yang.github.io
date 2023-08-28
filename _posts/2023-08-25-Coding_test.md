---
title: Coding Test
author: Rosie Yang
date: 2023-08-25
category: job
layout: post
---

## LeetCode - Top Interview 150
+ []()
+ [209. Minimum Size Subarray Sum](/job/2023/08/25/Coding_test.html#209-minimum-size-subarray-sum)
+ [167. Two Sum II - Input Array Is Sorted](/job/2023/08/25/Coding_test.html#167-two-sum-2-input-array-in-sorted)
+ [125. Valid Palindrome](/job/2023/08/25/Coding_test.html#125-valid-palindrome)
+ [169. Majority Element](/job/2023/08/25/Coding_test.html#169-majority-element)
+ [80. Remove Duplicates from Sorted Array II](/job/2023/08/25/Coding_test.html#80-remove-duplicates-from-sorted-array-2)
+ [26. Remove Duplicates from Sorted Array](/job/2023/08/25/Coding_test.html#26-remove-duplicates-from-sorted-array)
+ [27. Remove Element](/job/2023/08/25/Coding_test.html#27-remove-element)
+ [88. Merge Sorted Array](/job/2023/08/25/Coding_test.html#88-merge-sorted-array)

<br>

---
<br>

### 209. Minimum Size Subarray Sum
#### 1. 문제
[문제 URL](https://leetcode.com/problems/minimum-size-subarray-sum/?envType=study-plan-v2&envId=top-interview-150)
+ 배열의 요소들의 합으로 ```target```이 되기 위한 서브 배열의 최소 길이를 구하는 문제입니다.
+ 배열 ```nums```는 정렬되지 않았습니다.
+ 서브배열이 없는 경우에는 ```0```을 return 합니다.
+ 시간복잡도가 O(n) 또는 O(n log n)인 방법을 찾아습니다.

#### 2. 나의 풀이
##### 시도 1
이중루프 밖에 생각나지 않아서 이렇게 시도했지만 역시나 시간초과로 실패했습니다.
+ 시간복잡도: O(n log n)
+ 공간복잡도: O(1)

```java
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        if(Arrays.binarySearch(nums, target)>=0) return 1;
        int j=1, cnt=0;
        boolean b = false;
        for(int i=0; i<nums.length; i++){
            int value = nums[i];
            j = i+1;
            while(true){
                if(j<=nums.length-1) value += nums[j];
                if(target==value){
                    b=true;
                    break;
                }
                if(j==nums.length-1 || target<value) break;
            }
            if(b) cnt = j-i+1;
        }
        return cnt;
    }
}
```

#### 3. 다른 사람의 풀이를 보고 개선하기
```슬라이딩 윈도우```를 이용한 풀이었는데, 앞에서 나의 풀이와 같이 for문과 while문을 함께 쓰는 방식인데 효율적이라는 부분이 달랐습니다.   

먼저 서브배열이 없는 경우 ```0```을 반환합니다. 그리고 부분 배열의 합이 target 이상이 되도록 left 포인터를 이동시키면서 최소 길이를 갱신합니다.
+ 시간복잡도: O(n)
+ 공간복잡도: O(1)

```java
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        if (nums == null || nums.length == 0) {
            return 0;
        }

        int left = 0;
        int minLength = Integer.MAX_VALUE;
        int currentSum = 0;

        for (int right = 0; right < nums.length; right++) {
            currentSum += nums[right];

            while (currentSum >= target) {
                minLength = Math.min(minLength, right - left + 1);
                currentSum -= nums[left];
                left++;
            }
        }

        if (minLength != Integer.MAX_VALUE) 
            return minLength;
        else 
            return 0;
    }
}
```

#### 4. 생각해 볼 부분
두 포인터를 이용한 방법과는 약간 상이했는데, 슬라이딩 윈도우를 사용하는 방법에 좀 더 능숙해져야 겠다고 생각했습니다.

---
<br>

### 167. Two Sum 2 - Input Array Is Sorted
#### 1. 문제
[문제 URL](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/?envType=study-plan-v2&envId=top-interview-150)
+ 정렬된 배열 내 두 요소를 더한 값으로 원하는 값을 만드는 문제입니다.
+ 이 때 그 배열 내 두 요소의 순서 쌍을 return 합니다.

#### 2. 나의 풀이
##### 시도 1
```Arrays.binarySearch```로 짝이 되는 index를 구할 수 있었습니다.
+ 이렇게 풀었을 때 기본적인 예제는 다 통과했지만, ```[5,25,75]```에서 ```100```이 될 수 있는 값을 찾는 것에서 실패했습니다.
+ output이 ```[1,-3]```이 나왔는데 ```indexOf```와 착각하고 ```-1```만을 반환한다고 가정했기 때문입니다.
  + ```Arrays.binarySearch```은 찾지 못한 경우 음수 값을 반환합니다. 반환 값은 key의 정확한 위치가 아니라 key를 삽입해야 할 위치에 대한 음수 값을 반환합니다.

```java
class Solution {
    public int[] twoSum(int[] numbers, int target) {
        int i=0, j=0;
        for(i=0; i<numbers.length; i++){
            j = Arrays.binarySearch(numbers, target-numbers[i]);
            if(j>0) break;
        }
        return new int[]{i+1, j+1};
    }
}
```
##### 시도 2
위에서 ```Arrays.binarySearch```의 음수 문제, 동일한 숫자가 연속으로 있을 때의 같은 인덱스 반환문제, 반환 배열의 정렬문제 등을 고려해서 코드를 수정했습니다.
+ 시간복잡도: O(n log n)
+ 공간복잡도: O(1)

```java
class Solution {
    public int[] twoSum(int[] numbers, int target) {
        int i=0, j=0;
        for(i=0; i<numbers.length; i++){
            j = Arrays.binarySearch(numbers, target-numbers[i]);
            if(j>=0 && i!=j) break;
        }
        if(i<j) return new int[]{i+1, j+1};
        else return new int[]{j+1, i+1};
    }
}
```

#### 3. 다른 사람의 풀이를 보고 개선하기
주어진 ```nums``` 배열이 정렬된 값이므로 양쪽의 값을 더해서 ```target```보다 작은지 큰지에 따라 포인터를 변경해주는 방식이었습니다.
코드도 훨씬 깔끔하고 이 접근법을 기억해두는게 좋겠다는 생각이 들었습니다.
```java
class Solution {
  public int[] twoSum(int[] nums, int target) {
    int l = 0, r = nums.length - 1;
    while (nums[l] + nums[r] != target) {
      if (nums[l] + nums[r] < target)
        l++;
      else
        r--;
    }
    return new int[]{l + 1, r + 1};
  }
}
```

#### 4. 생각해 볼 부분
참고한 풀이 작성자가 쓴 내용에서 정렬된 배열이 주어졌을 때 고려해볼 내용으로 다음 내용들을 추천했습니다. 이 문제에서는 두 포인터를 이용한 방식을 사용한 방법이었습니다.
+ 이진 검색
+ 두 개(또는 세 개)의 포인터
+ 슬라이딩 윈도우
  + 슬라이딩 윈도우 알고리즘의 핵심 아이디어는 윈도우의 시작과 끝을 조절하면서 필요한 연산을 수행하는 것입니다. 이를 통해 불필요한 계산을 줄이고, 문제의 복잡도를 줄이며, 효율적으로 문제를 해결할 수 있습니다.
+ 오른쪽부터 순회

---
<br>

### 125. Valid Palindrome
#### 1. 문제
[문제 URL](https://leetcode.com/problems/valid-palindrome/?envType=study-plan-v2&envId=top-interview-150)
+ Palindrome인지를 찾는 문제입니다. 예를 들어 토마토, 기러기, 우영우 같은 문자열 여부를 확인합니다.
  + ```true``` or ```false```로 반환
+ 특수문자와 공백은 제외하고 영자 대소문자는 동일하게 간주합니다.
+ ```" "``` 빈 문자열도 Palindrome으로 간주합니다.

#### 2. 나의 풀이
##### 시도 1
문자열을 char[] 배열로 변환해서 뒤의 배열의 값과 비교하는 방식으로 풀이했습니다. 
+ 이 경우, 458 / 485 테스트케이스는 통과했지만 ```0P```는 통과하지 못했습니다.
  + 문제에서 ```Alphanumeric characters include letters and numbers.```로 숫자도 고려해야 하므로 수정이 필요했습니다.

```java
class Solution {
    public boolean isPalindrome(String s) {
        if(s.isEmpty()) return true;
        int end = s.length()-1;
        char[] arr = s.toCharArray();
        for(int i=0; i<arr.length/2; i++){
            if(!(arr[i] >= 'A' && arr[i] <= 'Z') && !(arr[i] >= 'a' && arr[i] <= 'z'))
                continue;
            while(true){
                if((arr[end] >= 'A' && arr[end] <= 'Z') || (arr[end] >= 'a' && arr[end] <= 'z'))
                    break;
                else end--;    
            }
            if(Character.toLowerCase(arr[i]) != Character.toLowerCase(arr[end]))            
                return false;
            else end--;
        }
        return true;
    }
}
```  
##### 시도 2
위 코드에서 숫자를 다시 반영한 코드로 작성했지만 시간초과가 발생했습니다.
+ ```!(start >= 0 && start <= 9) && !(start >= 'a' && start <= 'z')```

```java
class Solution {
    public boolean isPalindrome(String s) {
        if(s.isEmpty()) return true;
        int end = s.length()-1;
        char[] arr = s.toCharArray();
        for(int i=0; i<arr.length/2; i++){
            int start = Character.toLowerCase(arr[i]);
            int compare = Character.toLowerCase(arr[end]);
            if(!(start >= 0 && start <= 9) && !(start >= 'a' && start <= 'z'))
                continue;
            while(true){
                if((compare >= 0 && compare <= 9) || (compare >= 'a' && compare <= 'z'))
                    break;
                else end--;    
            }
            if(start != compare) return false;
            else end--;
        }
        return true;
    }
}
```
##### 시도 3
이중 반복문 때문에 발생한 것 같아서 다른 방법을 생각해보게 되었습니다. while문으로 처리하지 않고 미리 ```s```의 숫자와 영문자 외 다른 문자는 공백처리하는 방법입니다. 
+ 이 경우, 462 / 485 테스트케이스는 통과했지만 ```0P```는 통과하지 못했습니다.
  + ```if(!(arr[i] >= 0 && arr[i] <= 9) && !(arr[i] >= 'a' && arr[i] <= 'z')) arr[i]=' ';```
  + 이 부분에서 ```0```을 공백처리하고 있었는데, ```if(!(arr[i] >= '0' && arr[i] <= '9') && !(arr[i] >= 'a' && arr[i] <= 'z')) arr[i]=' ';
    }```로 변경해주었습니다.

결과적으로 통과는 했지만 마음에 드는 풀이는 아니었습니다.
+ 시간복잡도: O(n)
+ 공간복잡도: O(n)

```java
class Solution {
    public boolean isPalindrome(String s) {
        if(s.isEmpty()) return true;
        s = s.toLowerCase();
        char[] arr = s.toCharArray();
        for(int i=0; i<arr.length; i++){
            if(!(arr[i] >= '0' && arr[i] <= '9') && !(arr[i] >= 'a' && arr[i] <= 'z')) arr[i]=' ';
        }
        String str = String.valueOf(arr).replaceAll(" ", "");
        arr = str.toCharArray();
        for(int i=0, j=arr.length-1; i<arr.length/2; i++, j--){
            if(arr[i]!=arr[j]) return false;
        }
        return true;
    }
}
```

#### 3. 다른 사람의 풀이를 보고 개선하기
```Character.isLetterOrDigit```를 사용해서 코드를 확실히 깔끔하게 처리한 것을 볼 수 있었습니다. 
그리고 저는 배열로 변환했다가 문자열로 합치는 과정을 거쳤는데 그냥 반복문을 돌면서 문자열의 문자 위치를 ```left```와 ```right```로 처리해서 비교하는 법을 배웠습니다.
+ 시간복잡도: O(n)
+ 공간복잡도: O(1)

```java
class Solution {
   public boolean isPalindrome(String s) {
        if (s.isEmpty()) {
            return true;
        }

        int left = 0;
        int right = s.length() - 1;

        while (left < right) {
            char leftChar = s.charAt(left);
            char rightChar = s.charAt(right);

            if (!Character.isLetterOrDigit(leftChar)) {
                left++;
            } else if (!Character.isLetterOrDigit(rightChar)) {
                right--;
            } else {
                if (Character.toLowerCase(leftChar) != Character.toLowerCase(rightChar)) {
                    return false;
                }
                left++;
                right--;
            }
        }

        return true;
    }
}
```

#### 4. 생각해 볼 부분
너무 복잡하게 생각하지 말고, 기존 문자열을 최대한 변환하지 않는 선에서 문제를 풀어야겠다는 생각이 들었습니다.

----
<br>

### 169. Majority Element
#### 1. 문제
[문제 URL](https://leetcode.com/problems/majority-element/?envType=study-plan-v2&envId=top-interview-150)
+ 배열에서 최대 빈도 값을 구하는 문제입니다. 
+ 최빈값은 적어도 ```배열의 길이/2``` 이상의 빈도를 갖습니다.
+ 추가로 시간복잡도 O(n), 공간복잡도 O(1)의 방법으로 풀 수 있는지 알아보는 문제입니다.

#### 2. 나의 풀이
먼저 제가 생각한 방식은 map에 같은 키를 가지는 값을 넣어주고 중복된 키를 갖는 경우 중복된 수를 값으로 넣어주는 방식이었습니다.
통과는 했지만 문제에서 고려해봐야할 복잡도 부분에서는 부족했습니다.
+ 시간 복잡도: O(n + n log n)
+ 공간 복잡도: O(n)

```java
class Solution {
  public int majorityElement(int[] nums) {
    int limit = nums.length/2;
    Map<Integer, Integer> map = new HashMap<>();
    for(int i:nums){
      if(map.containsKey(i) map.put(i, map.get(i)+1);
      else map.put(i, 1);
    }
    List<Integer> keySet = new ArrayList<>(map.keySet());
    keySet.sort((o1, o2) -> map.get(o2).compareTo(map.get(o1)));
    return keySet.get(0);
  }
}
```

#### 3. 다른 사람의 풀이를 보고 개선하기
**```Arrays.sort(nums)``` 이용하기**  
가장 먼저 본 풀이는 이 방법이었는데 이렇게 된다고 하는 생각이 들었지만, 생각해보니 ```limit``` 부분을 문제 풀면서 잊고 있었습니다.
최대 빈도 값은 ```배열의 길이/2``` 이상의 빈도를 갖기 때문에 정렬로 중앙값만 도출해도 값은 값이 나올 수 있는 문제였습니다.
하지만 원하는 시간, 공간 복잡도는 아니였습니다.
+ 시간 복잡도: O(n log n)
+ 공간 복잡도: O(1)

```java
class Solution {
    public int majorityElement(int[] nums) {
        Arrays.sort(nums);
        int n = nums.length;
        return nums[n/2];
    }
}
```

**Moore Voting Algorithm**  
과반수 요소를 구하기 위한 Moore Voting Algorithm입니다.
+ 변수 candidate를 과반수 요소 후보로 초기화하고, count를 0으로 초기화합니다.
  + 배열을 순회하면서 각 요소에 대해 다음을 수행합니다.
  + 만약 count가 0이라면 현재 요소를 candidate로 설정합니다.
  + 만약 현재 요소가 candidate와 같다면 count를 증가시킵니다.
  + 그렇지 않으면 count를 감소시킵니다.
  
+ 시간 복잡도: O(n)
+ 공간 복잡도: O(1)

```java
class Solution {
    public int majorityElement(int[] nums) {
        int count = 0;
        int candidate = 0;
        
        for (int num : nums) {
            if (count == 0) {
                candidate = num;
            }
            if (num == candidate) {
                count++;
            } else {
                count--;
            }
        }
        return candidate;
    }
}
```

#### 4. 생각해 볼 부분
**Moore Voting Algorithm**이 생소했는데 이번 기회에 과반수 이상의 요소를 구하기 위해 좋은 알고리즘에 대해 공부하게 되었습니다.  
위에서 가장 이해되지 않았던 부분이 ```candidate = num;``` 부분이었는데 처음에 값을 넣는 건 당연했지만 값을 넣은 이후에 다른 요소로 빈도로 그 값(count)을 줄이고 0이 되면 다른 요소를 후보자로 넣는다는게 처음에 이해되지 않았습니다. 

하지만 간단하게 예제로 주어진 배열 ```2,2,1,1,1,2,2```을 넣어서 생각해보면 오히려 쉬웠는데 결국에는 이 알고리즘이 **과반수 이상의 요소가 있다는 전제**가 있었기 때문에 가능하므로 이 부분을 고려할 필요가 있습니다.

---
<br>

### 80. Remove Duplicates from Sorted Array 2
#### 1. 문제
[문제 URL](https://leetcode.com/problems/remove-duplicates-from-sorted-array-ii/?envType=study-plan-v2&envId=top-interview-150)
+ 주어진 배열에서 순서대로 동일한 요소가 3개 이상 연속되지 않도록 만들어야 하는 문제입니다.
+ ```1,1,2,2,2,3```일 때, ```1,1,2,2,3,_```으로 만들고 그 요소의 수를 return합니다.

#### 2. 나의 풀이
먼저 어떻게 풀지에 대해 고민했는데 26번 문제를 풀 때 익힌 포인터 방식으로 풀고 싶었는데 두 번째 if문에서 방법이 생각나지 않았습니다..

```java
class Solution {
  public int removeDuplicates(int[] nums) {
    int a=1, cnt=0;
    for(int i=1; i<nums.length; i++){
      if(nums[i]==nums[i-1]) cnt++;
      if(cnt>=2 || nums[i]!=nums[i-1]){
        //cnt=0;
        nums[a++]=nums[i];
      }

    }
    return a;
  }
}
```

#### 3. 다른 사람의 풀이를 보고 개선하기
```java
class Solution {
    public int removeDuplicates(int[] nums) {
        if (nums.length <= 2) {
            return nums.length;
        }
        int index = 2;
        for (int i = 2; i < nums.length; i++) {
            if (nums[i] != nums[index - 2]) {
                nums[index] = nums[i];
                index++;
            }
        }
        return index;
    }
}
```
+ 먼저 ```nums```가 2보다 작다면 당연히 별도의 계산이 필요하지 않으므로 그 길이를 return 합니다.
+ 그리고 시작되는 ```i```와 ```index```도 2의 값부터 진행될 수 있도록 합니다.
+ 시간복잡도: O(n)
+ 공간복잡도: O(1)

#### 4. 생각해 볼 부분
```java
if (nums[i] != nums[index - 2]) {
    nums[index] = nums[i];
    index++;
}
```
이 부분에 대한 내용이 헷갈렸는데 실제로 ```index```로 들어간 배열들은 차곡차곡 중복을 2개까지 허용한 값들이고
```nums[i]```의 값이 기존에 값들과 다른 경우(새로 쌓은 배열과 다른 경우), ```nums[index] = nums[i];```로 다른 요소를 넣어줍니다.

---
<br>

### 26. Remove Duplicates from Sorted Array
#### 1. 문제
[문제 URL](https://leetcode.com/problems/remove-duplicates-from-sorted-array/?envType=study-plan-v2&envId=top-interview-150)
+ 정렬된 배열 ```nums```의 중복값을 제외한 요소의 수를 return하는 문제입니다.
+ 배열의 중복값은 없어야 합니다.

#### 2. 나의 풀이
```java
class Solution {
    public int removeElement(int[] nums, int val) {
        nums = Arrays.stream(nums).distinct().toArray();
        return nums.length;
    }
}
```
IDE에서와 달리 ```nums``` 값의 output이 중복값이 제거되지 않은 상태로 나와서 다시 고민하게 되었습니다.
+ 시간복잡도: O(n)
+ 공간복잡도: O(n)

#### 3. 다른 사람의 풀이를 보고 개선하기
```java
class Solution {
    public int removeDuplicates(int[] nums) {
        int a=1;
        for(int i=1; i<nums.length; i++){
            if(nums[i]!=nums[i-1]){
                nums[a++]=nums[i];
            }
        }
        return a;
    }
}
```
```nums```의 값이 앞의 값과 같지 않으면, 인덱스 a 위치에 대입하게 됩니다.

#### 4. 생각해 볼 부분
stream에서 사용되는 ```distinct()```와 ```toArray()```는 객체를 새로 생성, 메모리를 추가로 필요로 한다는 점에서 공간복잡도 결과가 좋지 않게 나왔습니다.  
새로운 객체 생성보다는 주어진 ```nums``` 객체 값에 변화를 주는 방식으로 해결하는 것을 생각해봐야 합니다.

---
<br>

### 27. Remove Element
#### 1. 문제
[문제 URL](https://leetcode.com/problems/remove-element/?envType=study-plan-v2&envId=top-interview-150)
+ ```nums``` 배열과 배열에서 삭제해야 하는 요소가 주어집니다.
+ return 값은 배열로 삭제한 요소를 제외한 요소의 수입니다.

#### 2. 나의 풀이
```java
class Solution {
    public int removeElement(int[] nums, int val) {
        int answer = 0;
        for(int i=0; i<nums.length; i++){
            if(nums[i]!=val){ 
                nums[answer] = nums[i];
                answer++;
            }    
        }
        return answer;
    }
}
```
처음에 return 값만 고려해서 for문으로 만들었는데 ```nums[answer] = nums[i];```와 같은 방식으로 ```nums``` 배열도 다시 고려해야 해서 추가해주었습니다.
이렇게 하면 ```val```과 같지 않은 값들만 ```nums``` 배열 앞에 놓을 수 있습니다.
+ 시간복잡도: O(n)
+ 공간복잡도: O(1)

#### 3. 다른 사람의 풀이를 보고 개선하기
```java
class Solution {
    public int removeElement(int[] nums, int val) {
        int count=0; //variable for occurance
        int i=0; 
        int j=nums.length-1;
        while(i<nums.length){
            if(nums[i]==val){
               nums[i]=nums[j];
               nums[j]=-1;
               j--;
               count++;
            }
            else{
                i++;
            }
        }
        return nums.length-count;
    }
}
```
이 문제도 포인터로 접근한 분도 있었는데 시간복잡도와 공간복잡도가 이 경우는 저의 풀이와 같았습니다.
```val``` 값과 같은 경우는 다시 ```nums``` 뒤에 요소 값을 넣어주는 부분이 새로웠습니다.

#### 4. 생각해 볼 부분
포인터를 이용한 풀이가 정렬 문제에서 시간복잡도를 줄일 수 있는 방법이므로 좀 더 익숙해지도록 해야겠습니다.

---
<br>

### 88. Merge Sorted Array
#### 1. 문제
[문제 URL](https://leetcode.com/problems/merge-sorted-array/description/?envType=study-plan-v2&envId=top-interview-150)
+ ```nums1```과 ```nums2```가 주어지고 ```nums1``` 안에 병합한 뒤 정렬하는 문제입니다.
+ 별도의 return 값은 없습니다.

#### 2. 나의 풀이
```java
class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        for(int i=0; i<n; i++){
            nums1[m+i] = nums2[i];
        }
        Arrays.sort(nums1);
    }
}
```
```nums1```의 크기가 원래 문제에서 n+m의 크기로 주어지기 때문에 m개 외의 값은 0으로 되어 있다고 생각을 하고 그 이후의 값만 ```nums2```에서 받아와서
for문으로 넣어주는 방식입니다.
병합 후 마지막 정렬은 ```Arrays.sort``` 메서드로 정렬했습니다.
+ 시간복잡도: O(n + m * log(m+n))
+ 공간복잡도: O(1)

#### 3. 다른 사람의 풀이를 보고 개선하기
```java
class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        int i = m - 1;
        int j = n - 1;
        int k = m + n - 1;
        
        while (j >= 0) {
            if (i >= 0 && nums1[i] > nums2[j]) {
                nums1[k--] = nums1[i--];
            } else {
                nums1[k--] = nums2[j--];
            }
        }
    }
}
```
다른 풀이 중에 가장 깔끔하고 신선했던 접근이었습니다. 두 포인터를 두고 푸는 풀이고 시간복잡도가 ```O(n+m)```으로 개선됩니다.
+ 변수 i, j, k를 각각 ```nums1```, ```nums2```, ```병합한 nums2```의 인덱스로 초기화합니다.
+ 주어진 배열이 이미 정렬되어 있다는 특징을 활용하여, 큰 값들을 뒤에서부터 순차적으로 ```nums1``` 배열에 채워 넣습니다.
+ 병합시 더 큰 값을 ```nums1``` 배열의 끝부터 시작하는 인덱스 k에 저장합니다. 만약 ```nums1``` 배열의 요소가 ```nums2``` 배열의 요소보다 크다면 해당 요소를 먼저 ```nums1[k]``` 위치에 저장하고, 그렇지 않은 경우에는 ```nums2[j]``` 값을 ```nums1[k]``` 위치에 저장합니다.
+ 각각의 배열 요소를 처리할 때마다, i, j, 그리고 k를 감소시킵니다.
+ ```nums2``` 배열의 요소를 모두 ```nums1``` 배열에 합칠 때까지 위의 과정을 반복합니다.

#### 4. 생각해 볼 부분
포인터를 이용한 접근법에 대해 생각해보지 못했는데 시간복잡도를 고려해 새로운 접근법을 익히게 된 계기가 되었습니다.
```Arrays.sort```를 이용한 풀이가 시간복잡도가 높아진다는 것을 확인할 수 있었습니다.   
그리고 return 값이 별도 존재하지 않고 ```nums1```에 값에 병합처리한다는 전제가 다른 코드테스트 사이트와 차이가 있어 문제를 더 잘 읽어야겠다는 생각이 들었습니다.

---

<div style="padding:3px; margin:200px 0;"></div>   