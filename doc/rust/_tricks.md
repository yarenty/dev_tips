# println! speedup 500%
tczajka

Here is a way to make the code 5x faster -- replace this line:

println!("{} {} {} {} {}", x1, x2, x3, x4, x5);

with:

println!("{} {} {} {} {}", {x1}, {x2}, {x3}, {x4}, {x5});

The compiler is worried that the variables may get changed through immutable references passed to Display implementations, making the f calls not invariant across loop iterations. That is unfortunate, given that immutability is such a core language feature.