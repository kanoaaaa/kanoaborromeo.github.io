---
layout: project
type: project
image: img/pexels-photo-9559882 copy.jpeg
title: "Sea Of Islands"
date: 2024
published: true
labels:
  - Python
summary: "A partner and myself collbaorated together on a class assignment to use algorithms like priority queues and deques to model an archipelago using a weighted graph to efficiently direct resource distribution and tourist routes."
---
Kanoa Borromeo and Chester Leoso
ICS-311: Assignment 4 - Sea of Island
07/16/2024

import heapq
from collections import deque

class Island:
    def __init__(self, name, population):
        self.name = name
        self.population = population
        self.resources = {}
        self.experiences = []
        self.neighbors = {}

    def add_neighbor(self, neighbor, travel_time):
        self.neighbors[neighbor] = travel_time

    def add_resource(self, resource, quantity):
        self.resources[resource] = quantity

    def add_experience(self, experience, time_to_enjoy):
        self.experiences.append((experience, time_to_enjoy))


class Archipelago:
    def __init__(self):
        self.islands = {}

    def add_island(self, name, population):
        self.islands[name] = Island(name, population)

    def add_route(self, from_island, to_island, travel_time):
        if from_island in self.islands and to_island in self.islands:
            self.islands[from_island].add_neighbor(to_island, travel_time)
        else:
            raise ValueError("One or both of the islands not found in the archipelago")

    def add_resource_to_island(self, island, resource, quantity):
        if island in self.islands:
            self.islands[island].add_resource(resource, quantity)
        else:
            raise ValueError("Island not found in the archipelago")

    def add_experience_to_island(self, island, experience, time_to_enjoy):
        if island in self.islands:
            self.islands[island].add_experience(experience, time_to_enjoy)
        else:
            raise ValueError("Island not found in the archipelago")

# Example of creating an archipelago
archipelago = Archipelago()
archipelago.add_island("Hawaii", 1500000)
archipelago.add_island("Rapanui", 7000)
archipelago.add_island("Aotearoa", 5000000)
archipelago.add_island("Niihau", 130)

archipelago.add_route("Hawaii", "Rapanui", 15)
archipelago.add_route("Rapanui", "Aotearoa", 10)
archipelago.add_route("Hawaii", "Aotearoa", 20)
archipelago.add_route("Niihau", "Hawaii", 2)

archipelago.add_resource_to_island("Niihau", "kahelelani shells", 1000)
archipelago.add_experience_to_island("Hawaii", "Volcano Tour", 5)

# Algorithm 1: Distributing Resources from a Single Source
def distribute_resource(archipelago, start_island, resource, quantity):
    pq = [(0, start_island, quantity)]
    visited = set()
    distribution = {island: 0 for island in archipelago.islands}

    while pq:
        current_time, current_island, current_quantity = heapq.heappop(pq)
       
        if current_island in visited:
            continue

        visited.add(current_island)
        distribution[current_island] += current_quantity
       
        for neighbor, travel_time in archipelago.islands[current_island].neighbors.items():
            if neighbor not in visited:
                next_quantity = min(current_quantity, archipelago.islands[neighbor].population)
                heapq.heappush(pq, (current_time + travel_time, neighbor, next_quantity))
   
    return distribution

# Example usage
resource_distribution = distribute_resource(archipelago, "Niihau", "kahelelani shells", 1000)
print("Resource Distribution:", resource_distribution)

# Algorithm 2: Efficiently Sharing Knowledge
def share_knowledge(archipelago, start_island):
    pq = [(0, start_island)]
    visited = set()
    knowledge_path = []

    while pq:
        current_time, current_island = heapq.heappop(pq)
       
        if current_island in visited:
            continue

        visited.add(current_island)
        knowledge_path.append(current_island)
       
        for neighbor, travel_time in archipelago.islands[current_island].neighbors.items():
            if neighbor not in visited:
                heapq.heappush(pq, (current_time + travel_time, neighbor))

    return knowledge_path

# Example usage
knowledge_path = share_knowledge(archipelago, "Hawaii")
print("Knowledge Sharing Path:", knowledge_path)

