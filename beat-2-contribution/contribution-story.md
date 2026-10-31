# Contribution: 2-bit and 4-bit /DeviceGray images are extracted as black images

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** baowent
**GitHub Username:** b6tang
**Issue:** https://github.com/py-pdf/pypdf/issues/4316
**Pull Request:** [GitHub PR link, once opened]

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end - the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: Built and tested an issue-scout search strategy**

I created a search strategy using GitHub CLI queries for Python, Java, and C++ projects, with metadata-based ranking filters. My first scan evaluated five issues and accepted one, but I had concerns about its implementation complexity and the repository's support for external contributors. I then graded three issues from the Issue Selection site, and all were rejected because the repository prohibits LLM-sourced contributions. These results showed that repository contribution policies and active PRs must be checked before choosing an issue.

**Unit 6: Installed a voice guard and created repository memory for py-pdf/pypdf**

I installed a user-level voice guard and used it to check my claim before posting it to the pypdf issue. My original claim rule required checkable scope before reproduction, which made a simple claim unnecessarily detailed, so I revised the rule to require a concrete first step without promising a fix. I also created a local `CLAUDE.local.md` that records the relevant image-extraction subsystem, tests, contribution rules, and AI policy, and excluded it from Git through `.git/info/exclude`. After setting up the repository's CI test dependencies, I verified the targeted commands and updated the memory file with the actual Windows, Python, pypdf, and Pillow versions.

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

I found the issue through the GitHub Issues Search API rather than by browsing. I searched recent, unassigned Python and C++ issues, then checked repository activity, comments, and linked pull requests. The issue was open, unclaimed, and filed by a pypdf maintainer. I graded it with issue-scout in direct mode; it received an accept verdict with moderate difficulty.

The issue fits my profile because pypdf is a Python project, and the bug involves binary data decoding and debugging in production code. I hope to learn more about handling packed binary image data and how to turn a real parsing bug into a focused regression test.

My regular scout scan did not surface this issue because it was new and lacked contribution-readiness labels. I had to find it through a broader GitHub Issues Search API query and then use issue-scout in direct mode to evaluate it.

Claim comment: https://github.com/py-pdf/pypdf/issues/4316#issuecomment-6521095341

**Toolkit**

