### Reflection
This week's tasks consisted of 2 PR's and a code review. here is what I did for each task with the most notable things I learned from each task.

#### [PR 1: fix(book): fix various typos, a syntax error, and grammatical errors.](https://github.com/knurling-rs/defmt/pull/1058)
I opened a PR in [knurling-rs/defmt](https://github.com/knurling-rs/defmt), deferred formatting for logging on embedded systems. I was working on a project that used it and while reading through the documentation I noticed a couple errors and decided to leave my mark on the rust embedded ecosystem by fixing them.

#### PR 2: [feat(ide-diagnostics): add diagnostics for invalid union patterns (E0784) ](https://github.com/rust-lang/rust-analyzer/pull/22433)

This was the most difficult task of the week, which was mostly due to me being overly ambitious, I located a pinned issue in [rust-lang/rust-analyzer](https://github.com/rust-lang/rust-analyzer) that detailed steps for adding diagnostics and replacing FIXME comments in the codebase. I tried to solve 3 FIXMEs that I thought were related to the same issue, and endded up only solving 2 because the 3rd one would be out of scope for this PR, and I had to revert the changes I made to solve the 3rd one, which was a bit of a mess.

I then proceded to tweak the code until it passed the test suite on my machine, which also took some time, but at least the CI on the PR passed the first time around.

#### Code Review: Testing and verifying correctnes of a [feature PR in Syncthing](https://github.com/syncthing/syncthing/pull/10335)
This PR was one that introduced a feature I used myself, and it was one I actually reviewed before to help getting it approved, but this week's task motivated me to revisit my review and as I was retesting I noticed some abnormal behavior, so I decided to notify the authors and I am still investigating the issue's cause.