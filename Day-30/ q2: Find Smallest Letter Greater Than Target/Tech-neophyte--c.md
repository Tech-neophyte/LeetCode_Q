## Cpp code: 
## Approach 1:
``T.C=O(n)``
```
class Solution {
public:
    char nextGreatestLetter(vector<char>& letters, char target) {
        char ans=letters[0];
        int tar= target-'a';
        for(char c:letters){
            if ((c-'a')>tar){
                return c;
            }
        }
        return ans;
    }
};
```
## Approach 2: Using Binary Search to reduce time complexity
class Solution {
public:
    char nextGreatestLetter(vector<char>& letters, char target) {


        // Method 1 - using STL
        // int pos = upper_bound(letters.begin(),letters.end(),target) - letters.begin();
        

        // // if pos == n, means greater element than target not found 
        // if(pos == letters.size()){
        //     return letters[0];
        // }


        // return letters[pos];



        // Method 2 - by implementing upper bound
        int s = 0, e = letters.size() - 1, mid;

        while(s<=e){
            mid = s + (e-s)/2;

            if(letters[mid] <= target){
                s = mid + 1;
            }
            else{
                e = mid - 1;
            }
        } 

        // if pos == n, means greater element than target not found 
        if(s == letters.size()){
            return letters[0];
        }

        return letters[s];
    }
};
