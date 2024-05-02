二叉搜索树
左子树的值小于根节点
右子树的值大于根节点


可以完成数据的增删改查，对于平衡二叉搜索树，增删改查都为lgn级别

## 数据结点
```cpp
class BinaryNode
{
public:
    T val;
    int cnt=1;
    BinaryNode<T>* lc=nullptr;
    BinaryNode<T>* rc= nullptr;
    BinaryNode(const T val_):val(val_){};
    ~BinaryNode()=default;

};
```

结点具有值域，左孩子结点和右孩子结点

## BST
```cpp
//
// Created by dlut2102 on 2024/4/30.
//
#include<bits/stdc++.h>
using namespace std;
template<class T>
class BinaryNode
{
public:
    T val;
    int cnt=1;
    BinaryNode<T>* lc=nullptr;
    BinaryNode<T>* rc= nullptr;
    BinaryNode(const T val_):val(val_){};
    ~BinaryNode()=default;

};
template<class T>
class BinaryTree
{
public:
    //根节点
    BinaryNode<T>* root=nullptr;
    //插入函数
    void insert_(const T& val) noexcept
    {
        //特判，如果根节点为空，也就是树为空，为根节点赋值
        if(root== nullptr)
        {
            root=new BinaryNode<T>(val);
            return ;
        }
        //这里用now指针代指root指针
        //注意这里用的是值赋值，并不是引用，因为now会在接下来的过程中移动
        //我们并不希望root指针移动
        BinaryNode<T> * now=root;
        while(now)
        {
            //特判 当树中有该值的时候，直接累加返回
            if(val==now->val)
            {
                now->cnt++;
                return;
            }
            //如果val大于当前值，说明在右子树，now往右子树走
            //如果右子树非空，就往右子树走
            //如果右子树为空，那么说明val就应该在空的那个位置
            //至于为什么不继续往下走，因为往右走是nullptr，没有意义
            if(now->rc&&val>now->val)
                now=now->rc;
            else if(!now->rc&&val>now->val)
            {
                now->rc=new BinaryNode<T>(val);
                return;
            }
            //左子树和右子树一样
            if(now->lc&&val<now->val)
                now=now->lc;
            else if(!now->lc&&val<now->val)
            {
                now->lc=new BinaryNode<T>(val);
                return;
            }
        }
    }
    void insert(const T& val) noexcept
    {
        //特判，如果根节点为空，也就是树为空，为根节点赋值
        if(root== nullptr)
        {
            root=new BinaryNode<T>(val);
            return ;
        }
        BinaryNode<T> *now=root;
        BinaryNode<T> *prev= nullptr;
        while(now!= nullptr)
        {
            prev=now;
            if(val==now->val)
            {
                now->cnt++;
                return;
            }
            if(val>now->val)
                now=now->rc;
            else
                now=now->lc;
        }
        if(val<prev->val)
            prev->lc=new BinaryNode<T>(val);
        else
            prev->rc=new BinaryNode<T>(val);
        
    }
    bool find(const T& val)noexcept
    {
        BinaryNode<T> *now=root;
        while(now)
        {
            if(val>now->val)
                now=now->rc;
            else if(val<now->val)
                now=now->lc;
            else
                return true;
        }
        return false;
    }
//    void remove(const T& val)
//    {
//        if(!find(val))
//            return ;
//        BinaryNode<T>* now=root;
//        while(now != nullptr && now->val != val)
//        {
//            if(val>now->val)
//                now=now->rc;
//            else if(val<now->val)
//                now=now->lc;
//        }
//        //如果要删除的结点是叶子结点
//        if(!now->lc&&!now->rc)
//        {
//            delete now;
//            now= nullptr;
//            return ;
//        }
//        //要删除的结点只有一个右儿子
//        else if(!now->lc&&now->rc)
//        {
//            BinaryNode<T>*t=now;
//            now=now->rc;
//            delete t;
//            t=nullptr;
//            return ;
//        }
//        //只有一个左儿子
//        else if(!now->rc&&now->lc)
//        { 
//            BinaryNode<T>*t=now;
//            now=now->lc;
//            delete t;
//            t=nullptr;
//            return ;
//        }
//        //既有左儿子 又有右儿子
//        else
//        {
//            
//        }
//    }
    void remove(const T& val)
    {
        if (!find(val))
            return;
        BinaryNode<T>* parent = nullptr;
        BinaryNode<T>* now = root;
        while (now != nullptr && now->val != val)
        {
            parent = now;
            if (val > now->val)
                now = now->rc;
            else
                now = now->lc;
        }

        // 如果找到了要删除的节点
        if (now != nullptr && now->val == val)
        {
            // 情况1: 要删除的节点是叶子节点
            if (now->lc == nullptr && now->rc == nullptr)
            {
                if (parent == nullptr) // 如果要删除的是根节点
                    root = nullptr;
                else if (parent->lc == now)
                    parent->lc = nullptr;
                else
                    parent->rc = nullptr;
                delete now;
            }
                // 情况2: 要删除的节点只有一个子节点
            else if (now->lc == nullptr)
            {
                if (parent == nullptr)
                    root = now->rc;
                else if (parent->lc == now)
                    parent->lc = now->rc;
                else
                    parent->rc = now->rc;
                delete now;
            }
            else if (now->rc == nullptr)
            {
                if (parent == nullptr)
                    root = now->lc;
                else if (parent->lc == now)
                    parent->lc = now->lc;
                else
                    parent->rc = now->lc;
                delete now;
            }
                // 情况3: 要删除的节点有两个子节点
            else
            {
                BinaryNode<T>* successor = findMin(now->rc); // 找到右子树中最小的节点
                now->val = successor->val; // 将当前节点替换为右子树中最小节点的值
                remove(successor->val); // 递归删除右子树中最小的节点
            }
        }
    }

    BinaryNode<T>* findMin(BinaryNode<T>* node) const
    {
        if (node == nullptr)
            return nullptr;
        while (node->lc != nullptr)
            node = node->lc;
        return node;
    }

    void inOrder(const BinaryNode<T>*now)
    {
        if(now==nullptr)
            return;
        inOrder(now->lc);
        //for(int i=0;i<now->cnt;i++)
            cout<<now->val<<" ";
        inOrder(now->rc);
    }
    void preOrder(const BinaryNode<T>*now)
    {
        if(now==nullptr)
            return;
        cout<<now->val<<" ";
        preOrder(now->lc);
        //for(int i=0;i<now->cnt;i++)
        preOrder(now->rc);
    }
    void postOrder(const BinaryNode<T>*now)
    {
        if(now==nullptr)
            return;
        postOrder(now->lc);
        //for(int i=0;i<now->cnt;i++)
        postOrder(now->rc);
        cout<<now->val<<" ";
    }
    //递归释放空间
    void release(BinaryNode<T>*& now)
    {
        if(now!= nullptr)
        {
            //cout<<"当前节点"<<now->val<<endl;
            release(now->lc);
            release(now->rc);
            //找到子树 delete掉
            if(!now->lc&&!now->rc)
            {
                //cout<<"删除结点："<<now->val<<endl;
                delete now;
                now=nullptr;
            }
        }
    }
    ~BinaryTree()
    {
        //cout<<"释放"<<endl;
        release(root);
    }
};
int main()
{
    BinaryTree<int>tr;
    int n,t;
    cin>>n;
    while(n--)
    {
        cin>>t;
        tr.insert(t);
    }
    
    tr.preOrder(tr.root);
    cout<<endl;
    tr.inOrder(tr.root);
    cout<<endl;
    tr.postOrder(tr.root);
    cout<<endl;
    cout<<tr.find(10)<<endl;
    cout<<tr.find(1)<<endl;
    tr.remove(8);
    cout<<tr.find(6)<<endl;
    cout<<tr.find(8)<<endl;
    cout<<tr.find(5)<<endl;
    tr.remove(5);

    cout<<tr.find(5)<<endl;
    return 0;
}
```


