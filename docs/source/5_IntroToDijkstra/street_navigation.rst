Street navigation
=================

GPS Navigation
--------------

People use mapping programs based on Global Positioning System (GPS) satellites every day to help them
drive from one place to another. They have been built into cars and are now available on portable
devices like cell phones and handheld navigators. They attempt to present the most optimal route
to get from one place to another.

.. image:: phone_navigation.png
    :width: 400

But what is the most optimal route? Think about some of the things that might make one route better
than another.

* Shortest distance to maximize fuel used
* Least travel time to get somewhere in a hurry
* Minimizing the elevation gains for hikers
* Minimizing the highway tolls that have to be paid
* Including stops for recharging an electric car

What are some of the things that might go into the calculation of the most optimal route?

* Road conogestion on a particular part of the route
* Road construction that might slow down travel
* Highway roads that might be longer vs more direct but slower city streets
* Accidents on a road that slow traffic that might even occur once the trip begins

What is Dijkstra's algorithm
----------------------------
Dijkstra's algorithm is a computer science algorithm used to find the shortest path between one node
and all other nodes in a weighted graph, essentially identifying the most efficient route through a
network where each connection has a designated cost or distance; it works by iteratively selecting
the closest unvisited node and updating the distances to its neighbors until all nodes have been visited. 

Key points about Dijkstra's algorithm:

.. csv-table::
    :widths: auto

    Purpose, "To find the shortest path from a single source node to all other nodes in a graph."
    How it works, "It greedily selects the node with the smallest known distance from the source node at each step,
    then updates the distances to its neighbors, ensuring that the shortest path is always chosen.
    
    Can be visualized as a *spreading wave* from the starting node."
    Applications, "**Navigation systems**: Finding the shortest route between two points on a map

    **Network routing**: Optimizing data packet routing in computer networks

    **Transportation logistics**: Finding the most efficient delivery route" 

Apps like Google Maps and Waze use Dijkstra's algorithm to calculate the shortest path from your
current location to your destination, taking into account factors like traffic and road quality.

Your final project
------------------
The final project that you will do is to find the shortest path between intersections
on a grid of squares. This is a simplified version of the problem just discussed, but
it has many of the same characteristics of the real case. To make it more realistic,
some of the intersections in the grid will be blocked, representing accidents or
road construction that your robot will have to avoid. 

The algorithm that we will use is very similar to what commercial GPS navigators use
to help us drive from one place to another. We will discuss it in more detail when we
get near the end of the course.