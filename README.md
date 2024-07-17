# L2DAA

PRIMS

import java.util.*;
import java.io.*;
class Prims{
  void prim(int n,int arr[][]){
    int i,j,k,u,v;
    int d[] = new int[10];
    int s[] = new int[10];
    int p[] = new int[10];
    int t[][] = new int[10][2];
    int min=9999;
    int source = 0;
    for(i=0;i<n;i++){
      for(j=0;j<n;j++){
        if(arr[i][j]!=0 && arr[i][j]<=min){
          min = arr[i][j];
          source = i;
        }
      }
    }
    for(i=0;i<n;i++){
      d[i] = arr[source][i];
      s[i] = 0;
      p[i] = source;
    
    }
    s[source] = 1;
    int sum=0;
    k=0;
    for(i=1;i<n;i++){
      min=9999;
      u=-1;
      for(j=0;j<n;j++){
        if(s[j]==0){
          if(d[j]<min){
            min = d[j];
            u=j;
          }
        }
      }
      if(u==-1){
        return;
      }
      t[k][0]= u;
      t[k][1] = p[u];
      k++;
      sum = sum+arr[u][p[u]];
      s[u] = 1;
      for(v=0;v<n;v++){
        if(s[v]==0 && arr[u][v]<d[v]){
          d[v] = arr[u][v];
          p[v] = u;
        }
      }
    }
    if(sum>=9999){
      System.out.println("ST doesnot exists");
    }
    else{
      System.out.println("ST exists and min cost is : ");
      for(i=0;i<n-1;i++){
        System.out.println(t[i][0]+"->"+t[i][1]);
      }
      System.out.println("Cost of st: "+sum);
    }
  }
  public static void main(String args[]){
    Scanner sc = new Scanner(System.in);
    int i,j,n;
    int arr[][] = new int[10][10];
    System.out.println("enter the number of nodes : ");
    n = sc.nextInt();
    System.out.println("enter the adj matrix : ");
    for(i=0;i<n;i++){
      for(j=0;j<n;j++){
        arr[i][j] = sc.nextInt();
      }
    }
    Prims p =new Prims();
    p.prim(n,arr);
  }
}


KRUSKALS

import java.util.*;
import java.io.*;
class Kruskals{
  public static void main(String args[]){
    Scanner sc = new Scanner(System.in);
    int cost[][] = new int[10][10];
    int mincost = 0;
    int i,j,n;
    System.out.println("Enter the number of nodes : ");
    n = sc.nextInt();
    System.out.println("Enter the adj matrix : ");
    for(i=1;i<=n;i++){
      for(j=1;j<=n;j++){
        cost[i][j] = sc.nextInt();
      }
    }
    mincost = kruskals(n,mincost,cost);
    System.out.println("the min cost of st is : " + mincost);
    
  }
  static int kruskals(int n,int mincost,int cost[][]){
    int ne=1,a=0,b=0,u=0,v=0,min;
    int parent[] = new int[10];
    while(ne<n){
      min=999;
      for(int i=1;i<=n;i++){
        for(int j=1;j<=n;j++){
          if(cost[i][j]<min){
            min =cost[i][j];
            a=u=i;
            b=v=j;
          }
        }
      }
      if(parent[u]>0){
        u = parent[u];
      }
      if(parent[v]>0){
        v = parent[v];
      }
      if(u!=v){
        System.out.println((ne++)+" min edge is: (" + a +"->"+b+") and its cost is : " + min);
        mincost+=min;
        parent[v] = u;
      }
      cost[a][b] = cost[b][a] = 999;
    }
    return mincost;
  }
}


FLOYDS

import java.util.*;
class flyods{
  public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);
    System.out.println("Enter the number of ele: ");
    int n =sc.nextInt();
    System.out.println("Enter the adj matrxi: ");
    int adj[][] = new int[10][10];
    for(int i=1;i<=n;i++){
      for(int j=1;j<=n;j++){
        adj[i][j] = sc.nextInt();
      }
    }
    flyod(adj,n);
    System.out.println("the all pair shoretst path is: ");
    for(int i=1;i<=n;i++){
      for(int j=1;j<=n;j++){
        System.out.print(adj[i][j]+" ");
      }
      System.out.println();
    }
  }
  static void flyod(int arr[][],int n){
    for(int k=1;k<=n;k++){
      for(int i=1;i<=n;i++){
        for(int j=1;j<=n;j++){
          arr[i][j] = min(arr[i][j],(arr[i][k]+arr[k][j]));
        }
      }
    }
  }
  static int min(int a,int b){
    if(a<b){
      return a;
    }
    return b;
  }
}

