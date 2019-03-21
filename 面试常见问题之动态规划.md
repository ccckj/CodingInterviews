# 爬楼梯问题总结 #
## 1. 入门级爬楼梯##
问题：一只青蛙一次可以跳上1级台阶，也可以跳上2级台阶。求该青蛙跳上一个n级台阶总共有多少种跳发。
> 首先考虑最简单的情况。如果只有1级台阶，显然只有一种跳法；如果有两级，那就有两种跳法：一种是分两次跳，每次跳一级；另一种就是一次跳两级。
> 接下来讨论一般情况。把n级台阶的跳法看成是n的函数，记为$f(n)$。当n>2时，第一次跳有两种跳法：一是第一次跳1级，此时剩下n-1级台阶有$f(n-1)$种跳法；二是第一次跳2级，此时n-2级台阶剩下$f(n-2)$种跳法。因此n级台阶总的不同跳法数为：
$$f(n)=f(n-1)+f(n-2)$$

即为斐波那契数列。

	class Solution {
	public:
    	int jumpFloor(int number) {
        	int f1=2;
        	int f2 =3;
        	if(number<0)
            	return -1;
        	if(number <=3)
            	return number;
            
        	for(int j=4; j<=number; ++j)
        	{
            	f2 = f1 + f2;
            	f1 = f2 - f1; 
        	}
        	return f2;
    	}
	};

##  略升级版爬楼梯##
问题：同样的因此可以爬一级或者两级，但是每级有对应的代价cost[i], 求爬上楼梯最小代价是多少？
> 令$f(n)$为爬上第n级楼梯所付的代价。当n=1时，需要付上第一级楼梯的代价(1作为起点)；当n=2时需要的代价为:$$f(2)=min(cost[1], cost[2])$$
> 当n>2时有：
> $$f(n)=min(f(n-1), f(n-2)) + cost[n]$$

	class Solution {
	public:
    	int minCostClimbingStairs(vector<int>& cost) {
        	int length = cost.size();
        	if(length<=0)
            	return 0;
        	if(length==1)
            	return cost[0];
        	if(length==2)
            	return min(cost[1], cost[1]);
        	int f1 = cost[0], f2 = cost[1];
        	for(int i = 2; i<length; i++)
        	{
            	int f0 = cost[i] + min(f1, f2);
            	f1 = f2;
            	f2 = f0;
        	}
        	return min(f1,f2);
    	}
	};

## 终极版爬楼梯 ##
问题描述：n级楼梯，每级对应不同代价cost[i]，一次可以爬一级也可以跳一至两级，每跳完一次必须在爬一次才能继续跳。求爬楼梯总代价最小为多少（跳上去不需要代价）。
> 可以维护两个数组，一个climb数组表示从下级爬上来所付出的代价，另一个jump数组表示从下级跳上来所花费的代价。有
> $$climb[i]=min(climb[i-1], jump[i-1])+cost[i]$$
> 由于每次只能爬一级，所以只能从i-1级爬到第i级，而第i-1级既有可能是跳上去的，也有可能是爬过去的。
> $$jump[i]=min(climb[i-1], climb[i-2])$$
> 表示既可以从i-1级跳上去，也可以从i-2级跳上去。由于需要调到第i级，所以i-1或i-2只能是爬上去的。
> 最后，取两个数组中的最小值则表示到达第n级花费的最小代价。

	public:
		int MinCostClimbingStairs(vector<int> cost)
		{
			int length = cost.size();
			if (length <=2)
				return 0;
			vector<int> jump(length + 1, 0); //维护一个从下面跳上来的数组
			vector<int> climb(length + 1, 0);//维护一个从下面爬上来的数组
			for (int i = 1; i <= length; ++i) //表示上到第i阶。第0阶存储初始值。
			{
				if (i == 1) 
				{
					jump[i] = 0; //第一阶，跳过去
				}
				else {
					jump[i] = min(climb[i - 1], climb[i - 2]);
				}
				climb[i] = min(climb[i - 1], jump[i - 1]) + cost[i-1];
			}
			return min(climb[length], jump[length]);
		}
	};




