---
layout: post
title: Pre-Order, In-Order, Post-Order Traversal
category: 算法
tags: [BST, traversal, Leetcode]
keywords: BST, Leetcode
---

## Pre-Order, In-Order, Post-Order

1. [Pre-Order](http://www.programcreek.com/2012/12/leetcode-solution-for-binary-tree-preorder-traversal-in-java/)
2. [In-Order](http://www.programcreek.com/2012/12/leetcode-solution-of-binary-tree-inorder-traversal-in-java/)
3. [Post-Order](http://www.programcreek.com/2012/12/leetcode-solution-of-iterative-binary-tree-postorder-traversal-in-java/)

## Leetcode 145. Binary Tree Postorder Traversal

1. Recursive
2. Iterative

```java
//my recurssive solution
public List<Integer> postorderTraversal_recurssive(TreeNode root) {

	List<Integer> res =new ArrayList<Integer>();

	if(root==null) return res;
	if(root.left !=null){
		List<Integer>  left  = postorderTraversal_recurssive(root.left);
		for(int ele : left)
			res.add(ele);
	}
	if(root.right!=null){
		List<Integer>  right = postorderTraversal_recurssive(root.right);
		for(int ele : right)
			res.add(ele);
	}
	res.add(root.val);
	return res;
}

//my iterative solution
public List<Integer> postorderTraversal_iterative(TreeNode root) {
	List<Integer> res_reverse =new ArrayList<Integer>();
	if(root==null)  return res_reverse;

	Stack<TreeNode> stack =new Stack<TreeNode>();

	stack.push(root);
	while(!stack.isEmpty()){
		TreeNode cur = stack.pop();
		res_reverse.add(cur.val);
		if(cur.left!=null)
			stack.push(cur.left);
		if(cur.right!=null)
			stack.push(cur.right);
	}

	List<Integer> res =new ArrayList<Integer>();
	for(int i=res_reverse.size()-1;i>=0;i--){
		res.add(res_reverse.get(i));
	}
	return res;
}

//Using stack, iterative
//Put a simple sample tree in your mind when thinking
//    8
//   / \
//  5   9
//   \
//    6
public ArrayList<Integer> postorderTraversal(TreeNode root) {
  ArrayList<Integer> lst = new ArrayList<Integer>();
  if(root == null) return lst;

  Stack<TreeNode> stack = new Stack<TreeNode>();
  stack.push(root);

  TreeNode prev = null;
  while(!stack.empty()){
      TreeNode curr = stack.peek();

      // go down the tree.
      //check if current node is leaf, if so, process it and pop stack,
      //otherwise, keep going down
      if(prev == null || prev.left == curr || prev.right == curr){
          //prev == null is the situation for the root node
          if(curr.left != null){
              stack.push(curr.left);
          }else if(curr.right != null){
              stack.push(curr.right);
          }else{
              stack.pop();
              lst.add(curr.val);
          }

      //go up the tree from left node
      //need to check if there is a right child
      //if yes, push it to stack
      //otherwise, process parent and pop stack
      }else if(curr.left == prev){
          if(curr.right != null){
              stack.push(curr.right);
          }else{
              stack.pop();
              lst.add(curr.val);
          }

      //go up the tree from right node
      //after coming back from right node, process parent node and pop stack.
      }else if(curr.right == prev){
          stack.pop();
          lst.add(curr.val);
      }

      prev = curr;
  }
  return lst;
}

//Using stack, iterative
//Put a simple sample tree in your mind when thinking
//    8
//   / \
//  5   9
//   \
//    6
public List<Integer> postorderTraversal2(TreeNode root) {
  List<Integer> res = new ArrayList<Integer>();

  if(root==null) return res;

  Stack<TreeNode> stack = new Stack<TreeNode>();
  stack.push(root);

  while(!stack.isEmpty()) {
      TreeNode temp = stack.peek();
      if(temp.left==null && temp.right==null) {//pop condition
          TreeNode pop = stack.pop();
          res.add(pop.val);
      }
      else {
          if(temp.right!=null) {
              stack.push(temp.right);
              temp.right = null;//in order to fit the pop condition
          }
          if(temp.left!=null) {
              stack.push(temp.left);
              temp.left = null;//in order to fit the pop condition
          }
      }
  }
  return res;
}
```
