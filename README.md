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
![graphviz (21)](https://user-images.githubusercontent.com/1582097/198413424-36553430-3286-4ac3-8163-a4198ddb2f48.svg)



