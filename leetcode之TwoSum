##  leetcode 之two sum
https://leetcode.com/problems/two-sum/
### java
``` java
public class Solution {
    public int[] twoSum(int[] nums, int target) {
		Map<Integer,Integer> map = new HashMap<Integer,Integer>();
		int [] res = new int[2];
		for(int i = 0; i < nums.length; i++){
			int other = target - nums[i];
			if(map.containsKey(other)){
				res[0] = map.get(other);
				res[1] = i ;
				return res;
			}
			map.put(nums[i], i);
		}
		return res;
    }
}
```
### C++
``` Cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int,int> recordMap;
        vector<int> result;
        for(int i = 0; i < nums.size(); i++){
        	int other = target - nums[i];
        	if(recordMap.find(other)!= recordMap.end()){
        		result.push_back(recordMap[other]);
        		result.push_back(i);
        		return result;
        	}
        	recordMap[nums[i]] = i;
        }
        return result;
    }
};
```
