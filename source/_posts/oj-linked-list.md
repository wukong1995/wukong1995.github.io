---
  date: 2014-03-17 21:24:36
  title: oj链表第一题
  tags: [c]
  categories:
    - [算法]
    - [c]
  description:
---

```c
#include <iostream>
using namespace std;
struct link
{
 int num;
 link *next;
};
link *creat(void)
{
 int n;
 cin>>n;
 link *p1,*p2,*head;
 head=NULL;
 p1=new link;
 head=p1;
 for(int i=0;i<n;i++)
 {
  cin>>p1->num;
  p2=p1;
  p1=new link;
  p2->next=p1;
  
 }
 p2->next=NULL;
 return head;
}
link *daozhi(link *head)
{
 link *p,*q,*r;
 p=head;
 q=r=NULL;
 while(p)
 {
     q=p->next;
     p->next=r;
     r=p;
     p=q;
 }
return r;
}

void print(link *head)
{
 link *p;
 p=head;
 while(p)
 {
  cout<<p->num<<" ";
  p=p->next;
 }
 cout<<endl;
}

int main()
{
 link *creat(void);
 link *daozhi(link *head);
 void print(link *head);
 link *head;
 head=creat();
 head=daozhi(head);
 print(head);
 return 0;
}
```


