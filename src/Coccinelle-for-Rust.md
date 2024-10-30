# Coccinelle for Rust

Coccinelle is a tool for automatic program matching and transformation that
was originally developed for making large scale changes to the Linux kernel
source code (ie, C code).  Matches and transformations are driven by
user-specific transformation rules having the form of abstracted patches,
referred to as semantic patches. As the Linux kernel, and systems software
more generally, is starting to adopt Rust, we are developing Coccinelle for
Rust, to make the power of Coccinelle available to Rust codebases.

## Examples

Changing a method call sequence in the Rust implementation:

<pre><code class="language-diff hljs"><span class="hljs-title">@@</span>
<span class="hljs-keyword">expression</span> tcx, arg;
<span class="hljs-title">@@</span>
<span class="hljs-deletion">- tcx.type_of(arg)</span>
<span class="hljs-addition">+ tcx.bound_type_of(arg).subst_identity()</span>
</code></pre>

Replace Generic Bound with Impl Trait:

<pre><code class="language-diff hljs"><span class="hljs-title">@@</span>
<span class="hljs-keyword">identifier</span> f, P, p;
<span class="hljs-keyword">type</span> T1, T2;
<span class="hljs-title">@@</span>
<span class="hljs-deletion">- f&lt;P: T1&gt;(p: P) -&gt; T2</span>
<span class="hljs-addition">+ f(p: impl T1) -&gt; T2</span>
     { ... }
</code></pre>

## Current status

Coccinelle for Rust is currently a prototype.  It relies on Rust Analyzer
for parsing and rustfmt for pretty printing.  It mainly supports matching
and transformation of expressions and types, but reasoning about control
flow is not yet supported.

## Availability

[Coccinelle for Rust](https://gitlab.inria.fr/coccinelle/coccinelleforrust.git)

[LWN article from Kangregos 2024](https://lwn.net/Articles/991399/)

[A recent talk about Coccinelle for Rust](https://gitlab.inria.fr/coccinelle/coccinelleforrust/-/raw/ctl2/talks/lpc24.pdf?ref_type=heads)

## Feedback

Coccinelle for Rust relies on user feedback for its improvement.
Several bugs have been fixed thanks to helpful feedback from users. 
If you are interested in using Coccinelle for Rust please feel free to
reach out to us at [Contact](#Contact) with your questions or feedback.


## Support

We would like to thank [Collabora](https://www.collabora.com) for supporting
the development of Coccinelle for Rust.

## Contact

- Julia Lawall: [Julia.Lawall@inria.fr](mailto:Julia.Lawall@inria.fr)
- Tathagata Roy: [tathagata.roy1278@gmail.com](mailto:tathagata.roy1278@gmail.com)
