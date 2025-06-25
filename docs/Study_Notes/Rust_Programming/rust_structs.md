---
id: rust
title: Rust Structs
created: 2025-06-26
---


```table-of-contents
```

# Struct Methods

- First parameter always has to be `self` to be used as a method for the struct. Placing it in the `impl` block makes the method in context of the Struct defined in `impl`
```rust
#[derive(Debug)]
struct Rectangle {
    width: u32,
    height: u32,
}

impl Rectangle {
    fn area(&self) -> u32 {
        self.width * self.height
    }
}

fn main() {
    let rect1 = Rectangle {
        width: 30,
        height: 50,
    };

    println!(
        "The area of the rectangle is {} square pixels.",
        rect1.area()
    );
}
```
