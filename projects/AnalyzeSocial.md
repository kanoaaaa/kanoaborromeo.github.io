---
layout: project
type: project
image: img/photo-1683721003111-070bcc053d8b copy.jpeg
title: "AnalyzeSocial"
date: 2024
published: true
labels:
  - Python
summary: "A partner and myself collaborated on a class assignment to analyze social networks by using graph theory to analyze trending content and post references with algorithms and data structures like networkx and word clouds."
---

![Social Media](../img/SocialMedia.jpeg)


import networkx as nx
import matplotlib.pyplot as plt
from wordcloud import WordCloud
from datetime import datetime
from typing import List, Dict

# Data Structures
class User:
    def __init__(self, username: str, real_name: str, attributes: Dict[str, str] = None):
        self.username = username
        self.real_name = real_name
        self.attributes = attributes or {}
        self.connections = {}  # Dictionary to store connections by type
        self.posts = []  # List of Post objects
        self.read_posts = []  # List of Post objects
        self.comments = []  # List of Comment objects

    # Consider adding methods to manage user connections, posts, and comments

class Post:
    def __init__(self, post_id: str, content: str, creation_time: datetime):
        self.post_id = post_id
        self.content = content
        self.creation_time = creation_time
        self.comments = []  # List of Comment objects
        self.views = []  # List of tuples (User, view_time)
        self.references = []  # List of Post objects that this post references

    # Consider adding methods to manage post comments, views, and references

class Comment:
    def __init__(self, comment_id: str, content: str, creation_time: datetime, author: User):
        self.comment_id = comment_id
        self.content = content
        self.creation_time = creation_time
        self.author = author

    # Consider adding methods to manage comment replies and likes

# Functions for Analysis

def create_social_network_graph(users: List[User]) -> nx.Graph:
    """
    Creates a social network graph from a list of users.
   
    Args:
    users (List[User]): A list of User objects.
   
    Returns:
    nx.Graph: A social network graph.
    """
    G = nx.Graph()
   
    # Add nodes for users and posts
    for user in users:
        G.add_node(user.username, type='user')
        for post in user.posts:
            G.add_node(post.post_id, type='post', content=post.content)
            G.add_edge(user.username, post.post_id, type='authored')
        for viewed_post in user.read_posts:
            G.add_edge(user.username, viewed_post.post_id, type='viewed')
   
    # Add edges between posts and comments
    for user in users:
        for post in user.posts:
            for comment in post.comments:
                G.add_node(comment.comment_id, type='comment', content=comment.content)
                G.add_edge(post.post_id, comment.comment_id, type='comment')
   
    return G

def draw_network_graph(G: nx.Graph, highlight_criteria: str = 'comments') -> None:
    """
    Draws a social network graph.
   
    Args:
    G (nx.Graph): A social network graph.
    highlight_criteria (str): The criteria to highlight in the graph (default: 'comments').
    """
    pos = nx.spring_layout(G)
    node_colors = ['skyblue' if G.nodes[n]['type'] == 'user' else 'lightgreen' for n in G.nodes]
   
    nx.draw(G, pos, with_labels=True, node_color=node_colors, node_size=50, font_size=8, edge_color='gray')
    plt.title(f"Social Network Graph ({highlight_criteria.capitalize()})")
    plt.show()

def generate_word_cloud(posts: List[Post], filters: List[str] = None) -> None:
    """
    Generates a word cloud from a list of posts.
   
    Args:
    posts (List[Post]): A list of Post objects.
    filters (List[str]): A list of keywords to filter the posts (default: None).
    """
    text = ''
    for post in posts:
        if filters:
            if any(keyword in post.content for keyword in filters):
                text += post.content + ' '
        else:
            text += post.content + ' '
   
    wordcloud = WordCloud(width=800, height=400, background_color='white').generate(text)
   
    plt.figure(figsize=(10, 5))
    plt.imshow(wordcloud, interpolation='bilinear')
    plt.axis('off')
    plt.show()

def trending_posts(posts: List[Post]) -> List[Post]:
    """
    Returns the top 10 trending posts.
   
    Args:
    posts (List[Post]): A list of Post objects.
   
    Returns:
    List[Post]: The top 10 trending posts.
    """
    sorted_posts = sorted(posts, key=lambda p: len(p.views), reverse=True)
    return sorted_posts[:10]

def print_trending_posts(posts: List[Post]) -> None:
    """
    Prints the top 10 trending posts.
   
    Args:
    posts (List[Post]): A list of Post objects.
    """
    trending = trending_posts(posts)
    for post in trending:
        print(f"Post ID: {post.post_id}, Views: {len(post.views)}, Content: {post.content[:100]}")