# Algorithm 3: Distributing and Planting Resources with Multiple Trips
def distribute_and_plant_resource(archipelago, start_island, resource):
    q = deque([(start_island, 0)])
    visited = set()
    distribution_plan = []

    while q:
        current_island, current_time = q.popleft()
       
        if current_island in visited:
            continue

        visited.add(current_island)
        distribution_plan.append((current_island, current_time))
       
        for neighbor, travel_time in archipelago.islands[current_island].neighbors.items():
            if neighbor not in visited:
                q.append((neighbor, current_time + travel_time))

    return distribution_plan

# Example usage
planting_plan = distribute_and_plant_resource(archipelago, "Hawaii", "uala")
print("Planting Plan:", planting_plan)

# Algorithm 4: Optimizing Tourist Routes
def optimize_tourism(archipelago, start_island):
    visited = set()
    tourism_path = []
    current_island = start_island
    current_time = 0

    while True:
        visited.add(current_island)
        tourism_path.append((current_island, current_time))
       
        next_island = None
        min_time = float('inf')
       
        for neighbor, travel_time in archipelago.islands[current_island].neighbors.items():
            if neighbor not in visited and travel_time < min_time:
                next_island = neighbor
                min_time = travel_time
       
        if next_island is None:
            break

        current_island = next_island
        current_time += min_time

    return tourism_path

# Example usage
tourism_path = optimize_tourism(archipelago, "Hawaii")
print("Tourism Path:", tourism_path)





Intro	

For this assignment, Chester and I represented the archipelago as a directed and weighted graph where each island is a node, the travel time between islands and weighted and directed edges. Through this structure, tracking travel routes and islands can be efficiently handled, and the Island class itself contains all the necessary attributes for an island. The Archipelago class expands further to managing the islands as a collection as well as the routes and resources. 

Data Structures

Priority Queue - Minimizing overall travel time is accomplished by the shortest amount of travel time for distribution of knowledge and resources, which can be handled by a Priority Queue.
Dequeue - management of both ends of the queue is useful when handling multiple trips.


Algorithms and their implementation 

Distributing Resources
A priority queue can be implemented to simulate resources being distributed that focuses on minimizing travel time while maximizing the length of destination. The structure ensures that the resource distribution always connects with the island closest in to its proximity. With this algorithm, one location is capable of distributing its produced resources to as many islands as possible. Additionally, the quantity of the resource that is distributed by a particular island to another  tracks that the capacity of the receiving island is met in an efficient manner. For example when shells are distributed from Niihau, each island receives the appropriate amount based on its population, and the rank in which islands receive are based on shorter travel times. The time complexity of this algorithm is O((N + R) log N), where N is the number of Islands and R is the number of routes.

Sharing Knowledge
By utilizing a priority queue to prioritize islands with higher populations within a closer distance, this algorithm can ensure that knowledge is shared to the islands in effective time. The priority queue will allow efficient travel by always selecting the island that is within closest proximity. For example, a leader from the Islands of Hawaii can efficiently share knowledge by traveling to the closest islands that have the highest population density. The algorithm run-time complexity is the same as distributing resources, O((N + R) log N).


Multiple Trips
With given constraints and considerations of limited canoe capacity and multiple necessary trips, this algorithm ensures resources can be transported across various islands. A deque is also incorporated which permits FIFO and LIFO operations for managing distribution. The run-time complexity of this algorithm is O(N + R) where each edge is processed once. 

Tourist Routes
This algorithm optimizes tourist routes by allowing tourists to maximize different activities within a short amount of time. By using a greedy algorithm, we ensure that tourists select the nearest island that minimizes travel time and still can pursue various activities. For example, if a tourist wanted to visit different volcano sites in Hawaii, they could travel in a close proximity sequence that maximizes the amount of volcanoes that they visit. In this algorithm, each edge is visited once resulting in a run-time complexity of O(N + R). 


Conclusion 

	The sea of islands can be accutrust modeled by a directed and weighted graph that incorporates algorithms to manage resource distribution, knowledge distribution, and tourist optimization. 
