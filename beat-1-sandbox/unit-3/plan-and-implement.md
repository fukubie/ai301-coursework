## Posted Upstream

**GitHub username**

fukubie

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/54#issuecomment-6029990935

## Plan for #54: Support Leading Whitespace in Resume Section Detection and Markdown Stripping

### Diagnosis

In `ingestion/parsers/resume_parser.py`, `_detect_sections()` anchors section header patterns strictly at the start of a line (`^` and `\n`) without allowing horizontal indentation, returning `[]` on indented resumes. Additionally, `_strip_markdown()` expects unindented heading markers, which can affect markdown resumes containing leading indentation.

### Proposed Changes

In `ingestion/parsers/resume_parser.py`:

- Update `_detect_sections()` regex patterns to tolerate optional leading horizontal whitespace (`[ \t]*`) before section keywords.
- Update `_strip_markdown()` to handle leading horizontal whitespace preceding markdown headers and formatting markers.

In `tests/unit/test_resume_parser.py`:

- In accordance with `docs/CONTRIBUTING.md`, drop the `@pytest.mark.xfail(strict=True)` decorator from tests covering issue #54 whose pass condition is met by this fix (including `test_detect_sections`, `test_parse_single_column_resume_text`, and `test_parse_resume_no_work_experience`), avoiding XPASS(strict) CI failures while evaluating markdown test markers against the adjusted markdown parser.

### Validation

Run targeted tests to confirm resolved tests pass with 0 XPASS and 0 failures:

```bash
pytest tests/unit/test_resume_parser.py -k "test_detect_sections or test_parse_single_column_resume_text or test_parse_resume_no_work_experience" -v
```

Run the full module test suite to confirm overall status:

```bash
pytest tests/unit/test_resume_parser.py
```

## Your Branch

**Branch**

`fix/54-leading-whitespace`

## Evidence

### Before

```text
$ pytest tests/unit/test_resume_parser.py -k "test_detect_sections" --runxfail -v
=================== test session starts ====================
platform win32 -- Python 3.14.5, pytest-9.1.1, pluggy-1.6.0
collected 10 items / 9 deselected / 1 selected

tests/unit/test_resume_parser.py::TestResumeParser::test_detect_sections FAILED [100%]

========================= FAILURES =========================
__________ TestResumeParser.test_detect_sections ___________

self = <tests.unit.test_resume_parser.TestResumeParser object at 0x00000215ADBEDF20>
parser = <ingestion.parsers.resume_parser.ResumeParser object at 0x00000215ADBC8830>

    @pytest.mark.xfail(
        strict=True, reason="issue #54: resume section detection fails on leading whitespace"
    )
    def test_detect_sections(self, parser):
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
================= short test summary info =====================
FAILED tests/unit/test_resume_parser.py::TestResumeParser::test_detect_sections - assert 0 > 0
============= 1 failed, 9 deselected in 1.25s ==============
```

### After

```text
$ pytest tests/unit/test_resume_parser.py -v
========================================= test session starts =========================================
platform win32 -- Python 3.14.5, pytest-9.1.1, pluggy-1.6.0
collected 10 items

tests/unit/test_resume_parser.py::TestResumeParser::test_parse_single_column_resume_text PASSED  [ 10%]
tests/unit/test_resume_parser.py::TestResumeParser::test_parse_resume_no_work_experience PASSED  [ 20%]
tests/unit/test_resume_parser.py::TestResumeParser::test_parse_multipage_pdf PASSED              [ 30%]
tests/unit/test_resume_parser.py::TestResumeParser::test_parse_markdown_resume PASSED            [ 40%]
tests/unit/test_resume_parser.py::TestResumeParser::test_parse_invalid_content_type PASSED       [ 50%]
tests/unit/test_resume_parser.py::TestResumeParser::test_parse_invalid_list_content PASSED       [ 60%]
tests/unit/test_resume_parser.py::TestResumeParser::test_detect_sections PASSED                 [ 70%]
tests/unit/test_resume_parser.py::TestResumeParser::test_strip_markdown_syntax PASSED            [ 80%]
tests/unit/test_resume_parser.py::TestResumeParser::test_pdf_parsing_error_handling PASSED       [ 90%]
tests/unit/test_resume_parser.py::TestResumeParser::test_parse_preserves_text_content PASSED     [100%]

========================================= 10 passed in 0.27s ==========================================
```

## Eval Iterations

### Run History

- Agreement: 18/20 scored items

### Package Analysis

**Package:** `pkg-01`\
**Category:** wrong-cause\
**Skill verdict:** reject\
**Gold verdict:** reject

**Explanation:** `pkg-01` identified an incorrect root cause by attributing the failure to an ungrounded parser state rather than the actual line-start regex anchors in `_detect_sections()`. Our rubric check, `diagnosis_grounding`, evaluated whether the stated root cause directly accounts for the specific error demonstrated in the reproduction trace. Because the proposed cause failed to explain why `assert len(sections) > 0` failed with `0 = len([])`, the check failed and yielded a reject verdict matching gold.

### Check Rationale

**Check quoted from `rubric.md`:**

```markdown
| diagnosis_grounding | The plan's stated root cause compared against the failure trace and observations in the repro evidence | The identified root cause directly accounts for the specific error or unexpected output demonstrated in the reproduction, rather than addressing an unrelated issue or superficial symptom. | required |
```

**Rationale:** We required that the identified root cause directly explain the specific error observed in the reproduction trace rather than merely matching keywords in the issue title. Earlier iterations allowed vague references to the subsystem; tightening this check prevented packages that address superficial symptoms (like pkg-01 and pkg-11) from mistakenly passing.

### Trade-offs

By strictly requiring that the stated root cause directly explain the specific reproduction failure trace, the rubric risks rejecting valid preliminary plans for complex multi-subsystem bugs where the initial reproduction only manifests a secondary symptom or where multiple interacting faults combine to cause the observed failure.