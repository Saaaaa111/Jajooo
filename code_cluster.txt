import networkx as nx
import matplotlib.pyplot as plt
import xlrd
import xlrd
from netgraph import Graph
import community

file="./rr.xls"
#----------

#------------
G = nx.karate_club_graph() 

names=[]
workbook = xlrd.open_workbook(file)
sheet=workbook.sheet_by_index(0)
for row in range (sheet.nrows):
    data=sheet.row_slice(row)
    person1=data[0].value
    person2=data[1].value
    names.append((person1,person2 ))
    
G.add_edges_from(names)
print(G)
#nx.draw(G , with_labels=True)
#nx.draw_random(G , with_labels=True)
#nx.draw_circular(G , with_labels=True)
#-------------------------------------------
partition = community.best_partition(G)
pos = nx.spring_layout(G)
plt.figure(figsize=(8, 8))  # image is 8 x 8 inches
nx.draw(
    G, pos, edge_color='black', width=1, linewidths=1,
    node_size=500, node_color='pink', alpha=0.9,
    labels={node: node for node in G.nodes()}
)
#nx.draw_spectral(G , with_labels=True ) 
nx.draw_networkx_nodes(G, pos, node_size=600, cmap=plt.cm.RdYlBu, node_color=list(partition.values()))
nx.draw_networkx_edges(G, pos, alpha=0.3)

#-----------------------
#nx.draw_circular(G , with_labels=True) 
#partition_sizes = [10, 20, 30, 40]
#G=nx.random_partition_graph(partition_sizes, 0.5, 0.1)
#nx.draw_spectral(G , with_labels=True)

plt.axis('off')     
plt.show()
plt.savefig('graph.png')
