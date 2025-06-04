# Modifications Applied from Fork

This document summarizes the modifications applied from your fork to the edamametechnologies/extended-isolation-forest repository.

## 1. Recursion Cap Functionality

Added recursion cap to prevent stack overflow when scoring deeply nested trees:

- Modified `Forest::score()` to use a default recursion cap (2x the average tree depth)
- Added `Forest::score_with_recursion_cap()` method that accepts a custom maximum recursion depth
- Updated `Tree::path_length_with_cap()` to track and limit recursion depth
- Modified `path_length_recurse()` to accept depth parameters and return early when cap is reached

This prevents potential stack overflow issues when dealing with pathological data that creates very deep trees.

## 2. Optimized Sparse Normal Vector Generation

Improved performance for high-dimensional data by only generating non-zero values for active dimensions:

- Changed from generating N random normal values and then zeroing out some
- Now only generates `extension_level + 1` random values for the active dimensions
- Significantly reduces computational cost for high-dimensional datasets

## Key Benefits

1. **Stability**: The recursion cap prevents stack overflow errors
2. **Performance**: Sparse vector generation is much faster for high-dimensional data
3. **Compatibility**: All existing tests continue to pass

## Usage Example

```rust
// Using default recursion cap
let score = forest.score(&features);

// Using custom recursion cap for safety
let score = forest.score_with_recursion_cap(&features, 12);
``` 