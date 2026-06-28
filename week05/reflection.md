### Week05: Reflection

#### Unit Tests PR

Inspired by last week's euclidean division PR review, I decided to check out the
rust compiler builtins, look for untested code branches and add coverage for
them. I added
[this PR](https://github.com/rust-lang/compiler-builtins/pull/1225) to do that.
I used llvm-cov to find untested branches, approach and reasoning explained in
the PR.

#### Test Workflow & `act`

I worked on [LeftHookRoll](https://github.com/Andary22/LeftHookRoll) alongside 2
other developers, each of us on a different operating system (debian/fedora) and
while I enforced a testing practice, it was a manual process built on
responsibility and trust. It never occured to me to automate this process, I
even already set up a CD workflow for the documentation of this project
([check it out here](https://andary22.github.io/LeftHookRoll/) ;p), so this was
a great opportunity to take advantage of existing tests to integrate them into a
CI workflow.

Initially I tried setting up a macOS test, and after I ran act locally it
worked, but on the GitHub actions runner, it kept failing, this was because my
local macOS runner was actually just an ubuntu runner with a macOS image, so
that was a valuable lesson.

> [!note] Aside
>
> if I were to tackle the problem of making this project cross-platform today, I
> would refer to a pre-built library like
> [libevent](https://libevent.org/index.html) built by smarter people than me
> specifically for this purpose. throughout the development process a lot of
> things were inconsistent accross platforms, and "its working on my machine"
> was a common occurance.

```bash
› ./bin/act -W '.github/workflows/ci.yml'

| ========================================
| Test Summary
| ========================================
| 
| [2026-06-27 21:42:14] Test Summary
| Total:  4
| Passed: 4
| Failed: 0
| 
| [2026-06-27 21:42:14] Summary: 4/4 tests passed
| All tests passed!
[CI/test-linux-1]   ✅  Success - Main Run Tests [16.356727626s]
[CI/test-linux-1] ⭐ Run Complete job
[CI/test-linux-1] Cleaning up container for job test-linux
[CI/test-linux-1]   ✅  Success - Complete job
[CI/test-linux-1] 🏁  Job succeeded
```

If this project was still ongoing, I would definitely seperate the 4 tests into
separate jobs, but this is just a proof of concept for now.
