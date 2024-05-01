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