### insert

```cpp
void insert(const T& val) noexcept
{
    //特判，如果根节点为空，也就是树为空，为根节点赋值
    if(root== nullptr)
    {
        root=new BinaryNode<T>(val);
        return ;
    }
    //这里用now指针代指root指针
    //注意这里用的是值赋值，并不是引用，因为now会在接下来的过程中移动
    //我们并不希望root指针移动
    BinaryNode<T> * now=root;
    while(now)
    {
        //特判 当树中有该值的时候，直接累加返回
        if(val==now->val)
        {
            now->cnt++;
            return;
        }
        //如果val大于当前值，说明在右子树，now往右子树走
        //如果右子树非空，就往右子树走
        //如果右子树为空，那么说明val就应该在空的那个位置
        //至于为什么不继续往下走，因为往右走是nullptr，没有意义
        if(now->rc&&val>now->val)
            now=now->rc;
        else if(!now->rc&&val>now->val)
        {
            now->rc=new BinaryNode<T>(val);
            return;
        }
        //左子树和右子树一样
        if(now->lc&&val<now->val)
            now=now->lc;
        else if(!now->lc&&val<now->val)
        {
            now->lc=new BinaryNode<T>(val);
            return;
        }
    }
}
```

整体思想，如果val比当前值要大，就应该插入到右子树上，直到找到一个空位置，插入

