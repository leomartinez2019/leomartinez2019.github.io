---
layout: post
title:  "Percolacion Python"
description: "Percolacion"
date:   2017-04-16 00:15:28 -0500
categories: python
---

Este es el código correspondiende al la tarea de algoritmos de Sedgeway.

{% highlight python %}

""" Sedgewick Percolation task """

from random import choice

class Percolation(object):
    """ Determina la grilla y sus propiedades """
    def __init__(self, row, col):
        self.row = row
        self.col = col
        self.grid = []
        fila = []
        self.virtual_top = Point(0, 0)
        self.virtual_bottom = Point(row + 1, 0)
        for row_indx in range(1, row + 1):
            for col_indx in range(1, col + 1):
                point = Point(row_indx, col_indx)
                fila.append(point)
            self.grid.append(fila)
            fila = []
        self.unir_extremos_virtuales()

    def unir_extremos_virtuales(self):
        """ Crea uniones con extremos virtuales """
        top_row = self.grid[0]
        bottom_row = self.grid[self.col - 1]
        for elem in top_row:
            self.union(elem, self.virtual_top)
        for elem in bottom_row:
            self.union(elem, self.virtual_bottom)

    def __str__(self):
        cadena = ""
        for row in self.grid:
            for point in row:
                cadena += str(point) + " "
            cadena += "\n"
        return cadena

    def get_point(self, tupla):
        """ Retorna nodo de la grilla """
        row = tupla[0]
        col = tupla[1]
        if row == 0:
            return self.virtual_top
        elif row == self.row + 1:
            return self.virtual_bottom
        else:
            return self.grid[row - 1][col - 1]

    def get_top(self):
        """ Retorna el nodo virtual superior """
        return self.virtual_top

    def get_bottom(self):
        """ Retorna el nodo virtual inferior """
        return self.virtual_bottom

    def is_full(self, point):
        """ Indica que el nodo esta conectado al nodo virtual superior """
        return self.connected(point, self.virtual_top)

    def percolates(self):
        """ Determina si los dos nodos extremos estan conectados """
        return self.connected(self.virtual_bottom, self.virtual_top)

    def open_point(self, row, col):
        """ Abre un nodo y crea conecciones """
        row_values = range(1, self.row + 1)
        col_values = range(1, self.col + 1)
        point = self.get_point((row, col))
        point.open()
        arriba = row + 1
        abajo = row - 1
        derecha = col + 1
        izquierda = col - 1
        if arriba in row_values:
            point_arriba = self.get_point((arriba, col))
            if point_arriba.is_open():
                self.union(point, point_arriba)
        if abajo in row_values:
            point_abajo = self.get_point((abajo, col))
            if point_abajo.is_open():
                self.union(point, point_abajo)
        if derecha in col_values:
            point_derecha = self.get_point((row, derecha))
            if point_derecha.is_open():
                self.union(point, point_derecha)
        if izquierda in col_values:
            point_izquierda = self.get_point((row, izquierda))
            if point_izquierda.is_open():
                self.union(point, point_izquierda)

    def root(self, point):
        """ Retorna la raiz de un nodo dado por sus coordenadas """
        point_coord = point.get_coord()
        point_root = point.get_root()
        while point_root != point_coord:
            nuevo = self.get_point(point_root)
            point_root = nuevo.get_root()
            point_coord = nuevo.get_coord()
        return point_root

    def connected(self, point1, point2):
        """ Verifica que dos nodos esten conectados """
        return self.root(point1) == self.root(point2)

    def union(self, point1, point2):
        """ Une dos nodos """
        root1 = self.root(point1)
        root2 = self.root(point2)
        target = self.get_point(root1)
        target.set_root(root2)


class Point(object):
    """ Nodo de la grilla """
    def __init__(self, row, col):
        self.coord = (row, col)
        self.root = (row, col)
        self.opened = False
        self.full = False

    def __str__(self):
        if self.opened:
            return "+"
        return "."

    def get_coord(self):
        """ Retorna la coordenada """
        return self.coord

    def get_root(self):
        """ Retorna la raiz """
        return self.root

    def set_root(self, tupla):
        """ Da un valor a la raiz de un nodo """
        self.root = tupla

    def open(self):
        """ Abre un nodo """
        self.opened = True

    def is_open(self):
        """ Determina que un node esta abierto """
        return self.opened

    def fill(self):
        """TODO"""
        self.full = True

    def is_full(self):
        """TODO"""
        return self.full


def client():
    """ Cliente de prueba """
    pepe = Percolation(8, 8)
    lista = range(1, 9)
    contador = 0
    control = []
    while True:
        fila = choice(lista)
        columna = choice(lista)
        tupla = (fila, columna)
        if tupla not in control:
            pepe.open_point(fila, columna)
            control.append(tupla)
            contador += 1
            if pepe.percolates():
                break
    print control, contador

client()

{% endhighlight %}
