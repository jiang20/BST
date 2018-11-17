# BST
package BinarySearchTree;

import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner input = new Scanner(System.in);
        long  operation_num = input.nextLong();
        //long[] operation = new long[line + 1];
        String[] do_operation = new String[(int)operation_num + 1];
        for (int i = 0; i < do_operation.length; i++) {
            do_operation[i] = input.nextLine();
        }
        BinarySearchTree tree = new BinarySearchTree();//get empty tree
        for (int i = 0; i < do_operation.length; i++) {
            use_function(tree, do_operation[i]);
        }
    }
        public static void use_function (BinarySearchTree tree, String string){
            String operation = "";
            long operation_num = 0;
            int i = 0;
            for (; i < string.length(); i++) {
                if (string.charAt(i) != ' ') {
                    operation += string.charAt(i);
                }
                else break;
            }//get operation
            if (operation.equals("delete_interval")) {
                long operation_num_1 = 0;
                long operation_num_2 = 0;
                for (i = i + 1; string.charAt(i) != ' '; i++) {
                    operation_num_1 = operation_num_1 * 10 + (long) (string.charAt(i) - '0');
                }
                for (i = i + 1; i < string.length(); i++) {
                    operation_num_2 = operation_num_2 * 10 + (long) (string.charAt(i) - '0');
                }
                tree.delete_interval(operation_num_1, operation_num_2);
            } else {
                for (i = i + 1; i < string.length(); i++) {
                    operation_num = operation_num * 10 + (long) (string.charAt(i) - '0');
                }
                if (operation.equals("insert")) {
                    tree.insert(operation_num);
                } else if (operation.equals("find")) {
                    tree.find(operation_num);
                } else if (operation.equals("find_ith")) {
                    tree.find_ith(operation_num);
                } else if (operation.equals("delete")) {
                    tree.delete(operation_num);
                } else if (operation.equals("delete_greater_than")) {
                    tree.delete_greater_than(operation_num);
                } else if (operation.equals("delete_less_than")) {
                    tree.delete_less_than(operation_num);
                }
            }
        }
    }
    
    
    package BinarySearchTree;

import java.nio.BufferUnderflowException;
import java.util.LinkedList;

//insert delete delete_less_than delete_greater_than delete_interval find find_ith
public class BinarySearchTree{
    private BinaryNode root;//奇怪为啥可以这样定义
    private long num ;
    private static class BinaryNode
    {
        long element;//data
        BinaryNode left;//left node
        BinaryNode right;//right node
        long frequency;
        BinaryNode(long element){
            this.setElement(element);
            this.left = null;
            this.right = null;
            frequency = 1;
        }

        BinaryNode(long element, BinaryNode left, BinaryNode right){
            this.setElement(element);
            this.left = left;
            this.right = right;
            frequency = 1;
        }

        public long  getElement() {
            return element;
        }

        public void setElement(long element) {
            this.element = element;
        }
    }
    public BinarySearchTree(){
        root = null;//new tree
    }
    public void makeEmpty(){
        root = null;
        num = 0;
    }
    public boolean isEmpty(){
        return root == null;
    }
    public boolean find(long x){
        return find(x, root);
    }
    public boolean contains(long x){
        return contains(x,root);
    }
    public long findMin(){
        if(isEmpty())
            throw new BufferUnderflowException();
        return findMin(root).getElement();
    }
    public long findMax(){
        if(isEmpty())
            throw new BufferUnderflowException();
        return findMax(root).getElement();
    }
    public void insert(long x){
        root = insert(x,root);
    }
    public void delete(long x){
        delete(x, root);
    }
    public boolean delete_less_than(long x){
        return delete_less_than(x,root);
    }
    public boolean delete_greater_than(long x){
        return delete_greater_than(x,root);
    }
    public boolean delete_interval(long x, long y){
        return delete_interval(x,y,root);
    }
    public void find_ith(long i){
        find_ith(i, root);
    }
    public BinaryNode getNode(long x){
        return getNode(x,root);
    }