BELLMAN FORD

import java.util.*;

public class BellmanFord {
    static class Edge {
        int source, destination, weight;
    }
    static void bellmanFord(int V, int E, Edge[] edges, int src) {
        int[] dist = new int[V];
        for (int i = 0; i < V; i++)
            dist[i] = Integer.MAX_VALUE;
        dist[src] = 0;

        for (int i = 1; i < V; i++) {
            for (Edge e : edges) {
                int u = e.source;
                int v = e.destination;
                int w = e.weight;
                if (dist[u] != Integer.MAX_VALUE && dist[u] + w < dist[v])
                    dist[v] = dist[u] + w;
            }
        }     
        for (Edge e : edges) {
            int u = e.source;
            int v = e.destination;
            int w = e.weight;
            if (dist[u] != Integer.MAX_VALUE && dist[u] + w < dist[v])
                System.out.println("Graph contains negative weight cycle");
        }   
        for (int i = 0; i < V; i++)
            System.out.println("Distance from source " + src + " to vertex " + i + " is " + dist[i]);
    }
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        System.out.println("Enter the number of vertices:");
        int V = scanner.nextInt();

        System.out.println("Enter the number of edges:");
        int E = scanner.nextInt();

        Edge[] edges = new Edge[E];

        System.out.println("Enter the source, destination, and weight of each edge:");
        for (int i = 0; i < E; i++) {
            edges[i] = new Edge();
            edges[i].source = scanner.nextInt();
            edges[i].destination = scanner.nextInt();
            edges[i].weight = scanner.nextInt();
        }
        System.out.println("Enter the source vertex:");
        int src = scanner.nextInt();
        bellmanFord(V, E, edges, src);
    }
}

HAMILTONIAN

import java.util.*;
import java.io.*;
import java.util.Arrays;
class hamiltonian{
  private static int V,pc;
  private static int path[];
  private static int graph[][];
  static void findCycle(int g[][]){
    V = g.length;
    path =new int[V];
    Arrays.fill(path,-1);
    graph = g;
    try{
      pc =1;
      path[0] = 0;
      solve(0);
      System.out.println("no slution");
    }
    catch(Exception e){
      System.out.println(e.getMessage());
      display();
    }
  }
  static void solve(int vertex) throws Exception{
    if(graph[vertex][0]==1 && pc==V){
      throw new Exception ("solution found");
    }
    if(pc==V){
      return;
    }
    for(int v=0;v<V;v++){
      if(graph[vertex][v]==1){
        path[pc++]=v;
        graph[vertex][v] = graph[v][vertex] = 0;
        if(!isPresent(v)){
          solve(v);
        }
        graph[vertex][v] = graph[v][vertex] = 1;
        path[--pc]=-1;
      }
    }
  }
  static boolean isPresent(int v){
    for(int i=0;i<pc-1;i++){
      if(path[i]==v){
        return true;
      }
    }
    return false;
  }
  static void display(){
    System.out.println("Path: ");
    for(int i=0;i<=V;i++){
      System.out.print(path[i%V]+" ");
    }
    System.out.println();
  }
  public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);
    System.out.println("Enter the number of vertieces: ");
    int n = sc.nextInt();
    int graph[][] = new int[n][n];
    System.out.println("Enter the adj matrix: ");
    for(int i=0;i<n;i++){
      for(int j=0;j<n;j++){
        graph[i][j] = sc.nextInt();
      }
    }
    findCycle(graph);
  }
}


SUBSETS

