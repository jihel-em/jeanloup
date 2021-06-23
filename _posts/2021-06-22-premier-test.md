---
layout: post
title:  "Mon premier post ^^"
date:   2021-06-22 15:29:00 +0200
categories: posts jekyll
rangement: jekyll
---
Ceci est mon premier post test.
Voici donc un bout de tp d'algo pour tester :

~~~ python
import matplotlib.pyplot as plt
import networkx as nx
import queue
from networkx.algorithms.flow import shortest_augmenting_path

def visuGraphe(G):
    for n1,n2,attr in G.edges(data=True):
        print(n1,n2,attr)
        
def get_capacity(G):
    return nx.get_edge_attributes(G,'capacity')

def get_rsd(G):
    return nx.get_edge_attributes(G,'residuel')


def fermeture(G):
    C=G.copy()
    for k in C.nodes():
        for i in C.nodes():
            for j in C.nodes():
                if(j not in C.successors(i)):
                    if(k in C.successors(i) and j in C.successors(k)):
                        C.add_edge(i,j,weight=C[i][k]['capacity']+C[k][j]['capacity'])
    return C


def RoyWarshall(G):
    for x in G.nodes():
        for y in G.nodes():
            e1 =(x,y)
            if G.has_edge(*e1):
                for z in G.nodes():
                    e2=(y,z)
                    if G.has_edge(*e2):
                        e3 = (x,z)
                        if not G.has_edge(*e3):
                            G.add_edge(*e3,weight=0)
    return G





def Ford_Fulkerson(G,e,s):
    flotMax=0
    for u in G.nodes():
        for i in G.neighbors(u):
            G[u][i]['residuel']=G[u][i]['capacity']
    Parents = {}
    while BFS(G,e,s,Parents):
        path=findPath(Parents,e,s)
        flot=flotMin(G,path)
        flotMax+=flot
        updateflow(G,path,flot)
    return flotMax
    
def BFS(G,e,s,Parents):
    for nd in G.nodes():    #On attache à chaque noeud une variable 'visited'
        G.nodes[nd]['visited']=False
    q = queue.Queue()
    q.put(e)
    G.nodes[e]['visited']=True
    while q.qsize()>0:
        u=q.get()
        for i in G.neighbors(u):
            if not G.nodes[i]['visited'] and G[u][i]['residuel']>0:
                q.put(i)
                G.nodes[i]['visited']=True
                Parents[i] = u
    return G.nodes[s]['visited']
    

def findPath(Parents,e,s):
    source=s
    path=[s]
    while source!=e:
        ns=Parents[source]
        path.append(ns)
        source=ns
    path.reverse()
    return path

def flotMin(G,path):
    mini=float('inf')
    for i in range(len(path)-1):
        mini=min(mini,G[path[i]][path[i+1]]['residuel'])
    return mini

def updateflow(G,path,flot):
    for i in range(len(path)-1):
        G[path[i]][path[i+1]]['residuel'] -= flot
        if not path[i] in G.successors(path[i+1]):
            G.add_edge(path[i+1],path[i])
            G[path[i+1]][path[i]]['residuel'] = -G[path[i]][path[i+1]]['residuel']
        else:
            G[path[i+1]][path[i]]['residuel'] -= G[path[i]][path[i+1]]['residuel']
  
~~~