```cpp
void insert(const T& val) noexcept
{
    //特判，如果根节点为空，也就是树为空，为根节点赋值
    if(root== nullptr)
    {
        root=new BinaryNode<T>(val);
        return ;
    }
    BinaryNode<T> *now=root;
    //now的父节点
    BinaryNode<T> *prev= nullptr;
    while(now!= nullptr)
    {
        prev=now;
        if(val==now->val)
        {
            now->cnt++;
            return;
        }
        if(val>now->val)
            now=now->rc;
        else
            now=now->lc;
    }
    if(val<prev->val)
        prev->lc=new BinaryNode<T>(val);
    else
        prev->rc=new BinaryNode<T>(val);
    
}
```
prev和now指针来判断，省去特判now空指针情况
now是val插入的地方，为nullptr




### Order

```cpp
void inOrder(const BinaryNode<T>*now)
{
    if(now==nullptr)
        return;
    inOrder(now->lc);
    //for(int i=0;i<now->cnt;i++)
        cout<<now->val<<" ";
    inOrder(now->rc);
}
void preOrder(const BinaryNode<T>*now)
{
    if(now==nullptr)
        return;
    cout<<now->val<<" ";
    preOrder(now->lc);
    //for(int i=0;i<now->cnt;i++)
    preOrder(now->rc);
}
void postOrder(const BinaryNode<T>*now)
{
    if(now==nullptr)
        return;
    postOrder(now->lc);
    //for(int i=0;i<now->cnt;i++)
    postOrder(now->rc);
    cout<<now->val<<" ";
}
```

前序遍历：根左右
中序遍历：左根右
后序遍历：左右根


### find
```cpp
bool find(const T& val)noexcept
{
    BinaryNode<T> *now=root;
    while(now)
    {
        if(val>now->val)
            now=now->rc;
        else if(val<now->val)
            now=now->lc;
        else
            return true;
    }
    return false;
}
```

查找依据：如果比当前值要小，就去左子树；如果比当前值要大，就去右子树

### delete

- 如果要删除的结点出度为0，直接删
- 如果要删除的结点出度为1，直接转移其孩子
- 如果要删除的结点出度为2，则找到右子树最小结点，复制删除，将当前节点替换为右子树最小节点，然后递归删除右子树最小结点

```cpp
 void remove(const T& val)
    {
        if (!find(val))
            return;
        BinaryNode<T>* parent = nullptr;
        BinaryNode<T>* now = root;
        while (now != nullptr && now->val != val)
        {
            parent = now;
            if (val > now->val)
                now = now->rc;
            else
                now = now->lc;
        }

        // 如果找到了要删除的节点
        if (now != nullptr && now->val == val)
        {
            // 情况1: 要删除的节点是叶子节点
            if (now->lc == nullptr && now->rc == nullptr)
            {
                if (parent == nullptr) // 如果要删除的是根节点
                    root = nullptr;
                else if (parent->lc == now)
                    parent->lc = nullptr;
                else
                    parent->rc = nullptr;
                delete now;
            }
                // 情况2: 要删除的节点只有一个子节点
            else if (now->lc == nullptr)
            {
                if (parent == nullptr)
                    root = now->rc;
                else if (parent->lc == now)
                    parent->lc = now->rc;
                else
                    parent->rc = now->rc;
                delete now;
            }
            else if (now->rc == nullptr)
            {
                if (parent == nullptr)
                    root = now->lc;
                else if (parent->lc == now)
                    parent->lc = now->lc;
                else
                    parent->rc = now->lc;
                delete now;
            }
                // 情况3: 要删除的节点有两个子节点
            else
            {
                BinaryNode<T>* successor = findMin(now->rc); // 找到右子树中最小的节点
                now->val = successor->val; // 将当前节点替换为右子树中最小节点的值
                remove(successor->val); // 递归删除右子树中最小的节点
            }
        }
    }

    BinaryNode<T>* findMin(BinaryNode<T>* node) const
    {
        if (node == nullptr)
            return nullptr;
        while (node->lc != nullptr)
            node = node->lc;
        return node;
    }
```




## 个人理解BST

