### Reflection

I found that this week's was focused on doing the grunt work of open source. It
isn’t as flashy as submitting a PR, but it is at least as important for the
progress of a project. I triaged a couple of issues: I
[reproduced an existing bug](#bug-repro), discovered a new issue altogether
which I created an [MCVE](#mcve) for, and practiced
[bisecting a regression](#regression-bisect).

#### Triage

##### Bug Repro

My first task was verifying a reported bug in the Rerun SDK
([#12721](https://github.com/rerun-io/rerun/issues/12721)). The author claimed
that when starting a gRPC server with `newest_first=True`, the persistent queue
gets reversed, causing protocol-level blueprint messages to arrive out of order
for late-connecting viewers.

to exclude the author's environment as a factor, I ran their reproduction script
on a clean debian env. I also tested a synthetic variation to ensure the bug
wasn't isolated to 3D spatial blueprints. In both cases, the web viewer ignored
the blueprint layout, and the console displayed:
`Got ActivateStore message without first receiving a SetStoreInfo`.

The build system was brutal, I had to setup pixi and uv, build a python venv and
build the rust sdk, and because the bug lived inside the `re_grpc_server` crate,
I had to manually compile the Python bindings with the server feature enabled.
Waiting 24 minutes for the SDK to compile from scratch was rough, especially
when I had to recompile because I forgot to compile with the server feature the
first time. all this really made me appreciate the value of triage work.

##### MCVE

Next, I hunted for an issue that lacked a Minimal, Complete, and Verifiable
Example (MCVE). I initially found a bug report in `clap-rs`
([#5955](https://github.com/clap-rs/clap/issues/5955)) regarding shell
completions failing when an argument contained double colons (`::`). However,
the author's report was difficult to understand, filled with grammar mistakes,
and missing the required code blocks.

Instead of trying to salvage their report, I made some educated guesses about
their intent, isolated the core problem (Bash's `COMP_WORDBREAKS` variable
shearing the string at the colon), and opened a brand new, legible issue
([#6403](https://github.com/clap-rs/clap/issues/6403)). I provided a
crystal-clear MCVE: a simple main.rs with the needed steps to reproduce the
issue. I also added what I thought caused the issue.

#### Regression Bisect

For my final task, I wanted to practice `git bisect` on a real-world regression.
I searched GitHub for regression labels and found the perfect candidate in the
`pydantic` repository
([#11004](https://github.com/pydantic/pydantic/issues/11004)). The issue
reported that a specific serialization pattern using
`from __future__ import annotations` worked flawlessly in `v2.9.2` but threw a
`PydanticUserError: Model is not fully defined` in `v2.10.0`.

Because Pydantic is a Python library with zero compile time(I intentionally
seeked out a python project for this, imagine if I had to wait 24 minutes for
each bisection), it was the ultimate target for an automated bisect loop. I
modified the author's `a.py` to exit with `0` on success and `1` on failure:

```python
from pydantic import BaseModel
from b import Something

class Model(BaseModel):
    something: Something

try:
    Model(something=1)
    exit(0)
except Exception:
    exit(1)
```

To automate the search, I wrapped the setup and execution into a small bash
script:

```bash
#!/bin/bash
./.venv/bin/pip install -e . -q
./.venv/bin/python a.py
```

I kicked off the search with `git bisect start v2.10.0 v2.9.2` followed by
`git bisect run ./test_bisect.sh`. Git automatically bounced through the commit
tree, installing the package and checking my script's exit code at each step. In
7 bisections, it zeroed in on the exact commit that broke the behavior
(`c772b43edb952c5fe54bb28da5124b10d5470caf`). It was an incredibly satisfying
way to definitively pinpoint the source of a regression without manually reading
thousands of lines of diffs, and was a breath of fresh air after the rust build
system pain.
