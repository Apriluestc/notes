* [二叉搜索树](#二叉搜索树)
	* [基本概念](#基本概念)
	* [二叉树的搜索操作](#二叉树的搜索操作)
		* [查找](#查找)
		* [插入](#插入)
		* [删除](#删除)
	* [二叉搜索树的实现](#二叉搜索树的实现)
	* [二叉搜索树的性能分析](#二叉搜索树的性能分析)
* [平衡二叉树](#平衡二叉树)
	* [基本概念](#基本概念)
	* [基本操作](#基本操作)
		* [插入](#插入)
		* [调节平衡因子](#调节平衡因子)
		* [旋转](#旋转)
		* [删除](#删除)
	* [AVL 数的验证](#avl-数的验证)
		* [验证是否为二叉搜索树](#验证是否为二叉搜索树)
		* [验证是否为平衡树](#验证是否为平衡树)
	* [AVL 树的实现](#avl-树的实现)
	* [AVL 树的性能分析](#avl-树的性能分析)
* [红黑树](#红黑树)
	* [基本概念](#基本概念)
	* [基本操作](#基本操作)
		* [插入](#插入)
		* [删除](#删除)
		* [迭代器](#迭代器)
	* [红黑树的实现](#红黑树的实现)
	* [红黑树的验证](#红黑树的验证)

# 二叉搜索树

## 基本概念

二叉搜索树俗称二叉排序树，具有如下**性质**

- 若它的左子树不为空，则左子树上所有的结点的值都小于其根节点的值
- 若它的右子树不为空，则右子树上所有的节点的值都大于其根节点的值
- 它的左右子树也是二叉搜索树

## 二叉树的搜索操作

### 查找

```cpp
若根节点不空 && 如果根节点的值 == key 返回 true
    		  如果根节点的值 > key 返回 左子树
    		  如果根节点的值 < key 返回 右子树
	   否则  返回 false
```



### 插入

- 树为空，则直接插入
- 树不空，按照二叉搜索树性质查找插入位置，插入新节点



### 删除

前提要删除的值在树中，否则返回

- 要删除的结点无孩子结点
- 要删除的结点只有左孩子结点
- 要删除的结点只有右孩子结点
- 要删除的结点有左右孩子结点

## 二叉搜索树的实现

```cpp
template<class T>
struct BSTNode
{
    BSTNode(const T& val = T())
        :_left(nullptr)
        ,_right(nullptr)
        ,_val(val)
    {}
    BSTNode<T>* _left;
    BSTNode<T>* _right;
    T _val;
};
template<class T>
class BSTree
{
public:
    BSTree()
        :_root(nullptr)
    {}
    ~BSTree()
    {
        _Distroy(_root);
    }
    BSTNode<T>* Find(const T& val)
    {
        BSTNode<T>* pCur = _root;
        while(pCur)
        {
            if(val == pCur->_val)
                return pCur;
            else if(val > pCur->_val)
                pCur = pCur->_right;
            else
                pCur = pCur->_left;
        }
        return nullptr;
    }
    bool Insert(const T& val)
    {
        if(_root == nullptr)
        {
            _root = new BSTNode<T>*(val);
            return true;
        }
        BSTNode<T>* tmp = _root;
        BSTNode<T>* parent = nullptr;
        while(tmp)
        {
            parent = tmp;
            if(val < tmp->_val)
                tmp = tmp->_left;
            else if(val > tmp->_val)
                tmp = tmp->_right;
            else
                return false;
        }
        tmp = new BSTNode<T>*(val);
        if(val < parent->_val)
            parent->_left = tmp;
        else
            parent->_right = tmp;
        return true;
    }
    bool Erase(const T& val)
    {
        if(_root == nullptr)
            return false;
        //查找 val 在树中的位置
        BSTNode<T>* pCur = _root;
        BSTNode<T>* parent = nullptr;
        while(pCur)
        {
            if(val == pCur->_val)
                break;
            else if(val > pCur->_val)
            {
                parent = pCur;
                pCur = pCur->_right;
            }
            else
            {
                parent = pCur;
                pCur = pCur->_left
            }
        }
        if(nullptr == pCur)
            return false;
        //分以下情况进行删除
        if(pCur->_right == nullptr)
        {
            //直接删除
        }
        else if(pCur->_left == nullptr)
        {
            //当前节点只有右孩子，可直接删除
        }
        else
        {
            //左右孩子都存在
        }
        return true;
    }
    void InOrder()
    {
        _InOrder(_root);
    }
protected:
    void _InOrder(BSTNode<T>* root)
    {
        if(root)
        {
            _InOrder(root->_left);
            cout << root->_val;
            _InOrder(root->_right);
        }
    }
    void _Distroy(BSTNode<T>& root)
    {
        if(root)
        {
            _Distroy(root->_left);
            _Distroy(root->_right);
            root = nullptr;
        }
    }
private:
    BSTNode<T>* _root;
};
```

## 二叉搜索树的性能分析

​	插入和删除之前，先得进行查找，查找性能代表了二叉搜索树的性能

​	对有 n 个节点的二叉搜索树，若每个元素的查找效率相等，则二叉搜索树的平均查找长度时节点在二叉搜索树的深度的函数，即结点越深，比较次数越多

**时间复杂度**

**最好情况**
$$
log2N
$$
**最坏情况**
$$
N/2
$$
**平均查找长度**
$$
ASL = [(n+1)/n]*log2(n+1)-1
$$


# 平衡二叉树

## 基本概念

## 基本操作

### 插入

### 调节平衡因子

### 旋转

### 删除

## AVL 数的验证

### 验证是否为二叉搜索树

### 验证是否为平衡树

## AVL 树的实现

## AVL 树的性能分析

# 红黑树

## 基本概念

## 基本操作

### 插入

### 删除

### 迭代器

## 红黑树的实现

## 红黑树的验证