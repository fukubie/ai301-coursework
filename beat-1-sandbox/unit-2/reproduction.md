# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**

fukubie

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/54#issuecomment-3349947842

I am investigating issue #54 to attempt local reproduction on a clean build using the reported test cases for `_detect_sections()`. I will follow up with an environment and trace report once verified.

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/54#issuecomment-5902669470

## Environment

- **OS / Platform:** Windows 11 (win32) via Git Bash / MINGW64
- **Runtime:** Python 3.14.5
- **Test runner:** pytest 9.1.1 (pluggy 1.6.0)
- **Commit:** `2f4e82f52efbcfcc57d65b3fa5348672163ca088`

## Steps to Reproduce

Clone the repository, check out the commit, create and activate a virtual environment, and install the project and test runner:

```bash
git clone https://github.com/codepath/pathreview-ai301-fa26-s3.git
cd pathreview-ai301-fa26-s3
git checkout 2f4e82f52efbcfcc57d65b3fa5348672163ca088


# Create and activate a virtual environment.

python -m venv venv
source venv/Scripts/activate
# Install the project and pytest.

pip install -e .
pip install pytest
# Run the targeted test with its xfail marker bypassed.

pytest tests/unit/test_resume_parser.py -k "test_detect_sections" --runxfail -v
```
## Observed Behavior
The test fails on the assertion that parsed sections are non-empty:

```text
tests/unit/test_resume_parser.py::TestResumeParser::test_detect_sections FAILED [100%]

========================= FAILURES =========================
__________ TestResumeParser.test_detect_sections ___________

self = <tests.unit.test_resume_parser.TestResumeParser object at 0x000001EB2174DF20>
parser = <ingestion.parsers.resume_parser.ResumeParser object at 0x000001EB21744980>

    @pytest.mark.xfail(
        strict=True, reason="issue #54: resume section detection fails on leading whitespace"
    )
    def test_detect_sections(self, parser):
        """Test section detection in resume text."""
        text = """
        Experience:
        Senior Developer at TechCorp

        Education:
        BS Computer Science

        Skills: Python, JavaScript
        """
        sections = parser._detect_sections(text)

        assert isinstance(sections, list)
>       assert len(sections) > 0
E       assert 0 > 0
E        +  where 0 = len([])

tests\unit\test_resume_parser.py:152: AssertionError
================= short test summary info =========================
FAILED tests/unit/test_resume_parser.py::TestResumeParser::test_detect_sections - assert 0 > 0
============= 1 failed, 9 deselected in 0.36s =============
```
When the input contains leading whitespace before the section headings (Experience:, Education:, and Skills:), `_detect_sections()` in `ingestion/parsers/resume_parser.py` returns an empty list (`[]`).

## Eval iterations
Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

### Run history

- agreement: 16/20 scored items (bar: 18/20: below the bar)

- agreement: 4/4 scored items (partial run via `--only pkg-01,pkg-05,pkg-09,pkg-10`)

- agreement: 20/20 scored items (bar: 18/20: PASS)

### Package analysis

#### pkg-01

**Rubric decision:** reject in Run 1; accept in Run 3.

**Gold label:** accept

**Why the rubric read it that way:** In Run 1, my initial `target_alignment` pass condition demanded exact artifact alignment against the issue's primary failure mode description. In pkg-01, the recorded trace captured an environment-dependent output variation that differed in line formatting from the issue description. Because the check did not explicitly allow for reasonable variations across dependency or platform versions, the rubric graded it fail on `target_alignment` and rejected the package. Once the check was refined to accept valid variations of the failure domain rather than penalizing cosmetic formatting differences, it graded pass and matched the gold accept.

### Check rationale

> | followable_steps | The setup, command sequences, and input payloads detailed in the reproduction steps. | A developer familiar with standard project workflows could follow the sequence from a clone/baseline to trigger the target condition without guessing non-obvious configurations, custom undocumented flags, or hidden prerequisites. Standard developer actions (like changing directories or running standard package managers) may be assumed. | required |

**Why it reads that way:** The initial check required that “a stranger starting from a fresh clone could follow the sequence end-to-end to trigger the target condition without guessing missing flags, hidden paths, or unspoken configuration changes.” In Run 1, this failed pkg-05 because the author omitted common-sense developer primitives like `cd` commands and standard environment setup. I revised the pass condition to explicitly permit standard developer assumptions so that the check evaluates whether non-obvious configurations or missing reproduction flags are present, rather than penalizing concise instructions for omitting standard package management steps.

### Trade-offs

Loosening `followable_steps` and `target_alignment` introduces the risk of accepting packages with sloppy reproduction steps or adjacent errors. To ensure this change did not degrade our filters against bad packages, I verified the canaries from the negative categories: pkg-04 (disclosure canary) remained reject, and pkg-06 (wrong-target category) remained reject. The confirming full run proved that all negative categories (wrong-target 4/4, no-evidence 4/4, unfollowable-comms 3/3, disclosure 1/1) held at 100% agreement, confirming that accepting standard developer conventions did not compromise detection of truly missing or misaligned evidence.