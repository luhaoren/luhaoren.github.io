# 原题


北极的某区域共有$n$座村庄($1 ≤ n ≤ 500$ )，每座村庄的坐标用一对整数$(x, y$)表示，其中 $0 ≤ x, y ≤ 10000$。为了加强联系，决定在村庄之间建立通讯网络。

通讯工具可以是无线电收发机，也可以是卫星设备。所有的村庄都可以拥有一部无线电收发机， 且所有的无线电收发机型号相同。但卫星设备数量有限，只能给一部分村庄配备卫星设备。 

不同型号的无线电收发机有一个不同的参数$d$，两座村庄之间的距离如果不超过d就可以用该型号的无线电收发机直接通讯，$d$值越大的型号价格越贵。拥有卫星设备的两座村庄无论相距多远都可以直接通讯。

现在有$k$台卫星设备，请你编写程序计算出如何分配可以使所拥有的无线电收发机的$d$值最小，并保证任意两个村庄都可以直接或间接通讯

## 输入


第一行输入包含$N$，即测试数据的数量。

每个测试用例的第一行包含$1 <= S <= 100$(卫星数)，$S <P <= 500$(前哨数)。

接下来P行，给出每个前哨（$km$，坐标是0到10,000之间的整数）的$（x，y）$坐标。

## 输出

 每个样例输出一个最小d值，两位小数

## 样例输入
```
1
2 4
0 100
0 300
0 600
150 750
```
## 样例输出
```
212.13
```
## 提示

对于所有测试点，$1<=N<=10$。

# 题目大意
最小生成树，$p$个节点，$s$条路径，只能连$p-s$条线，输出最大的权值。
# 算法
最小生成树 $kruskall$算法
# 代码
```cpp
#include<bits/stdc++.h>
using namespace std;
int a[1010];//并查集函数
int find(int x){
	//并查集查询
	if(a[x]==x) return x;
	return a[x]=find(a[x]);//路径压缩
}
struct xy{
	double x,y;//xy坐标
};
struct node{
	int a,b;
	double w;
};
bool cmp(node a,node b){
	return a.w<b.w;//自定义排序，也可以重载运算符
}
inline double dis(double x1,double y1,double x2,double y2){
	return sqrt((x1-x2)*(x1-x2)+(y1-y2)*(y1-y2));//曼哈顿距离
}
int n;
void kruskall(){
	//因为有多组数据，所以单独一个函数
	xy point[1010];//点数组
	vector<node>v;
	int s,p,num=0;
	double d;
	cin>>s>>p;
	for(int i=1;i<=p;i++) a[i]=i;//并查集初始化
	for(int i=1;i<=p;i++){
		cin>>point[i].x>>point[i].y;
	}
	for(int i=1;i<=p;i++){
		for(int j=1;j<=p;j++){
			if(i!=j) v.push_back({i,j,dis(point[i].x,point[i].y,point[j].x,point[j].y)});
		}
	}
	sort(v.begin(),v.end(),cmp);//排序
	for(auto it:v){//kruskall算法
		int fa=find(it.a),fb=find(it.b);
		if(fa!=fb){
			a[fa]=fb;
			num++;
			if(num==p-s){
				d=it.w;
				break;
			}
		}
	}
	printf("%.2lf\n",d);//最后一个就是最大的，两位小数输出
}
int main(){
	cin>>n;
	while(n--) kruskall();
}
```
感谢观看$qwq$！
