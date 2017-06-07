# ast-loc-utils

A lightweight, cherry-pickable bundle of tools for manipulating source location information (`loc`, `start`, and `end`) in Babel AST nodes.

## Notes

### `/lib` cherry-picking convention

Avoid importing a monolithic bundle by cherry-picking exactly what you need. Individual methods are cherry-pickable with `import someMethod from 'ast-loc-utils/lib/someMethod'`.

### Location mixins

Some `ast-loc-utils` functions produce or consume a `LocationMixin`, which is a JS object that carries full location information for a Babel node and can be `Object.assign()`ed onto a Babel node to change its location.

## Functions

### getLoc
```js
getLoc(node: BabelNode) -> LocationMixin
```
Gets the location mixin representing the location of the given node.

### getSurroundingLoc
```js
getSurroundingLoc(nodes: Array<BabelNode>) -> LocationMixin
```
Gets a location mixin representing a source location that surrounds the total extent of all the input nodes.

### buildAtLoc
```js
buildAtLoc(loc: LocationMixin, builder: function, ...args: any) -> BabelNode
```
Construct a node by calling the given `babel-types` builder function with the given `args`, then place it at the given location.

### placeAtLoc
```js
placeAtLoc(node: BabelNode, loc: LocationMixin) -> BabelNode
```
Replaces the location information on the given node. Returns the node. Equivalent to `Object.assign(node, loc)`.

### placeAtNode
```js
placeAtNode(targetNode: BabelNode, sourceNode: BabelNode) -> BabelNode
```
Copies source location information from the `sourceNode` to the `targetNode`, returning the `targetNode`. Equivalent to `Object.assign(targetNode, getLocation(sourceNode))`.

### placeAroundNodes
```js
placeAroundNodes(targetNode: BabelNode, ...sourceNodes: BabelNode) -> BabelNode
```
Assigns a source location to the `targetNode` that surrounds the total extent of all the `sourceNode`s and returns the `targetNode`. Equivalent to `Object.assign(targetNode, getSurroundingLocation(sourceNodes))`.

### span
```js
span(loc: LocationMixin, n: Integer) -> LocationMixin
```

Create a `LocationMixin` obtained by extracting the first or last `n` characters of the given `LocationMixin`. If `n` is positive, the first `n` characters are extracted; if negative, the last `n` are extracted.

>Note that this operation has no insight into the token stream or newline boundaries. When extracting a span, you must be sure that all characters in your span are on the same source line. If the span would cross a line boundary, the resulting output will be incorrect.
