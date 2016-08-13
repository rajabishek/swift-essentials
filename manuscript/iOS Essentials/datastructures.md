package com.rajabishek;

import java.util.Arrays;
import java.util.LinkedList;
import java.util.ListIterator;

class Graph {

    private int nodes;

    private boolean[] visited;

    private LinkedList<Integer>[] adj;

    Graph(int nodes) {
        this.nodes = nodes;
        visited = new boolean[nodes];

        adj = new LinkedList[nodes];

        for(int i=0; i<nodes; ++i) {
            adj[i] = new LinkedList<>();
        }
    }

    public void addEdge(int node1, int node2) {
        adj[node1].add(node2);
    }

    public void bfs(int node) {
        Arrays.fill(visited, false);
        LinkedList<Integer> queue = new LinkedList<>();

        visited[node] = true;
        System.out.print(node + " ");
        queue.add(node);

        while (!queue.isEmpty()) {
            node = queue.poll();

            for(int n : adj[node]) {
                if(visited[n] == false) {
                    visited[n] = true;
                    System.out.print(n + " ");
                    queue.add(n);
                }
            }
        }
    }
}

public class Main {
    public static void main(String[] args) {
        Graph g = new Graph(4);

        g.addEdge(0, 1);
        g.addEdge(0, 2);
        g.addEdge(1, 2);
        g.addEdge(2, 0);
        g.addEdge(2, 3);
        g.addEdge(3, 3);

        g.dfs(2);
        g.bfs(2);
    }
}