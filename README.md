# directed_graph

The directed_graph gem is useful for modeling directed, acyclic, unweighted graphs.  It provides methods to topologically sort graphs, find the shortest path between two vertices, and render the graphs and json for client-side graphing.

The following graph will be used to demonstrate the functionality of the directed_graph gem.

![graph_example](https://github.com/MrPowers/directed_graph/blob/master/example/simple_directed_graph.png)

The `Graph` class should be instantiated with an array of edges:

```ruby
# create the vertices
@root = Vertex.new(name: "root")
@a = Vertex.new(name: "a")
@b = Vertex.new(name: "b")
@c = Vertex.new(name: "c")
@d = Vertex.new(name: "d")
@e = Vertex.new(name: "e")

# The first argument is the origin_vertex
# and the second is the destination_vertex
@ra = Edge.new(origin_vertex: @root, destination_vertex: @a)
@ab = Edge.new(origin_vertex: @a, destination_vertex: @b)
@bc = Edge.new(origin_vertex: @b, destination_vertex: @c)
@bd = Edge.new(origin_vertex: @b, destination_vertex: @d)
@ae = Edge.new(origin_vertex: @a, destination_vertex: @e)
@de = Edge.new(origin_vertex: @d, destination_vertex: @e)
@edges = [@ra, @ab, @bc, @bd, @ae, @de]
@graph = Graph.new(@edges)
```

The `@graph` object can be used to get a topological sorted array of the vertices:

```ruby
@graph.sorted_vertices # [@root, @a, @b, @d, @e, @c]
```

Here is an [awesome blog post](https://endofline.wordpress.com/2010/12/22/ruby-standard-library-tsort/) on topological sorting in Ruby with the `TSort` module.

The `@graph` object can also be used to calculate the shortest path between two vertices:

```ruby
@graph.shortest_path(@root, @e) # [@root, @a, @e]
@graph.shortest_path(@root, Vertex.new(name: "blah")) # nil because the "blah" vertex doesn't exist
@graph.shortest_path(@d, @a) # nil because the graph is directed and can't be traversed in the wrong direction
```

The `@graph` object can be used to calculate the longest path between two vertices:

```ruby
@graph.longest_path(@root, @e) # [@root, @a, @b, @d, @e]
@graph.longest_path(@a, @c) # [@a, @b, @c]
@graph.longest_path(@d, @a) # returns [] when the path doesn't exist
```

The `@graph` object can be used to generate a graphml file

```ruby
require 'color-generator'

generator = ColorGenerator.new saturation: 0.3, lightness: 0.75

# Set the color and label of each vertex
#
@graph.vertices.each do |vertex|
  color = generator.create_hex
  label = vertex.name

  vertex.data[:graphics]  = {:fill=>["##{color}"], :label=>[label], :group=>color}
end

# Generate a graphml file
# Import this file into program like yEd to size nodes and layout graph
File.open("output.graphml", "w+") { |f| f.write(@graph.to_graphml())}
```


## Installation

Add this line to your application's Gemfile:

```ruby
gem 'directed_graph'
```

Require the gem in your code:

```ruby
require 'directed_graph'
```

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/MrPowers/directed_graph.


## License

The gem is available as open source under the terms of the [MIT License](http://opensource.org/licenses/MIT).