- Tool: issue-scout; I repeatedly ran fresh scans while revising its search strategy, then used live direct mode to grade [py-pdf/pypdf#4316](https://github.com/py-pdf/pypdf/issues/4316) after finding it through a separate GitHub Issues Search API query.

- What it produced: The fresh scans produced several candidates that passed the rubric, but none was a good issue for me to claim. In direct mode, issue-scout accepted this issue with all required checks passing and reported a moderate difficulty level.

- Difficulties: The search stage did not surface a realistically claimable issue. This issue was created a few minutes after my latest scan began and would also have been excluded by the 14-day age filter. Several other accepted candidates exposed signals that the search and ranking did not capture well, including specialized setup requirements and limited evidence of support for external contributors.

- What changed: I applied the Unit 6 rubric patch, rewrote the queries and ranking filters, tested contribution-readiness labels, added repository deduplication, and increased the grading count from five to ten. When those changes still did not produce a suitable issue, I used a broader GitHub API search and passed the result to issue-scout in direct mode.

---

## Understanding the Issue

**Problem Description**

[In your own words, what is broken or missing?]

**Expected Behavior**

[What should happen?]

**Current Behavior**

[What actually happens?]

**Affected Components**

[Which parts of the codebase are involved?]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Reproduction Process

**Environment Setup**

- **Operating system:** Windows 11 (`10.0.26300`)
- **Python runtime:** Python 3.14.7
- **Package manager:** pip 26.2.1
- **Project:** pypdf 6.21.4
- **Branch and commit:** `main` at `7f0fb27944b994e2aec53d00457f038ab5312c57`
- **Relevant image dependency:** Pillow 13.0.0

I forked py-pdf/pypdf to my GitHub account and cloned my fork, then created and activated a virtual environment in the repository:

```powershell
gh repo fork py-pdf/pypdf --clone
Set-Location .\pypdf
py -3.14 -m venv .venv
.\.venv\Scripts\Activate.ps1
```

I initially followed the developer introduction and installed `requirements/dev.txt`:
```powershell
python -m pip install -r requirements/dev.txt
```

The first pytest command did not start because pypdf's pytest configuration uses `--disable-socket`, but `dev.txt` did not install the `pytest-socket` plugin that provides that option. I checked the project's testing documentation and installed the CI requirements specified for Python 3.11 and later:
```powershell
python -m pip install -r requirements/ci-3.11.txt
```

This installed `pytest-socket` and the remaining CI test dependencies. After that, pytest started correctly, and the targeted test reproduced the reported failure.

**Steps to Reproduce**

1. I copied the minimal 4 x 1 `/DeviceGray` example from the issue into a temporary `repro_4316.py` file and ran it against the same checkout:

   ```powershell
   python .\repro_4316.py
   ```
2. I ran the existing regression test that Martin pointed out:

   ```powershell
   python -m pytest tests/test_filters.py::test_4bits_image -v -rxX
   ```

   Pytest reported the test as an expected failure for [py-pdf/pypdf#4316](https://github.com/py-pdf/pypdf/issues/4316).
3. I reran the test while ignoring its `xfail` marker so that pytest would show the underlying assertion failure:

   ```powershell
   python -m pytest tests/test_filters.py::test_4bits_image -v --runxfail -vv
   ```
4. The existing test produced an all-black image instead of its expected 16-level grayscale gradient. The issue example produced black palette images for both 2-bit and 4-bit inputs, while the 8-bit control produced the expected grayscale values.



**Reproduction Evidence**

- **Commit showing reproduction:** None. I reproduced the issue using the main branch at commit 7f0fb27944b994e2aec53d00457f038ab5312c57 without changing any tracked files.
- **Screenshots/logs:** Relevant terminal logs are embedded below. Local clone paths were replaced with `<repo>`, and the Pillow deprecation warning from the minimal script was omitted; the outputs are otherwise unchanged.

<details>
<summary>Minimal 2/4/8-bit reproduction from the issue</summary>

```text
2 bits: mode P, palette [0, 0, 0]
[0, 0, 0, 0]
4 bits: mode P, palette [0, 0, 0]
[0, 0, 0, 0]
8 bits: mode L, palette None
[0, 85, 170, 255]
```
</details>

<details>
<summary>Existing test with the repository xfail marker</summary>

```text
===================================================== test session starts ======================================================
platform win32 -- Python 3.14.7, pytest-9.1.1, pluggy-1.7.0 -- <repo>\.venv\Scripts\python.exe
cachedir: .pytest_cache
benchmark: 5.3.0 (defaults: timer=time.perf_counter disable_gc=False min_rounds=5 min_time=0.000005 max_time=1.0 calibration_precision=10 warmup=False warmup_iterations=100000)
rootdir: <repo>
configfile: pyproject.toml
plugins: platformdirs-4.12.4, benchmark-5.3.0, cov-7.1.0, socket-0.8.1, timeout-2.4.0, xdist-3.8.0, typeguard-4.6.0
collected 1 item

tests/test_filters.py::test_4bits_image XFAIL (#4316: 4-bit /DeviceGray images are extracted as black palette images)     [100%]

=================================================== short test summary info ====================================================
XFAIL tests/test_filters.py::test_4bits_image - #4316: 4-bit /DeviceGray images are extracted as black palette images
====================================================== 1 xfailed in 1.71s ======================================================
```

</details>

<details>
<summary>Same test with xfail disabled (--runxfail)</summary>

```text
============================= test session starts =============================
platform win32 -- Python 3.14.7, pytest-9.1.1, pluggy-1.7.0 -- <repo>\.venv\Scripts\python.exe
cachedir: .pytest_cache
benchmark: 5.3.0 (defaults: timer=time.perf_counter disable_gc=False min_rounds=5 min_time=0.000005 max_time=1.0 calibration_precision=10 warmup=False warmup_iterations=100000)
rootdir: <repo>
configfile: pyproject.toml
plugins: platformdirs-4.12.4, benchmark-5.3.0, cov-7.1.0, socket-0.8.1, timeout-2.4.0, xdist-3.8.0, typeguard-4.6.0
collecting ... collected 1 item

tests/test_filters.py::test_4bits_image FAILED                           [100%]

================================== FAILURES ===================================
______________________________ test_4bits_image _______________________________

    @pytest.mark.enable_socket
    @pytest.mark.xfail(
        reason="#4316: 4-bit /DeviceGray images are extracted as black palette images", raises=AssertionError
    )
    def test_4bits_image():
        """
        From #1954, test with 4bits image.
    
        The 16 x 16 /DeviceGray image has the gray level y in each pixel of row y, from black to white.
        """
        reader = PdfReader(BytesIO(get_data_from_url(name="4bits_image.pdf")))
        image = reader.pages[0].images[0].image
        expected = bytes(y * 255 // 15 for y in range(16) for _ in range(16))
        assert image.size == (16, 16)
>       assert image.convert("L").tobytes() == expected
E       assert b'\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00' == b'\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x11\x11\x11\x11\x11\x11\x11\x11\x11\x11\x11\x11\x11\x11\x11\x11""""""""""""""""3333333333333333DDDDDDDDDDDDDDDDUUUUUUUUUUUUUUUUffffffffffffffffwwwwwwwwwwwwwwww\x88\x88\x88\x88\x88\x88\x88\x88\x88\x88\x88\x88\x88\x88\x88\x88\x99\x99\x99\x99\x99\x99\x99\x99\x99\x99\x99\x99\x99\x99\x99\x99\xaa\xaa\xaa\xaa\xaa\xaa\xaa\xaa\xaa\xaa\xaa\xaa\xaa\xaa\xaa\xaa\xbb\xbb\xbb\xbb\xbb\xbb\xbb\xbb\xbb\xbb\xbb\xbb\xbb\xbb\xbb\xbb\xcc\xcc\xcc\xcc\xcc\xcc\xcc\xcc\xcc\xcc\xcc\xcc\xcc\xcc\xcc\xcc\xdd\xdd\xdd\xdd\xdd\xdd\xdd\xdd\xdd\xdd\xdd\xdd\xdd\xdd\xdd\xdd\xee\xee\xee\xee\xee\xee\xee\xee\xee\xee\xee\xee\xee\xee\xee\xee\xff\xff\xff\xff\xff\xff\xff\xff\xff\xff\xff\xff\xff\xff\xff\xff'
E         
E         At index 16 diff: b'\x00' != b'\x11'
E         
E         Full diff:
E           (b'\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00'
E         -  b'\x11\x11\x11\x11\x11\x11\x11\x11\x11\x11\x11\x11\x11\x11\x11\x11""""""""""""'
E         -  b'""""3333333333333333DDDDDDDDDDDDDDDDUUUUUUUUUUUUUUUUffffffffffffffffwwwwwwww'
E         -  b'wwwwwwww\x88\x88\x88\x88\x88\x88\x88\x88\x88\x88\x88\x88\x88\x88\x88\x88'
E         -  b'\x99\x99\x99\x99\x99\x99\x99\x99\x99\x99\x99\x99\x99\x99\x99\x99'
E         -  b'\xaa\xaa\xaa\xaa\xaa\xaa\xaa\xaa\xaa\xaa\xaa\xaa\xaa\xaa\xaa\xaa'
E         -  b'\xbb\xbb\xbb\xbb\xbb\xbb\xbb\xbb\xbb\xbb\xbb\xbb\xbb\xbb\xbb\xbb'
E         -  b'\xcc\xcc\xcc\xcc\xcc\xcc\xcc\xcc\xcc\xcc\xcc\xcc\xcc\xcc\xcc\xcc'
E         -  b'\xdd\xdd\xdd\xdd\xdd\xdd\xdd\xdd\xdd\xdd\xdd\xdd\xdd\xdd\xdd\xdd'
E         -  b'\xee\xee\xee\xee\xee\xee\xee\xee\xee\xee\xee\xee\xee\xee\xee\xee'
E         -  b'\xff\xff\xff\xff\xff\xff\xff\xff\xff\xff\xff\xff\xff\xff\xff\xff')
E         +  b'\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00'
E         +  b'\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00'
E         +  b'\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00'
E         +  b'\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00'
E         +  b'\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00'
E         +  b'\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00'
E         +  b'\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00'
E         +  b'\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00'
E         +  b'\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00'
E         +  b'\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00'
E         +  b'\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00'
E         +  b'\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00'
E         +  b'\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00'
E         +  b'\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00'
E         +  b'\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00')

tests\test_filters.py:519: AssertionError
------------------------------ Captured log call ------------------------------
WARNING  pypdf._reader:_utils.py:531 incorrect startxref pointer(4)
WARNING  pypdf._reader:_utils.py:536 parsing for Object Streams
=========================== short test summary info ===========================
FAILED tests/test_filters.py::test_4bits_image - assert b'\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00' == b'\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x11\x11\x11\x11\x11\x11\x11\x11\x11\x11\x11\x11\x11\x11\x11\x11""""""""""""""""3333333333333333DDDDDDDDDDDDDDDDUUUUUUUUUUUUUUUUffffffffffffffffwwwwwwwwwwwwwwww\x88\x88\x88\x88\x88\x88\x88\x88\x88\x88\x88\x88\x88\x88\x88\x88\x99\x99\x99\x99\x99\x99\x99\x99\x99\x99\x99\x99\x99\x99\x99\x99\xaa\xaa\xaa\xaa\xaa\xaa\xaa\xaa\xaa\xaa\xaa\xaa\xaa\xaa\xaa\xaa\xbb\xbb\xbb\xbb\xbb\xbb\xbb\xbb\xbb\xbb\xbb\xbb\xbb\xbb\xbb\xbb\xcc\xcc\xcc\xcc\xcc\xcc\xcc\xcc\xcc\xcc\xcc\xcc\xcc\xcc\xcc\xcc\xdd\xdd\xdd\xdd\xdd\xdd\xdd\xdd\xdd\xdd\xdd\xdd\xdd\xdd\xdd\xdd\xee\xee\xee\xee\xee\xee\xee\xee\xee\xee\xee\xee\xee\xee\xee\xee\xff\xff\xff\xff\xff\xff\xff\xff\xff\xff\xff\xff\xff\xff\xff\xff'
  
  At index 16 diff: b'\x00' != b'\x11'
  
  Full diff:
    (b'\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00'
  -  b'\x11\x11\x11\x11\x11\x11\x11\x11\x11\x11\x11\x11\x11\x11\x11\x11""""""""""""'
  -  b'""""3333333333333333DDDDDDDDDDDDDDDDUUUUUUUUUUUUUUUUffffffffffffffffwwwwwwww'
  -  b'wwwwwwww\x88\x88\x88\x88\x88\x88\x88\x88\x88\x88\x88\x88\x88\x88\x88\x88'
  -  b'\x99\x99\x99\x99\x99\x99\x99\x99\x99\x99\x99\x99\x99\x99\x99\x99'
  -  b'\xaa\xaa\xaa\xaa\xaa\xaa\xaa\xaa\xaa\xaa\xaa\xaa\xaa\xaa\xaa\xaa'
  -  b'\xbb\xbb\xbb\xbb\xbb\xbb\xbb\xbb\xbb\xbb\xbb\xbb\xbb\xbb\xbb\xbb'
  -  b'\xcc\xcc\xcc\xcc\xcc\xcc\xcc\xcc\xcc\xcc\xcc\xcc\xcc\xcc\xcc\xcc'
  -  b'\xdd\xdd\xdd\xdd\xdd\xdd\xdd\xdd\xdd\xdd\xdd\xdd\xdd\xdd\xdd\xdd'
  -  b'\xee\xee\xee\xee\xee\xee\xee\xee\xee\xee\xee\xee\xee\xee\xee\xee'
  -  b'\xff\xff\xff\xff\xff\xff\xff\xff\xff\xff\xff\xff\xff\xff\xff\xff')
  +  b'\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00'
  +  b'\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00'
  +  b'\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00'
  +  b'\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00'
  +  b'\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00'
  +  b'\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00'
  +  b'\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00'
  +  b'\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00'
  +  b'\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00'
  +  b'\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00'
  +  b'\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00'
  +  b'\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00'
  +  b'\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00'
  +  b'\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00'
  +  b'\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00')
============================== 1 failed in 0.22s ==============================

```

</details>

- **My findings:** I reproduced the reported behavior on Windows 11 at commit `7f0fb27944b994e2aec53d00457f038ab5312c57`. The existing 4-bit test expected a 16-level grayscale gradient, but the extracted image contained only zero-valued pixels; the first difference appeared at byte index 16. The issue's minimal example also extracted both 2-bit and 4-bit `/DeviceGray` images as mode `P` images with a single black palette entry, while the 8-bit control was extracted correctly in mode `L`. Because the minimal example uses no image filter, the failure is independent of the PNG predictor behavior tracked in [py-pdf/pypdf#4308](https://github.com/py-pdf/pypdf/issues/4308). The existing test also emitted startxref and object-stream parsing warnings, but parsing completed and the assertion failed specifically on the extracted pixel values.

**Toolkit**

- Tool: repro-check; I used it in live mode to grade my posted claim together with the completed reproduction report for py-pdf/pypdf#4316.
- What it produced: It returned an accept verdict with all five required checks passing. The grader confirmed that the environment was recorded, the steps were followable, the artifacts directly showed the reported behavior, the conclusion matched the evidence, and the package followed the repository's conventions.
- Difficulties: The final result mixed the required rubric grades with several non-blocking suggestions, so I had to distinguish actual failures from optional feedback. In particular, I checked its comments about requirements/ci-3.11.txt and the test's gray-level wording against the repository's testing documentation and source.
- What changed: No change was required to obtain the accepted verdict. After manually checking the optional feedback, I kept the documented CI requirements and the source-accurate gray-level explanation. 

---

## Solution Approach

**Analysis**

[Your analysis of the root cause: what is causing the issue, traced
to the files and functions involved.]

**Proposed Solution**

[High-level description of your fix approach, and what you are
deliberately NOT changing.]

**Implementation Plan**

1. [Modify file X to do Y]
2. [Add or change Z]
3. [Update tests]

[Record deviations here as they happen: what changed mid-build and
why. A recorded deviation is honest work; silent drift is not.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Testing Strategy

[What observable checks prove the fix: your reproduction steps re-run
against the built change (before and after), and the repo's own test
suite or checks with the outcome shown. "Tests pass" with nothing
observable named is not a strategy.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Implementation Notes

[A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included.]

**[Date]**

[Entry]

**[Date]**

[Entry]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Pull Request

**PR Link:** [GitHub PR URL when opened]

**PR Description:** [what the PR says; much of the content above
adapts into it]

**Maintainer Feedback:**

- [Date]: [feedback received, or "none yet"; silence is a normal
  state and costs you nothing]
- [Date]: [how you addressed it]

**Status:** [Awaiting review / Iterating / Merged / Closed. The
status is a fact you record, never a grade: the PR is graded as
opened.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Learnings and Reflections

**Technical Skills Gained**

[What you learned technically, named concretely.]

**Challenges Overcome**

[What was hard and how you solved it; point at the real events, the
Toolkit blocks above are full of them.]

**What I'd Do Differently Next Time**

[Reflection on your process.]

---

## Resources Used

[What actually helped, not a reading list. List things this story
already shows you using: the doc you followed to set the environment
up, the thread that explained the bug, the person who answered you.
One real entry beats ten plausible ones.]

- [Link to documentation that helped]
- [Discussion, issue, or post that helped]
