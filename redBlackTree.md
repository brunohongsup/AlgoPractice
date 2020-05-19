# AlgoPractice
self taught algo

#include <stdio.h>

#include <iostream>
#include "RedBlackTree.h"
using namespace std;
static Node* AllocNode();
static void Print(Node* n);
Node* NIL = new Node;
Node* tree_search(Node* t, int x)
{
	if (t == NIL || x == t->key)
		return t;

	if (x > t->key)
		return tree_search(t->right, x);

	else
		return tree_search(t->left, x);

}
static Node* AllocNode()
{
	return new Node;
}
void Terminate(Node* x)
{
	if (x->left != NIL) Terminate(x->left);
	if (x->right != NIL) Terminate(x->right);
	delete x;
}
Node* rbtreeInsert(Node* t,int x,Node* parent)
{
	if (t == NIL) {
		t = AllocNode();
		t->key = x;
		t->left = t->right = NIL;
		t->parent = parent;
		t->color = RED;
		if (t->parent->color != BLACK) {
			Insert_pNotBlack(t);
		}
		else;
		return t;
	}

	if (x <t->key)
	{
		t->left = rbtreeInsert(t->left, x,t);
		return t;
	}
	else if (x > t->key) {
		t->right = rbtreeInsert(t->right, x,t);
		return t;

	}
	else {
		return t;
	}
}
void Insert_pNotBlack(Node* n)
{
	Node* uncle = getUncle(n);
	Node* g = getGrandparent(n);
	if (uncle->color == RED) 
		Insert_case1(n);
	
	else {
		if (n->parent==g->left) {
			if (n == n->parent->left)
				Insert_case2_2L(n);
			else
				Insert_case2_1L(n);
			
			
		}
		else {
			if (n == n->parent->right)
				Insert_case2_2R(n);
			else
				Insert_case2_1R(n);
				

		}

	}

	return;

}
Node* getGrandparent(Node* n)
{
	return n->parent->parent;
}
Node* getRoot(Node* n)
{
	Node* tmp = n;
	while (tmp->parent != NIL)
		tmp = tmp->parent;

	return tmp;
}
Node* getUncle(Node* n)
{
	Node* grand = getGrandparent(n);
	if (n->parent == grand->right)
		return grand->left;
	else
		return grand->right;
}
void Insert_case1(Node* n)
{
	Node* uncle = getUncle(n);
	Node* grand = getGrandparent(n);
	Node* root = getRoot(n);

	n->parent->color = uncle->color = BLACK;

	if (grand != root) {
		grand->color = RED;
		if (grand->parent->color != BLACK)
			Insert_pNotBlack(grand);
		else
			return;
	}
	else
		return;

}
void Insert_case2_1R(Node* n)
{
	rotate_leftR(n);
	Insert_case2_2R(n->right);
	return;
}

void Insert_case2_1L(Node* n)
{
	rotate_leftL(n);
	Insert_case2_2L(n->left);
	return;
}
void Insert_case2_2L(Node* n)
{
	rotate_rightL(n);
	return;
}
void Insert_case2_2R(Node* n)
{
	rotate_rightR(n);
	return;
}
void rotate_rightR(Node* n)
{
	Node* p = n->parent;
	Node* g = getGrandparent(n);
	p->parent = g->parent;
	if (g == g->parent->left)
		g->parent->left = p;
	else
		g->parent->right = p;
	
	g->parent = p;
	g->right = p->left;
	p->left = g;

	p->color = BLACK;
	g->color = RED;

	return;
}
void rotate_rightL(Node* n)
{
	Node* g = getGrandparent(n);
	Node* p = n->parent;
	p->parent = g->parent;

	if (g == g->parent->left)
		g->parent->left = p;

	else
		g->parent->right = p;

	g->parent = p;
	g->left = p->right;
	p->right = g;
	
	p->color = BLACK;
	g->color = RED;
			
	return;
}
void rotate_leftR(Node* n)
{
	Node* p = n->parent;
	Node* g = getGrandparent(n);

	n->parent = g;
	g->right = n;

	Node* tmp = n->right;
	n->right = p;
	p->parent = n;
	p->left = tmp;

	return;


}
void rotate_leftL(Node* n)
{
	Node* p = n->parent;
	Node* g = getGrandparent(n);
	g->left = n;
	n->parent = g;
	Node* tmp = n->left;
	n->left = p;
	p->parent = n;
	p->right = tmp;

	return;
}
