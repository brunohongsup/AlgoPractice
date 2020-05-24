# AlgoPractice
self taught algo
Node* rbtreeInsert(Node** root,Node* n,int x,Node* parent)
{
	if (n == NIL) {
		if (*root == NIL) {
			*root = AllocNode();	
			n = *root;
			n->color = BLACK;
		}
		else {
			n = AllocNode();
			n->color = RED;
		}
		n->key = x;
		n->left = n->right = NIL;
		n->parent = parent;
	}
	else {
		if (n->key > x)
			n->left = rbtreeInsert(root, n->left, x, n);
		else if (n->key < x)
			n->right = rbtreeInsert(root, n->right, x, n);
		else;
	}

	return n;

}
void Insert_pNotBlack(Node** root,Node* n)
{
	if (n->parent->color == BLACK) 
		return;
	else {
		Node* u = getUncle(n);
		Node* gp = getGrandparent(n);
		Node* p = n->parent;
		if (u->color == RED)
			Insert_case1(root, n);

		else {
			if (n == p->left && p == gp->left)
				Insert_case2_2(root, n);
			else if (n == p->left && p == gp->right)
				Insert_case2_1(root, n);

			else if (n == p->right && p == gp->right)
				Insert_case2_2(root, n);
			else
				Insert_case2_1(root, n);
		}

		return;
	}
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
void Insert_case1(Node**root,Node* n)
{
	Node* u = getUncle(n);
	Node* g = getGrandparent(n);
	Node* p = n->parent;

	p->color = u->color = BLACK;

	if (g != *root) {
		g->color = RED;
		if (g->parent->color != BLACK)
			Insert_pNotBlack(root,g);
		else
			return;
	}
	else
		return;

}
void Insert_case2_1(Node**root,Node* n)
{
	rotate_left(root,n);
	if (n->right->color == RED)
		Insert_case2_2(root,n->right);

	else
		Insert_case2_2(root,n->left);

	return;
}

void Insert_case2_2(Node** root,Node* n)
{
	*root=rotate_right(root,n);
	return;
}
Node* rotate_right(Node** root,Node* n)
{
	Node* p = n->parent;
	Node* gp = getGrandparent(n);
	Node* tmp;
	if (*root == gp) {
		root = &p;
		p->parent = NIL;
	}
	else {
		p->parent = gp->parent;

		if (gp == gp->parent->right)
			gp->parent->right = p;
		else
			gp->parent->left = p;
		
	}

	if (n == p->left) {
		tmp = p->right;
		p->right = gp;
		gp->left = tmp;
		if(tmp!=NIL) tmp->parent = gp;
	
		}
	else {
		tmp = p->left;
		p->left = gp;
		gp->right = tmp;
		if(tmp!=NIL)tmp->parent = gp;
	}

	n->parent = p;
	gp->parent = p;
	p->color = BLACK;
	gp->color = RED;
	return *root;
}
void rotate_left(Node** root,Node* n)
{
	Node* g = getGrandparent(n);
	Node* p = n->parent;
	Node* tmp;
	if (n == p->left) {
		g->right = n;
		tmp = n->right;
		p->left = tmp;
		if(tmp!=NIL) 
			tmp->parent = p;
		n->right = p;
	}
	else {
		g->left = n;
		tmp = n->left;
		p->right = tmp;
		if(tmp!=NIL)
			tmp->parent = p;
		n->left = p;
	}
	n->parent = g;
	p->parent = n;

	return;

}
