# 1.3. Todo

<primary-label ref="header-label"/>

<secondary-label ref="doc-wip"/>

## Functions {collapsible="true"}

- [ ] **Tuple function folding**: Calling a function with a tuple as an argument that is a single argument of the tuple
  element type, causing the function to be called multiple times with each element of the tuple. For
  example `vector.emplace_head((1, 2, 3, 4))..` would call `emplace_head` 4 times with the arguments `1`, `2`, `3`,
  and `4`.
- [ ] ****
