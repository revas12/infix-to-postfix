# infix-to-postfix
//converting infix exp to postfix with tree data structure
#include<iostream>
#include<string.h>
#include<stdio.h>
#define max 20
using namespace std;
class node
{
    char data;
    node *left;
    node *right;
    node(char d)
    {
        data=d;
        left=NULL;
        right=NULL;
    }
    friend class tree;
};

class tree
{
    node *root;
public:
    node *create(char post[]);
    void display(node *);
};
template<class T>
class Stack
{
public:
     T stk[max];
    int top;
    void infixtopostfix(char,char);

    Stack()
    {
        top=-1;
    }
    int empty()
    {
        if(top==-1)
            return 1;
        else
            return 0;
    }
    int full()
    {
        if(top==(max-1))
            return 1;
        else
            return 0;

    }
    int gettop()
    {
        if(!empty())
        return stk[top];
    }

     void push(T ptr2)  //push with node as argument
            {
                if(!full())
                {
                stk[++top]=ptr2;
                }
                else
                    cout<<"STACK IS FULL";
            }
     T pop()
             {
                 if(!empty())
                 {
                      return stk[top--];
                 }
                 else
                     cout<<"\nSTACK IS EMPTY";
             }
     int precedence(char ch)
     {
         if(ch=='+'||ch=='-')
         return 1;
         else if(ch=='*'||ch=='/')
         return 2;
     }
     void infixtopostfix(char in[],char arr[])
     {
         char temp;
         int i=0,j=0;
         char ch;
         cout<<"\n**INFIX TO POSTFIX CONVERSION**\n";
         cout<<"\nEnter Infix Expression\n";
         cin>>in;
         cout<<"\nThe Postfix Expression is: ";
         while(in[i]!='\0')
         {
        	 ch=in[i];
        	 if(ch=='(')
        	 push(ch);
        	 else if(ch==')')
        	 {
        		 temp=pop();
        		 while(temp!='(')
        		 {
        	        arr[j++]=temp;
        		    temp=pop();
        		 }
        	 }
        	         else if(isalpha(ch))
        	         arr[j++]=ch;

        	         else
        	         {
        	             if(empty())
        	             push(ch);
        	             else
        	             {
        	                 if(gettop()=='(')
        	                 {
        	                     push(ch);
        	                 }
        	                 else if(precedence(ch)>precedence(gettop()))
        	                 {
        	                     push(ch);
        	                 }
        	             else
        	             {
        	                 while(precedence(ch)<=precedence(gettop()))
        	                 {
        	                     //cout<<
        	                     arr[j++]=pop();
        	                 }
        	                 push(ch);
        	             }
        	             }
        	         }
        	     i++;
        	 }

        	         if(i==strlen(in))
        	         {
        	             while(!empty())
        	             {
        	                 ch=pop();
        	                 //cout<<ch;
        	                 arr[j++]=ch;
        	             }
        	             cout<<"\n";
        	         }

        	     cout<<arr;
        	 }
};

node *tree::create(char post[20])
{
    Stack<node*> s;
    node *current, *leftsubtree, *rightsubtree;
    for(int i=0;i<strlen(post);i++)
    {
        if(isalnum(post[i]))
        {
            current=new node(post[i]);
            s.push(current);
        }
        else
        {
            rightsubtree=s.pop();
            leftsubtree=s.pop();
            current=new node(post[i]);
            current->left=leftsubtree;
            current->right=rightsubtree;
            s.push(current);
        }
    }
    current=s.pop();
    return current;
}

void tree::display(node *root)
{
    Stack <node*> S;
        node* curr=root;
        if(curr==NULL)
        {
            cout<<"Tree is empty";
            return;
        }
        cout<<"\nThe Tree Is: \n";
        while(1)
        {
            while(curr)
            {
                S.push(curr);
                curr=curr->left;
            }
            if(S.empty())
                return ;
            curr=S.pop();
            cout<<curr->data;
            curr=curr->right;
        }
}

int main()
{
    char in[50],post[50];
    tree t;
    node *ptr;
    Stack <char> S;
    S.infixtopostfix(in,post);
   // cout<<"\n"<<post;
    ptr=t.create(post);
    t.display(ptr);
return 0;
}
