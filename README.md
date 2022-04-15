# HelloWorld
My first repository on GitHub
···
#include <iostream>
#include <bits/stdc++.h>

using namespace std;

typedef struct node
{
    int data,deep;
    node *l,*r;
}Tree;

//求深度
int Deep(Tree *root)
{
    if(!root)
        return -1;
    else
        return root->deep;
}

//返回新的深度值
int New_Deep(Tree *p)
{
    return max(Deep(p->l),Deep(p->r))+1;
}

//当深度差大于1发生在左子树的左孩子
//单向右旋
Tree* LL(Tree *root)
{
    Tree *p = root->l;
    root->l = p->r;
    p->r = root;
    p->deep = New_Deep(p);
    root->deep = New_Deep(root);
    return p;
}

//发生在右子树的右孩子
//单向左旋
Tree* RR(Tree *root)
{
    Tree *p = root->r;
    root->r = p->l;
    p->l = root;
    p->deep = New_Deep(p);
    root->deep = New_Deep(root);
    return p;
}

//发生在左子树的右孩子
Tree* LR(Tree *root)
{
    root->l = RR(root->l);
    return LL(root);
}

//发生在右子树的左孩子
Tree* RL(Tree *root)
{
    root->r = LL(root->r);
    return RR(root);
}

//建立平衡二叉树
//每插入一个数据就要判断是否平衡
Tree* creat_Tree(Tree *root, int key)
{
    //根为空直接建立并初始化
    if(!root)
    {
        root = new Tree;
        root->deep = 0;
        root->data = key;
        root->l = root->r = NULL;
    }
    else
    {
        if(key < root->data)
        {
            //左子树插入
            root->l = creat_Tree(root->l, key);
            //判断
            if(Deep(root->l)-Deep(root->r) > 1)
            {
                //平衡
                if(key < root->l->data)
                    //左子树的左孩子
                    root = LL(root);
                else
                    //左子树的右孩子
                    root = LR(root);
            }
        }
        else
        {
            //右子树插入
            root->r = creat_Tree(root->r, key);
            //判断
            if(Deep(root->r)-Deep(root->l) > 1)
            {
                if(key > root->r->data)
                    //右子树右孩子
                    root = RR(root);
                else
                    //右子树左孩子
                    root = RL(root);
            }
        }
    }
    root->deep = New_Deep(root);
    return root;
}

int main()
{
    int n,x;
    cin>>n;
    Tree *root = NULL;
    for(int i=0;i<n;i++)
    {
        cin>>x;
        root = creat_Tree(root, x);
    }
    cout<<root->data<<endl;
    return 0;
}
···