import java.util.*;
class subsets{
  static int c=0;
  public static void main(String[] args) {
    int x[] = new int[10];
    Scanner sc = new Scanner(System.in);
    System.out.println("Enter the number of ele: ");
    int n = sc.nextInt();
    System.out.println("enter the ele in inc order: ");
    int w[] = new int[10];
    for(int i=0;i<n;i++){
      w[i] = sc.nextInt();
    }
    System.out.println("Enter the positive sum: ");
    int d = sc.nextInt();
    int sum = 0;
    for(int i=0;i<n;i++){
      sum+=w[i];
    }
    if(sum<d || w[0]>d){
      System.out.println("no solution");
      System.exit(0);
    }
    subset(0,0,sum,x,w,d);
    if(c==0){
      System.out.println("no solution");
    }

  }
  static void subset(int cs,int k,int r,int x[],int w[],int d){
    x[k]=1;
    if(cs+w[k]==d){
      c++;
      System.out.println("solution "+c+": {");
      for(int i=0;i<=k;i++){
        if(x[i]==1){
          System.out.print(w[i]+",");
        }
      }
      System.out.println("}");
    }
    else if(cs+w[k]+w[k+1]<=d){
      subset(cs+w[k],k+1,r-w[k],x,w,d);
    }
    if((cs+r-w[k])>=d && (cs+w[k+1])<=d){
      x[k] = 0;
      subset(cs,k+1,r-w[k],x,w,d);
    }
  }
}


KNAPSACk

import java.util.Arrays;

class Item implements Comparable<Item> {
    int weight;
    int value;
    double ratio;

    public Item(int weight, int value) {
        this.weight = weight;
        this.value = value;
        this.ratio = (double) value / weight;
    }

    @Override
    public int compareTo(Item other) {
        if (this.ratio < other.ratio) {
            return 1;
        } else if (this.ratio > other.ratio) {
            return -1;
        }
        return 0;
    }
}

public class KnapsackBranchBound {
    static int knapsack(int[] weights, int[] values, int capacity) {
        int n = weights.length;
        Item[] items = new Item[n];
        for (int i = 0; i < n; i++) {
            items[i] = new Item(weights[i], values[i]);
        }
        Arrays.sort(items);

        return branchAndBound(items, capacity, 0, 0, 0);
    }

    static int branchAndBound(Item[] items, int capacity, int index, int currentWeight, int currentValue) {
        if (currentWeight > capacity) {
            return 0;
        }

        if (index == items.length) {
            return currentValue;
        }

        int withItem = 0;
        if (currentWeight + items[index].weight <= capacity) {
            withItem = branchAndBound(items, capacity, index + 1, currentWeight + items[index].weight, currentValue + items[index].value);
        }

        int withoutItem = branchAndBound(items, capacity, index + 1, currentWeight, currentValue);

        return Math.max(withItem, withoutItem);
    }

    public static void main(String[] args) {
        int[] weights = {10, 20, 30};
        int[] values = {60, 100, 120};
        int capacity = 50;

        int maxValue = knapsack(weights, values, capacity);
        System.out.println("Maximum value that can be obtained: " + maxValue);
    }
}



WARSHALL

import java.util.Scanner;

public class WarshallAlgorithm {
    final static int INF = 99999; // Use a large number to represent infinity

    // Function to print the solution matrix
    void printSolution(int reach[][], int V) {
        System.out.println("Following matrix is the transitive closure of the given graph:");
        for (int i = 0; i < V; i++) {
            for (int j = 0; j < V; j++) {
                System.out.print(reach[i][j] + " ");
            }
            System.out.println();
        }
    }

    // Function to compute the transitive closure using Warshall's algorithm
    void transitiveClosure(int graph[][], int V) {
        int reach[][] = new int[V][V];
        int i, j, k;

        // Initialize the reach matrix as the input graph matrix
        for (i = 0; i < V; i++) {
            for (j = 0; j < V; j++) {
                reach[i][j] = graph[i][j];
            }
        }

        // Update the reach matrix using Warshall's algorithm
        for (k = 0; k < V; k++) {
            for (i = 0; i < V; i++) {
                for (j = 0; j < V; j++) {
                    reach[i][j] = (reach[i][j] != 0) || ((reach[i][k] != 0) && (reach[k][j] != 0)) ? 1 : 0;
                }
            }
        }

        // Print the solution
        printSolution(reach, V);
}

public static void main(String[] args) {
    Scanner scanner = new Scanner(System.in);

    System.out.println("Enter the number of vertices:");
    int V = scanner.nextInt();

    int graph[][] = new int[V][V];

    System.out.println("Enter the adjacency matrix (0 for no edge, 1 for edge):");
    for (int i = 0; i < V; i++) {
        for (int j = 0; j < V; j++) {
            graph[i][j] = scanner.nextInt();
        }
    }

        WarshallAlgorithm wa = new WarshallAlgorithm();
        wa.transitiveClosure(graph, V);
    }
}
