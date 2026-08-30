### Reflection

This week was full of deadlines and finals, but here-goes a summary of the work
I did in the open source sphere this week:

#### [PR Review A: clap-rs (#6364)](https://github.com/clap-rs/clap/pull/6364)

_(Note:This is unrelated to learning objectives, but reviewing this kept popping
[Claptrap](https://borderlands.fandom.com/wiki/Claptrap) into my head. it was
slightly annoying.)_

before I summarize my review, some context that is nice to have:

- Completion scripts hardcode desired options, are sourced by the shell, and
  trigger via `<TAB>` to generate completions, these scripts are largely
  boilerplate and adopt a standard approach and are stored in
  `/usr/share/bash-completion/completions/`.
- `COMP_WORDBREAKS` is a global internal bash variable dictating split
  characters. It breaks completions for arguments containing characters like `:`
  or `=`. (This issue was a nice reminder of why globals are a bad idea)

So, the PR I reviewed tried to fix this issue by working around
`COMP_WORDBREAKS` entirely, while `clap-rs` internally doesnt use
`COMP_WORDBREAKS`, the issue is that bash invokes splitting via the seperator
set when it performs operations such as expansion, so we had to somehow obtain
the original argument before bash splits it.

the author of the PR used `eval` to bypass wordbreaks, which is a clever
solution, but `eval` is actually a 2-phase parser, and while in the first phase
it successfully bypasses wordbreaks, in the second phase it simply executes the
command, which while I wouldn't go as far as to call it an arbitrary code
execution CVE, is still not the correct behavior, imagine you wrote a clever
bash script that auto formats and tests your commits before push, where it took
arguments like this `script ["git action to perform"] ["tests to run"]`, and you
went ahead and typed `script "git push" <TAB>` hoping to get autocompletion for
the tests, but it just pushed instead.

> it turns out that eval is notorious for being a security risk here is an
> entire
> [SO thread dedicated to this topic](https://stackoverflow.com/questions/17529220/why-should-eval-be-avoided-in-bash-and-what-should-i-use-instead)

I pointed this out and the author was very receptive to the feedback, he fixed
it and requested another review, I found another blocking issue that broke
backwards compatibility, gave him a suggestion to fix it, and he implemented it.

PR author turned out to be a nice guy.

#### [PR Review B: rust-lang (#158154)](https://github.com/rust-lang/rust/pull/158154)

This PR took me right back into my numerical methods class. do you remember
[euclidean division](https://en.wikipedia.org/wiki/Euclidean_division)? I
didn't. put shortly, it's that thing they taught us before we learned about
fractions, its that long division that gives you a result and a remainder, the
key part to internalize is that the result is an integer, and the remainder is
positive.

leading us to [Issue #107904](https://github.com/rust-lang/rust/issues/107904),
which details that due to truncation errors, _sometimes_ the quotient is off by
+-1.

I stumbled upon the PR, I was skeptical of the approach at first, and thought
that it was needlessly heavy for an elementary operation, but after reading the
[paper linked in the original issue](https://hal.science/inria-00000159/), and
asking a clarifying question, I understood that the approach is actually quite
elegant, and the performance hit is justified by the correctness guarantee it
provides. we dont want another
[Ariane 5](https://en.wikipedia.org/wiki/Ariane_flight_V88) on our hands after
all.

TL;DR of the approach: perform
[Fused Multiply-Add (AKA A + B*C)](https://en.wikipedia.org/wiki/Multiply%E2%80%93accumulate_operation)
of the quotient, dividend, and divisor to compute the error of the quotient (if
it would get overestimated or underestimated), which also has the added benefit
of being a hardware accelerated operation, we then detect the
geometry/sign/direction of the error to determine if we need to compensate the
quotient by +1 or -1.

#### [Self-Review: siege (#261)](https://github.com/JoeDog/siege/pull/261)

Ironically, A PR of mine that I am most critical of is the one I used to apply
for OpenLab ;p

I took another cringful look at the PR, and here is what I have to say about
what I would've done differently now:

- I was too concerned with whether the _how_ of what I fixed was correct that I
  pretty much neglected the _what_ and _why_ of what I fixed. I would put more
  emphasis on why this fix is needed and how to reproduce the issue.
- when providing logs, I provided them as images, I would provide them as simple
  text instead. this is especially painful to remember now that I know that the
  maintainer of siege uses a mailing list to review PRs and not the GitHub web
  UI like I do...
- This PR is also a bit of overengineering, I did indeed fix the race condition
  that was causing the crash, but for some reason I also generalized the pthread
  function calls (I changed them from taking a concrete type to a void pointer),
  which in retrospect is pretty much the opposite of the YAGNI principle.
  **nothing** was being broken by the existing code, and the generalization
  added a lot of noise to the PR while adding no value, luckily the PR was still
  accepted, but I would've definitely not modify the function prototypes if I
  were to do this PR today.

To conclude, the PR ended up being a net positive and (hopefully) not breaking
anything, but it could've been much easier on me and the maintainer.

#### Real World Code Review Process: rust compiler builtins

I reviewed the review culture in
[rust-lang/compiler-builtins](https://github.com/rust-lang/compiler-builtins) by
analyzing three PRs:

- [Merged (#1213)](https://github.com/rust-lang/compiler-builtins/pull/1213)
- [Changes Requested (#812)](https://github.com/rust-lang/compiler-builtins/pull/812)
- [Closed/Alternative Approach (#1077)](https://github.com/rust-lang/compiler-builtins/pull/1077)

**Takeaways:** * Observed the deployment process for subtree updates (embedding
a repo and its history).

- Noted strict maintainer pushback against over-engineering in #812.
- Saw highly effective use of git infra bots for managing conflicts, PR states,
  and assignations.
- In #1077, despite six participants and complex technical disagreements, the
  thread reached a definitive and professional conclusion.

#### [Input-leap Issue #2390](https://github.com/input-leap/input-leap/issues/2390)

_(Not a weekly objective, but might be relevant context for upcoming work)._ I
isolated a persistent bug I suffered from for months. No maintainer interaction
yet, but I have drafted technical approaches and will work on a PR in the coming
weeks.
