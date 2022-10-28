# Rel Regex Mirror

Implements very basic code reflection entirely in user space using regular
expressions.

The reflection done here is very poor, since it is based on quick regexes that
I put together in a couple hours, but it shows a proof of concept for
user-space code reflection, by reflecting over the catalog's model relation.

If the data came from the compiler itself, this package would be trivial.

## Examples:

```rel
// Show a graph of the connections between definitions
def output = graphviz[connections]

// Show a graph of all connections between definitions, including the standard libs
def output = graphviz[all_connections]
```

```rel
// Graph the connections between file "a.rel" and "b.rel"
def ab = :edge, {
    d.name, d.reference.name from d
    where d.file = "a.rel"
    and   d.reference.file = "b.rel"
}
def output = graphviz[ab]
```

For example, here is the output from running `graphviz[connections]` on this package itself:
```rel
graphviz[connections]
```
![graphviz.svg](https://user-images.githubusercontent.com/1582097/198490818-8d295124-c4d5-49f5-8f88-e13d1c64eaa2.svg)


And in this (slightly more complex) example, we restrict the graph to just the edges reachable from `end_offset`:
```rel
def start = d in Definition : d.name = "end_offset"
def from_end_offset = union[
    start,
    reachable[non_stdlib_ref][start]
]

def edge1 = {
    d.name, d2.name
    from d in from_end_offset, d2 in d.non_stdlib_ref
}
def output = graphviz[{:edge, edge1}]
```
![subset_graphviz.svg](https://user-images.githubusercontent.com/1582097/198483505-e3a16e4c-ff57-49b5-aafc-dbe117aba191.svg)