    private boolean find(long x, BinaryNode t){
        if(t == null) {
            System.out.println('N');
            return false;
        }

        if(x < t.getElement() ){
            return find(x,t.left);
        }
        else if(x > t.getElement() ){
            return find(x, t.right);
        }
        else
            System.out.println('Y');
        return true;
    }
    public boolean contains(long x,BinaryNode t){//only return ,no print
        if(t == null) {
            return false;
        }
        if(x < t.getElement() ){
            return contains(x,t.left);
        }
        else if(x > t.getElement() ){
            return contains(x, t.right);
        }
        return true;
    }
    private BinaryNode findMin(BinaryNode t){
        if(isEmpty())
            return null;
        else if(t.left == null)
            return t;
        else
            return findMin(t.left);
    }
    private BinaryNode findMax(BinaryNode t){
        if(isEmpty())
            return null;
        else if(t.right == null)
            return t;
        else
            return findMax(t.right);
    }
    private BinaryNode insert(long x, BinaryNode t){
        if( t == null) {
            t = new BinaryNode(x);// insert the first node
            return t;
        }
        if(contains(x,t)){
            getNode(x,t).frequency++;
        }
        else {
            if (x < t.getElement()) {
                t.left = insert(x, t.left);
            } else if (x > t.getElement()) {
                t.right = insert(x, t.right);
            } else ;
        }
        num++;
        return t;
    }
    private BinaryNode delete(long x, BinaryNode t){
        if(t == null||!contains(x,t))
            return t;

        if(getNode(x,t).frequency > 1){
            getNode(x,t).frequency--;
            return t;
        }
        if(x < t.getElement()) {
            t.left = delete( x, t.left);
        }
        else if(x > t.getElement()){
            t.right = delete(x , t.right);
        }
        else if(t.left!= null&&t.right!=null){//删除节点有两个子节点
            t.element = findMin(t.right).getElement();
            t.right = delete(t.element,t.right);
        }
        else
            t = (t . left != null )? t.left : t.right;
        num --;
        return t;
    }
    private boolean delete_less_than(long x,BinaryNode t){
        if(t == null||findMin(t).getElement() >= x)
            return false;
        while (findMin(t).getElement() < x){
            delete(findMin(t).getElement(),t);
        }
        return true;
    }
    private boolean delete_greater_than(long x,BinaryNode t){
        if(t == null||findMax(t).getElement() <= x)
            return false;
        while (findMax(t).getElement() > x){
            delete(findMax(t).getElement(),t);
        }
        return true;
    }
    private boolean delete_interval(long x,long y,BinaryNode t){
        if(t == null||y < x||findMax(t).getElement() < y||findMin(t).getElement() > x)
            return false;//不用find_Min真的不好写啊感觉
        LinkedList<BinaryNode> list = new LinkedList<>();
        while (findMin(t).getElement() <= x){
            list.add(findMin(t));
            delete(findMin(t).getElement(),t);
        }
        while (findMin(t).getElement() < y){
            delete(findMin(t).getElement(),t);
        }
        int i = 0;
        while ( i < list.size()){
            insert(list.get(i).getElement(),t);
            i ++;
        }
        return true;
    }
    private void find_ith(long i, BinaryNode t){
        if(t == null ||i <= 0 ||i > num ) {
            System.out.println('N');
            return;
        }
        int j = 1;
        LinkedList<BinaryNode> list = new LinkedList<>();
        if(i == 1){
            System.out.println(findMin(t).getElement());
            return;
        }
        while (j < i){
            list.add(findMin(t));
            j++;
            delete(findMin(t).getElement());
        }
        long result = findMin(t).getElement();
        j = 1;
        while (j < i){
            insert(list.get(j-1).getElement(),t);
            j++;
        }
        System.out.println(result);
        return ;
    }
    private BinaryNode getNode(long x, BinaryNode t){//以该节点存在为前提
        if(t == null)
            return null;
        if( x < t.getElement()){
            return getNode(x,t.left);
        }
        else if(x > t.getElement()){
            return getNode(x,t.right);
        }
        return t;
    }
}

