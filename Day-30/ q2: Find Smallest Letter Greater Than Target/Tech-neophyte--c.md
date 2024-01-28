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