def create_social_network_graph_with_references(users: List[User]) -> nx.Graph:
    """
    Creates a social network graph with references from a list of users.
   
    Args:
    users (List[User]): A list of User objects.
   
    Returns:
    nx.Graph: A social network graph with references.
    """
    G = create_social_network_graph(users)
   
    # Add edges for post references
    for user in users:
        for post in user.posts:
            if hasattr(post, 'references'):
                for ref_post in post.references:
                    G.add_edge(post.post_id, ref_post.post_id, type='reference')
   
    return G

# Example Usage

# Create some sample data
user1 = User(username="alice", real_name="Alice Smith")
user2 = User(username="bob", real_name="Bob Johnson")

post1 = Post(post_id="p1", content="This is the first post.", creation_time=datetime(2024, 8, 1))
post2 = Post(post_id="p2", content="This is another post.", creation_time=datetime(2024, 8, 2))
post1.references = [post2]

comment1 = Comment(comment_id="c1", content="Great post!", creation_time=datetime(2024, 8, 1), author=user2)
post1.comments.append(comment1)

user1.posts.append(post1)
user2.read_posts.append(post1)
user1.read_posts.append(post2)

# Create and visualize the social network graph
G = create_social_network_graph([user1, user2])
draw_network_graph(G)

# Generate a word cloud
generate_word_cloud([post1, post2])

# Print trending posts
print_trending_posts([post1, post2])

# Create and visualize graph with references
G_ref = create_social_network_graph_with_references([user1, user2])
draw_network_graph(G_ref, highlight_criteria='references')

# Consider adding more functionality, such as:
# - User authentication and authorization
# - Post and comment moderation
# - User relationships (e.g. friendships, followers)
# - Post and comment likes and dislikes
# - Search functionality for posts and comments
# - User profiles and statistics




(Intro)
	The modern age lives in the digital world. Social media platforms integrating in daily life are apparent through how interactions are conducted and information is spread. The following tools and methods are valuable for analyzing social media data and focusing on areas such as content clusters and trends within networks. The code uses ‘networkx’  for graph representation, ‘matplotlib’ for visualization, and other data manipulation functions necessary for analysis. 

(Data Structures)

Classes
The classes implemented ensure structured data stores while providing efficient access and manipulation for different variables to be analyzed.

User Class - Each user object contains variables such as posts read, comments made, and other personal information. This allows easy management of relationships involving user-related data.

Post Class - Each post object contains variables such as the creation time, the content of the post, as well as the comments and views for the post. References are also included in order to draw links within the network.

	
Comment Class - Each comment is linked back to its authors and maintains creation time to support a hierarchical discussion structure.

Graphs 
	In order to represent the social network, networkx.Graph is implemented, where nodes represent the users, posts, and comments, while edges are relationships such as viewing and commenting.

Algorithms
Graphing Social Networks
By pulling from user data, the create_social_network_graph function generates a graph that iterates over users and their posts and adds nodes and edges to represent variables like views. Additional nodes are also integrated to represent comments. This will provide a well rounded structure that represents important relevant information.



Graphs Visualization 
Trending content within the network can be identified in visualization, where the draw_network_graph function generates a social network graph that highlights nodes based on certain features, such as comments. This can help identify trending content within the network.

Keywords
Trending topics and other relevant content can all be discerned by using the generate_word_cloud tool which will aid in filtering content based on keywords. This is accomplished through a visual to depict frequently used words that are associated with posts.

Trending Posts 
Identifying the top 10 trending posts is easily accomplished by the trending_posts function which provides an algorithm to rank posts based on viewership. This algorithm is important for spotting content gaining attention

Content References
Capturing the linkage between differing content is accomplished by including references between posts in the create_social_network_graph_with_references function.

Rationale for Choices


Algorithm efficiency
Efficient data structures are achieved through ‘netowrkx’, allowing for graph representation and manipulation, which is essential for large-scale analysis for social networks. The incorporation of nodes and edges greatly represent the inherent interaction in social media.

Flexibility
Organizing posts, users, and comments into separate classes allows easy addition of new attributes for managing interactions within the network.

Real-world applicability
With this design, large datasets can be handled by employing structures and algorithms that maintain performance for ranging data scales.

Conclusion
The implementation of this code presents an approach to analyzing social media networks while providing algorithmic efficiency and flexibility. Incorporating graph theory and data visualization provides a framework for dissecting intricate interaction within social media platforms. Future models can build upon this foundation to evaluate key analytics and support advanced analyses


	


