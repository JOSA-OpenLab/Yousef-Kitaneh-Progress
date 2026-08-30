### week07: Reflection

_I forgot to write the reflection ;p_

#### Dependabot

I followed the
[quick start guide](https://docs.github.com/en/code-security/tutorials/secure-your-dependencies/dependabot-quickstart)
for Dependabot and enabled it for LeftHookRoll (this project's infra is really
beefy now after all those tasks), the project being a C++ one, the only
releveant dependabot configuration is the one for
[CI/CD workflows](https://github.com/Andary22/LeftHookRoll/blob/main/.github/dependabot.yml).
I also enabled security updates and dependabot alerts.

#### Cosign Release

This was a trivial task, the only thing that almost went wrong was me almost
pushing the private cosign key..
https://github.com/Andary22/LeftHookRoll/releases

#### SBOM

I then ran grype and syft'ed the resultant [sbom](sbom.json), the
[resultant CVE report](grype.txt) noted a few CVEs, most notably
[CVE-2024-6345](https://nvd.nist.gov/vuln/detail/cve-2024-6345) which is a
remote code execution in the `pypa/setuptools` pip dependancy, it is a result of
improper input sanitization, and so in contexts where the functions isn't
exposed to user input, is not exploitable. It can be fixed however by ensuring
the `setuptools` version is >= 70.0.0.

#### OpenSSF Scorecard

Finally, I ran the Openssf scorecard (via the docker version, sorry go fans) I
tackled the "excessive token permissions" issue by ensuring that my workflows
followed
[the principle of least privilege](https://en.wikipedia.org/wiki/Principle_of_least_privilege)(fancy
talk for just setting permissions to read only unless write is needed). I also
read that by
[enabling branch rules](https://github.com/Andary22/LeftHookRoll/rules/13299403?ref=refs%2Fheads%2Fmain),
the score should go up(to 6 points), but that wasn't the case for me, I'll need to
investigate further.

For reference, here are the [before](ossf_scorecard.txt) and
[after](ossf_scorecard2.txt) scorecards.
