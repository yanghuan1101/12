#include<iostream>
#include<stdio.h>
#include<stdlib.h>
#define ElemType int
#define Status int
#define ERROR 0
#define OK 1
using namespace std;
typedef struct LNode 
{
	ElemType note;
	struct LNode* next;
}LNode, * LinkedList;

void MergeList_L(LinkedList& La, LinkedList& Lb, LinkedList& Lc)
{
	LNode* pa = La->next;
	LNode* pb = Lb->next;
	LNode* pc;
	Lc = pc = La;
	while (pa && pb)
	{
		if (pa->note <= pb->note)
		{
			pc->next = pa;
			pc = pa;
			pa = pa->next;
		}
		else
		{
			pc->next = pb;
			pc = pb;
			pb = pb->next;
		}
	}
	while (pa)
	{
		pc->next = pa;
		pc = pa;
		pa = pa->next;
	}
	while (pb)
	{
		pc->next = pb;
		pc = pb;
		pb = pb->next;
	}
	free(pb);
	free(pa);
}

//正序创建线性表，算法2.11
void CreateList_L(LinkedList& L, int n)
{
	L = (LinkedList)malloc(sizeof(LNode));
	L->next = NULL;
	for (int i = 0; i < n; ++i)
	{
		LNode* q = (LinkedList)malloc(sizeof(LNode));
		cin >> q->note;
		q->next = L->next;
		L->next = q;
	}
}

void swap(ElemType& a, ElemType& b)
{
	ElemType c = a;
	a = b;
	b = c;
}

//排序
void ListSort(LinkedList& L)
{
	if (L == NULL || L->next == NULL)
		return;
	LNode* pstart = new LNode();
	pstart->next = L;
	LNode* sortedTail = pstart;

	while (sortedTail->next != NULL)
	{
		LNode* minNode = sortedTail->next;
		LNode* p = sortedTail->next->next;
		while (p != NULL)
		{
			if (p->note < minNode->note)
				minNode = p;
			p = p->next;
		}
		swap(minNode->note, sortedTail->next->note);
		sortedTail = sortedTail->next;
	}
	L = pstart->next;
	delete pstart;
}
//输出线性链表
void PrintList_L(LinkedList& L)
{
	LNode* p = L->next;
	while (p)
	{
		cout << p->note;
		if (p->next != NULL)
			cout << ",";
		p = p->next;
	}
	cout << endl;

}
int main()
{
	int na, nb;
	LinkedList La, Lb, Lc;
	cout << "";//第一个线性表的数字个数
	cin >> na;
	cout << "" << endl;//第一个线性表的内容
	CreateList_L(La, na);
	ListSort(La);
	cout << "";//第二个线性表的数字个数
	cin >> nb;
	cout << "" << endl;//第二个线性表的内容
	CreateList_L(Lb, nb);
	ListSort(Lb);
	MergeList_L(La, Lb, Lc);
	cout << endl;
	cout << "The current List is:" << endl;
	PrintList_L(Lc);
	system("pause");
}


