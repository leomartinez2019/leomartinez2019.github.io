---
layout: post
title:  "Percolacion Java"
description: "Percolacion"
date:   2017-04-24 00:15:28 -0500
categories: python
---

Este es el código correspondiende al la tarea de algoritmos de Sedgeway.

{% highlight java %}
import edu.princeton.cs.algs4.StdRandom;
import edu.princeton.cs.algs4.StdStats;
import edu.princeton.cs.algs4.StdOut;
import edu.princeton.cs.algs4.StdIn;
import edu.princeton.cs.algs4.WeightedQuickUnionUF;

public class Percolation {
  private boolean[] openness;
  public WeightedQuickUnionUF Lista;
  private int num;
  public int numberOfOpenSites =  0;
  public Percolation(int n) {
    num = n;
    Lista = new WeightedQuickUnionUF(n * n + 2);
    openness = new boolean[n * n + 2];
    openness[n * n] = true;
    openness[n * n  + 1] = true;
  }

  /**
    * Maps row and col to array index
    */
  public int translation(int row, int col) {
    int index;
    index = (row - 1) * num + col - 1;
    return index;
  }

  /**
    * Opens a site and runs unions when neighbors are open
    */
  public void open(int row, int col) {
    if (row < 1 || row > num) {
            throw new IndexOutOfBoundsException("row " + row + " is not between 1 and " + (num));  
    }
    if (col < 1 || col > num) {
            throw new IndexOutOfBoundsException("col " + col + " is not between 1 and " + (num));  
    }
    int indice = translation(row, col);
    if (row == 1) {                    // join virtual top
      int vTop = translation(num + 1, 1);
      Lista.union(indice, vTop);
    }
    else if (row == num) {     // join virtual bottom
      int vBot = translation(num + 1, 2);
      Lista.union(indice, vBot);
    }
    openness[indice] = true;
    numberOfOpenSites++;
    int arriba;
    int abajo;
    int derecha;
    int izquierda;
    if (row - 1 > 0) {
      arriba = row - 1;
      int ind_arriba = translation(arriba, col);
      if (openness[ind_arriba]) {
        Lista.union(indice, ind_arriba);
      }
    }
    if (row + 1 <= num) {
      abajo = row + 1;
      int ind_abajo = translation(abajo, col);
      if (openness[ind_abajo]) {
        Lista.union(indice, ind_abajo);
      }
    }
    if (col - 1 > 0) {
      izquierda = col - 1;
      int ind_izquierda = translation(row, izquierda);
      if (openness[ind_izquierda]) {
        Lista.union(ind_izquierda, indice);
      }
    }
    if (col + 1 <= num) {
      derecha = col + 1;
      int ind_derecha = translation(row, derecha);
      if (openness[ind_derecha]) {
        Lista.union(ind_derecha, indice);
      }
    }
  }

  /**
    * Determines if site is open
    */
  public boolean isOpen(int row, int col) {
    if (row < 1 || row > num) {
            throw new IndexOutOfBoundsException("row " + row + " is not between 1 and " + (num));  
    }
    if (col < 1 || col > num) {
            throw new IndexOutOfBoundsException("col " + col + " is not between 1 and " + (num));  
    }
    int indice = translation(row, col);
    return openness[indice];
  }

  /**
    * Opens a site and runs unions when neighbors are open
    */
  public boolean isFull(int row, int col) {
    if (row < 1 || row > num + 1) {
            throw new IndexOutOfBoundsException("row " + row + " is not between 1 and " + (num));  
    }
    if (col < 1 || col > num) {
            throw new IndexOutOfBoundsException("col " + col + " is not between 1 and " + (num));  
    }
    int vTop = translation(num + 1, 1);
    int indice = translation(row, col);
    return Lista.connected(indice, vTop);
  }

  /**
    * Opens a site and runs unions when neighbors are open
    */
  public boolean percolates() {
    //int vTop = translation(num + 1, 1);
    //int vBot = translation(num + 1, 2);
    return isFull(num + 1, 2);
    //return Lista.connected(vBot, vTop);
  }

}

public class PercolationStats {
    private double[] secuencia;
    private double threshold;
    private double desv;
    private int tries;
    public PercolationStats(int n, int trials) {
        tries = trials;
        secuencia = new double[trials];
        int numSites = n * n;
        //Percolation nuevo = new Percolation(n);
        for (int i = 0; i < trials; i++) {
            Percolation nuevo = new Percolation(n);
            while (!nuevo.percolates()) {
                int x = StdRandom.uniform(n) + 1;
                int y = StdRandom.uniform(n) + 1;
                nuevo.open(x, y);
            }
            threshold = (double)nuevo.numberOfOpenSites / (double)numSites;
            secuencia[i] = threshold;
        }
    }
    public double mean() {
        return StdStats.mean(secuencia);
    }

    public double stddev() {
        return StdStats.stddev(secuencia);
    }
    public double confidenceLo() {
        return mean() - 1.96 * stddev() / Math.sqrt(tries);
    }
    public double confidenceHi() {
        return mean() + 1.96 * stddev() / Math.sqrt(tries);
    }
    public static void main(String[] args) {
        int T = StdIn.readInt();
        int n = StdIn.readInt();
        PercolationStats estadistico = new PercolationStats(n, T);
        double mean = estadistico.mean();
        double desvStd = estadistico.stddev();
        double intervalLo = estadistico.confidenceLo();
        double intervalHi = estadistico.confidenceHi();
        StdOut.printf("mean: \t = %f", mean);
        StdOut.println();
        StdOut.printf("stddev: \t = %f", desvStd);
        StdOut.println();
        StdOut.printf("95%% confidence interval \t = [%f, %f]", intervalLo, intervalHi);
    }
}

{% endhighlight %}