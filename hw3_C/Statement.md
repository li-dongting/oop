题目描述
本题需要你在给定代码(见problem_80.zip)的基础上，实现类Vector。

Vector是一种动态数组，可以数据储存的过程中动态调整其容量大小。具体来说，给定Vector的接口如下：
```
class Vector {
private:
    int capacity; //容量，即该对象最多可以储存多少个元素
    int len;      //当前元素长度
    Node* array;  //指向元素数组头的指针
public:
    Vector(int n); //初始化Vector，容量设定为n。
    ~Vector();
    Vector(const Vector & other);
    Vector(Vector && other);
    Vector & operator=(const Vector & other);
    Vector & operator=(Vector && other);
    Node& operator [] (int pos);
    void append(int value);   //在数组的尾部添加一个指定值的元素，即Node(value)。保证插入后长度不会大于容量。
    void insert(int pos, int value);  //在数组的指定位置插入新元素(即Node(value))，其他元素顺次右移。保证插入后长度不会大于容量。
    void swap(int pos1, int pos2);    //交换数组中指定位置的元素
    void dilatation();                //将目前Vector中的数组容量翻倍。提示：你需要先申请更大的数组，然后将当前元素信息移动过去。
    int getlen();                     //获取当前Vector的长度。
};
```
Vector类的元素是Node类的对象，具体可见下载文件中的 Node.h 和 Node.cpp。

我们将使用测试函数来判断你的实现是否正确，样例测试代码见main.cpp中test1函数和test2函数，从标准输入读入，处理并产生相应输出，最后由输出结果判定你的结果是否正确。

在测试函数结束时，我们会调用Node::outputResult()函数来输出所有Node类对象的普通构造1（Node()）、普通构造函数2（Node(int v)）、拷贝构造、移动构造、拷贝赋值、移动赋值、析构的次数。通过这些次数来验证你的实现的正确性和效率。实现正确的Vector类不应该存在内存泄露，即满足：

普通构造1+普通构造函数2+拷贝构造+移动构造=析构

我们假定普通构造函数1由于没有进行显式赋值操作（默认值），该函数耗时较短。普通构造2、拷贝构造和拷贝赋值操作消耗时间较长，移动构造和移动赋值消耗时间较短，我们将使用如下公式简单地验证效率：

(普通构造2+拷贝构造+拷贝赋值)∗10+(普通构造1+移动构造+移动赋值)≤参考答案

我们将按照如下方式对你的程序评分：

1.程序输出结果正确，但存在内存泄漏，获得50%的分数；

2.程序输出结果正确，不存在内存泄漏，但效率不满足要求，获得70%的分数；

3.程序输出结果正确，不存在内存泄漏，且满足效率要求，获得100%的分数。

提示：只要合理利用移动构造和移动赋值，即可通过效率测试。

### 输入格式
第一行包含一个正整数ref_ans，表示效率的参考值。

第二行包含一个正整数n，表示Vector的初始容量。

第三行包含一个正整数m。

接下来m行，每行一个整数，按顺序加入Vector数组中。

第m+4行包含一个正整数k，表示操作的个数。

接下来的k行，每行1到3个整数：p (q) (v)，表示操作的参数。

当p=0时，该操作为在尾部添加元素，将q加入到数组尾部。

当p=1时，该操作为插入，将v插入到数组的第q个位置。

当p=2时，该操作为交换，将数组中第q个元素和第v个元素交换位置。

当p=3时，该操作为长度加倍。

输入保证所有的操作都合法。

输入样例
```
272
6
5
0
1
2
3
4
4
0 5
3
1 4 8
2 3 0
```
样例解释：
```
272      // 效率参考值
6        // 初始容量
5        // 接下来是5个初始值，初始值为 0 1 2 3 4
0
1
2
3
4
4        // 接下来是4个操作
0 5      // 将5加入数组尾部，此时数组为 0 1 2 3 4 5
3        // 容量扩大为原来的两倍
1 4 8    // 将8插入数组的第4个位置，此时数组为 0 1 2 3 8 5
2 3 0    // 将数组的第3个元素和第0个元素交换位置，此时数组为 3 1 2 0 8 5
```
### 输出样例
```
0 1 2 3 4 5
0 1 2 3 4 5
0 1 2 3 8 4 5
3 1 2 0 8 4 5
pass
3 1 2 0 8 4 5
pass
3 1 2 0 8 4 5
44 7 0 1 14 17 52
YES
```
### 编译指令
编译指令为：g++ main.cpp Vector.cpp Node.cpp -o main -std=c++11 -fno-elide-constructors

### 提交要求
在已有代码的基础上编写Vector.cpp，不能使用STL标准库中的vector容器。