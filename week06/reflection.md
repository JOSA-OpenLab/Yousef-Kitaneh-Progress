### Week06: Reflection

Before I talk about the week-specific tasks, I want to mention that the
[rust-analyzer PR](https://github.com/rust-lang/rust-analyzer/pull/22433) I made
a few weeks back got merged at last (= and
[another PR](https://github.com/rust-lang/compiler-builtins/pull/1225) I made
last week that got really thoroughly reviewed (and should be merged soon) was a
great learning experience.

#### Docs PR

In MDN, I found a weekly typo-tracker (I assume they employ a prose linter much
like vale) which contained several keywords that were marked as errors when they
were valid, [my PR#44653](https://github.com/mdn/content/pull/44653) adds them
to the dictionary to help clear noise and reduce confusion. I want to point out
however a different kind of
[docs PR#1058](https://github.com/knurling-rs/defmt/pull/1058) I made a while
back while reading confusing documentation for the `defmt` crate. The docs were
flat-out wrong about a certain feature, and I made a PR to fix it, I find that
the best kind of doc PRs (or PRs in general) are the ones that come organically
rather than hunting for them.

#### Setting up a Docs website

I found this quote from the diataxis framework a really nice hueristic to turn
to when deciding where to write new documentation.

> To use the compass, just two questions need to be asked: action or cognition?
> acquisition or application?

Anyways, I set up
[mkdocs for lefthookroll](https://andary22.github.io/LeftHookRoll/), the same
project I set up a CI on last week, I have also previously set up an
[API reference via doxygen](https://andary22.github.io/LeftHookRoll/api/index.html),
so this was a nice integration opportunity. I then retroactively added an ADR
which me and my teammates discussed a while back (but had no clue ADRs were a
thing), and then setup vale on the CI on the docs.

> ...Run it in CI. The first time you do, your team will hate you. The second
> time, your docs will be measurably better.

Yeah. I can confirm that I indeed hated vale the first time around, but after
tuning the dictionary and rules, and a lot of fixes, I can see the value it adds
for mature projects and maintaining a consistent style.
