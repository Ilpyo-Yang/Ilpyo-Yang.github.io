---
title: Coding Test
author: Rosie Yang
date: 2023-08-25
category: job
layout: post
---

## LeetCode
### Top Interview 150
+ [88. Merge Sorted Array](/job/2023/08/25/Coding_test.html#88-merge-sorted-array)
+ [27. Remove Element](/job/2023/08/25/Coding_test.html#27-remove-element)
+ [26. Remove Duplicates from Sorted Array]()

<br>

#### 88. Merge Sorted Array
##### 1. 문제
[문제 URL](https://leetcode.com/problems/merge-sorted-array/description/?envType=study-plan-v2&envId=top-interview-150)
+ ```nums1```과 ```nums2```가 주어지고 ```nums1``` 안에 병합한 뒤 정렬하는 문제입니다.
+ 별도의 return 값은 없습니다.

##### 2. 나의 풀이
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

##### 3. 다른 사람의 풀이를 보고 개선하기
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

##### 4. 회고
포인터를 이용한 접근법에 대해 생각해보지 못했는데 시간복잡도를 고려해 새로운 접근법을 익히게 된 계기가 되었습니다. 
```Arrays.sort```를 이용한 풀이가 시간복잡도가 높아진다는 것을 확인할 수 있었습니다.   
그리고 return 값이 별도 존재하지 않고 ```nums1```에 값에 병합처리한다는 전제가 다른 코드테스트 사이트와 차이가 있어 문제를 더 잘 읽어야겠다는 생각이 들었습니다. 

---

#### 27. Remove Element
##### 1. 문제
[문제 URL](https://leetcode.com/problems/remove-element/?envType=study-plan-v2&envId=top-interview-150)
+ ```nums``` 배열과 배열에서 삭제해야 하는 요소가 주어집니다.
+ return 값은 배열로 삭제한 요소를 제외한 요소의 수입니다.

##### 2. 나의 풀이
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

##### 3. 다른 사람의 풀이를 보고 개선하기
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

##### 4. 회고
포인터를 이용한 풀이가 정렬 문제에서 시간복잡도를 줄일 수 있는 방법이므로 좀 더 익숙해지도록 해야겠습니다.

---

#### 26. Remove Duplicates from Sorted Array
##### 1. 문제
[문제 URL](https://leetcode.com/problems/remove-duplicates-from-sorted-array/?envType=study-plan-v2&envId=top-interview-150)
+ 정렬된 배열 ```nums```의 중복값을 제외한 요소의 수를 return하는 문제입니다.
+ 배열의 중복값은 없어야 합니다.

##### 2. 나의 풀이
```java
class Solution {
    public int removeElement(int[] nums, int val) {
        Arrays.stream(nums).distinct().toArray();
        return nums.length;
    }
}
```
IDE에서와 달리 ```nums``` 값의 output이 중복값이 제거되지 않은 상태로 나와서 다시 고민하게 되었습니다.
+ 시간복잡도: O(n)
+ 공간복잡도: O(n)

##### 3. 다른 사람의 풀이를 보고 개선하기
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

##### 4. 회고
stream에서 사용되는 ```distinct()```와 ```toArray()```는 객체를 새로 생성, 메모리를 추가로 필요로 한다는 점에서 공간복잡도 결과가 좋지 않게 나왔습니다.  
새로운 객체 생성보다는 주어진 ```nums``` 객체 값에 변화를 주는 방식으로 해결하는 것을 생각해봐야 합니다.

<div style="padding:3px; margin:200px 0;"></div>   