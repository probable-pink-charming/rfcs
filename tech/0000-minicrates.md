- Feature Name: **"Dynamic minicrates" infrastructure for Flara story modules*
- Start Date: 2023-03-09
- RFC PR: [project-flara/flarastory-open-rfcs#0000](https://github.com/project-flara/flarastory-open-rfcs/pulls/0000)

# Summary
[summary]: #summary

A minicrate is a module that is not statically linked to the parent crate, but rather built individually as a dynamic library that can be later loaded by the parent crate through the `dlopen` crate.

# Motivation
[motivation]: #motivation

A commercial narrative-based gacha video games use a LOT of dialog files. I'm not sure how many we'll use once Project Flara can release its own indie titles, but I want them to be quite long I guess.

Project Sekai's Japanese release for instance has 87 events. Each events usually have 8-10 episodes inside. There are also the group stories.
There's a reason why they download dialog assets on demand, if I am correct.

It wouldn't make sense to run `cargo new` and add dependencies to all of them. Seems too much boilerplate work.
It doesn't make sense to include them in the main binary either.

Minicrates enable better developer experience and productivity, by not having to do that.
# Guide-level explanation
[guide-level-explanation]: #guide-level-explanation
Hii! Do you want to create a minicrate? It's quite easy.
Just create a new file with the `.mini.rs` extension.

```toml
[flara.minicrates "**/*"] # accept any kind of glob expressions
framework = { path = "./framework" } # path relative to the parent's Cargo.toml manifest
parent = {} # you can depend on the parent crate too
```

Flara will not force you into making the parent crate to be a `dynlib`, but it's maybe good to load them, and many other crates, as a dynamic library instead of statically link them into the minicrate.

## `cfg!()` directives
We can have a cfg directive too! If we're inside  minicrate, cfg!(minicrate) should be true. **However, I haven't been able to find any use cases for this.**
So they don't have to be implemented for now.

# Reference-level explanation
[reference-level-explanation]: #reference-level-explanation

Flara minicrates work by generating "output crates" that can later be loaded by the parent crate trough dlopen. 
It works by 
# Drawbacks
[drawbacks]: #drawbacks

> Using this macro is often a bad idea, because if the file is parsed as an expression, it is going to be placed in the surrounding code unhygienically. This could result in variables or functions being different from what the file expected if there are variables or functions that have the same name in the current file.

# Rationale and alternatives
[rationale-and-alternatives]: #rationale-and-alternatives

- Why is this design the best in the space of possible designs?
- What other designs have been considered and what is the rationale for not choosing them?
- What is the impact of not doing this?
- If this is a language proposal, could this be done in a library or macro instead? Does the proposed change make Rust code easier or harder to read, understand, and maintain?

# Prior art
[prior-art]: #prior-art

Discuss prior art, both the good and the bad, in relation to this proposal.
A few examples of what this can include are:

- For language, library, cargo, tools, and compiler proposals: Does this feature exist in other programming languages and what experience have their community had?
- For community proposals: Is this done by some other community and what were their experiences with it?
- For other teams: What lessons can we learn from what other communities have done here?
- Papers: Are there any published papers or great posts that discuss this? If you have some relevant papers to refer to, this can serve as a more detailed theoretical background.

This section is intended to encourage you as an author to think about the lessons from other languages, provide readers of your RFC with a fuller picture.
If there is no prior art, that is fine - your ideas are interesting to us whether they are brand new or if it is an adaptation from other languages.

Note that while precedent set by other languages is some motivation, it does not on its own motivate an RFC.
Please also take into consideration that rust sometimes intentionally diverges from common language features.

# Unresolved questions
[unresolved-questions]: #unresolved-questions

- What parts of the design do you expect to resolve through the RFC process before this gets merged?
- What parts of the design do you expect to resolve through the implementation of this feature before stabilization?
- What related issues do you consider out of scope for this RFC that could be addressed in the future independently of the solution that comes out of this RFC?

# Future possibilities
[future-possibilities]: #future-possibilities

Think about what the natural extension and evolution of your proposal would
be and how it would affect the language and project as a whole in a holistic
way. Try to use this section as a tool to more fully consider all possible
interactions with the project and language in your proposal.
Also consider how this all fits into the roadmap for the project
and of the relevant sub-team.

This is also a good place to "dump ideas", if they are out of scope for the
RFC you are writing but otherwise related.

If you have tried and cannot think of any future possibilities,
you may simply state that you cannot think of anything.

Note that having something written down in the future-possibilities section
is not a reason to accept the current or a future RFC; such notes should be
in the section on motivation or rationale in this or subsequent RFCs.
The section merely provides additional information.