```cpp
//
// Created by dlut2102 on 2024/5/2.
//
//实现二叉搜索树的增删改查

#include<iostream>
#include<vector>
#include <cassert>

using namespace std;

//模版类的声明
template<class T>
class BST;
template<class T>
class BSTNode;

//使用别名
template<class T>
using bstptr= BSTNode<T>*;

template<class T>
class BSTNode
{
public:
    T val;
    bstptr<T> lc= nullptr;
    bstptr<T> rc= nullptr;
    //BSTNode(const T&val_):val(val_){}
    ~BSTNode()
    {
        if(lc== nullptr)
        {
            delete lc;
            lc= nullptr;
        }
        if(rc== nullptr)
        {
            delete rc;
            rc= nullptr;
        }
    }
};

template<class T>
class BST
{
public:
    bstptr<T> root= nullptr;
    //插入
    void insert(const T&val);
    //删除
    void remove(const T&val);
    void removeByCopying(bstptr<T>& now);
    //查找
    bool find(const T&val);
    //遍历
    void inOrder(bstptr<T>now);
    void preOrder(bstptr<T>now);
    void postOrder(bstptr<T>now);
};

template<class T>
void BST<T>::postOrder(bstptr<T> now) 
{
    if(now!= nullptr)
    {
        postOrder(now->lc);
        postOrder(now->rc);
        cout<<now->val<<" ";

    }
}

template<class T>
void BST<T>::preOrder(bstptr<T> now) 
{
    if(now!= nullptr)
    {
        cout<<now->val<<" ";
        preOrder(now->lc);
        preOrder(now->rc);

    }
}

template<class T>
void BST<T>::inOrder(bstptr<T> now)
{
    if(now!= nullptr)
    {
        inOrder(now->lc);
        cout<<now->val<<" ";
        inOrder(now->rc);

    }
}

template<class T>
bool BST<T>::find(const T &val) {
    return false;
}

template<class T>
void BST<T>::insert(const T &val) 
{
    if(root== nullptr)
    {
        root=new BSTNode<T>{val};
        return;
    }
    bstptr<T> now=root;
    bstptr<T> prev= nullptr;
    while(now!= nullptr && now->val!=val)
    {
        prev=now;
        if(val> now->val)
            now=now->rc;
        else
            now=now->lc;
    }
    //这里用val来比较，是因为now为空了，没法判断now是prev是左右孩子
    if( now == nullptr)
        val>prev->val?prev->rc=new BSTNode<T>{val}:prev->lc=new BSTNode<T>{val};
    
}
template<class T>
void BST<T>::remove(const T &val)
{
    bstptr<T> now=root;
    bstptr<T> prev= nullptr;
    
    while( now != nullptr && now->val != val)
    {
        prev=now;
        if(val>now->val)
            now=now->rc;
        else
            now=now->lc;
    }
    //now非空说明找到了
    //此时可以用now判断now是prev的左孩子还是右孩子
    //这里并没有传参now，而是其父指针
    //个人理解 now 是无头有尾指针
    //而 prev->lc/rc 是有头有尾指针
    //按引用传递，修改now 不会对prev有影响，而下面则会有影响
    if(now != nullptr)
        removeByCopying(prev->lc==now?prev->lc:prev->rc);
}
template<class T>
void BST<T>::removeByCopying(bstptr<T>& now) 
{
    //now为要删除的结点其父指针
    bstptr<T> tmp=now;
    bstptr<T>  prev=now;
    //assert(typeid(tmp).name==typeid(prev).name);
    //如果被删除结点没有左子树，直接指针往右子树走
    if(now->lc== nullptr)
        now=now->rc;
    //如果没有右子树，往左子树走
    else if(now->rc== nullptr)
        now=now->lc;
    //既有左子树 又有右子树
    //采用复制删除方法，把要删除结点左子树最大值拷贝过来，然后删除左子树最大值
    else
    {
        tmp=tmp->lc;
        while(tmp->rc != nullptr)
        {
            prev=tmp;
            tmp=tmp->rc;
        }
        //此时一定走到了左子树最大值位置
        now->val=tmp->val;
        //拷贝完成，删除左子树最大值结点
        
        //最大值结点就是now的左子树
        if(prev==now)
            prev->lc=tmp->lc;
        //最大值结点 其父节点 被删除结点 是三个不同节点
        else
            prev->rc=tmp->lc;
    }
    delete tmp;
    tmp= nullptr;
}
#define RMLOG(STR)  tr.remove(STR);\
                    cout<<"删除"<<STR<<"后:"<<endl; \
                    tr.inOrder(tr.root);\
                    cout<<endl;
int main()
{
    
    BST<int>tr;
//    int n,t;
//    cin>>n;
//    while(n--)
//    {
//        cin>>t;
//        tr.insert(t);
//    }
    vector<int>vals{1,6,5 ,9, 8};
    for(auto ele:vals)
        tr.insert(ele);
    
    tr.preOrder(tr.root);
    cout<<endl;
    tr.inOrder(tr.root);
    cout<<endl;
    
    tr.postOrder(tr.root);
    cout<<endl;
//
    //tr.remove(8);

    RMLOG(7)
    RMLOG(5)
    RMLOG(6)
    RMLOG(7)
    return 0;
}
```

几个要注意的点
- 是不是要now为空？如果是，就得需要其父节点
- 类内成员变量是否初始化？
- 值传参与引用传参，需不需要参数进行变动

