# Plan: Resume section detection fails on text with leading whitespace (#54)

## Diagnosis

In `ingestion/parsers/resume_parser.py`, `_detect_sections()` (lines 133–138) anchors section header regex patterns strictly to the start of a line (`^` or `\n`) without tolerating leading whitespace or indentation:

```python
patterns = [
    rf"^{re.escape(section)}\s*$",
    rf"^{re.escape(section)}\s*[:|-]",
    rf"\n{re.escape(section)}\s*$",
    rf"\n{re.escape(section)}\s*[:|-]",
]
```

When resume text contains leading spaces or tabs before section headers (e.g. `    Experience:`, `    Education:`), none of the patterns match, causing `_detect_sections()` to return `[]`.

Additionally, `_strip_markdown()` in the same parser assumes unindented markdown syntax, which interacts with indented text during markdown-formatted resume parsing.

Reproduction evidence from unit 2 repro:

```text
pytest tests/unit/test_resume_parser.py -k "test_detect_sections" --runxfail -v

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
FAILED tests/unit/test_resume_parser.py::TestResumeParser::test_detect_sections - assert 0 > 0

```

## Scope

- Modify the section heading regex patterns in `_detect_sections()` in `ingestion/parsers/resume_parser.py` to allow optional horizontal whitespace (`[ \t]*`) after line starts.
- Adjust `_strip_markdown()` in `ingestion/parsers/resume_parser.py` if needed to handle leading indentation before markdown formatting characters.
- In `tests/unit/test_resume_parser.py`, remove the `@pytest.mark.xfail(strict=True, reason="issue #54...")` decorators from all tests covering issue #54 that pass once indentation is handled (`test_detect_sections`, `test_parse_single_column_resume_text`, `test_parse_resume_no_work_experience`, and any markdown parsing tests resolved by the fix), complying with `docs/CONTRIBUTING.md`.
- Non-goals: Do not restructure `SECTION_HEADERS`, alter the public API schema of `ResumeParser`, or modify unrelated document parsers.

## Files to touch

- `ingestion/parsers/resume_parser.py`
- `tests/unit/test_resume_parser.py`

## Approach

1. In `ingestion/parsers/resume_parser.py`, update `_detect_sections()` patterns to permit horizontal whitespace (`[ \t]*`) before `{re.escape(section)}`:

   ```python
   patterns = [
       rf"^[ \t]*{re.escape(section)}\s*$",
       rf"^[ \t]*{re.escape(section)}\s*[:|-]",
       rf"\n[ \t]*{re.escape(section)}\s*$",
       rf"\n[ \t]*{re.escape(section)}\s*[:|-]",
   ]
   ```


2. Inspect `_strip_markdown()` in `ingestion/parsers/resume_parser.py` to ensure leading whitespace preceding markdown headings/markers does not prevent stripping.
3. In `tests/unit/test_resume_parser.py`, drop the `@pytest.mark.xfail` markers from all tests covering issue #54 whose pass condition is met by this fix, preventing `XPASS(strict)` test failures in CI.
4. Run `pytest tests/unit/test_resume_parser.py` to verify that previously failing and XPASS tests now cleanly pass as standard passing tests.

## Test plan

1. Run the targeted repro test directly:

    ```bash
    pytest tests/unit/test_resume_parser.py -k "test_detect_sections" -v
    ```

    **Expected output after fix:** `1 passed` (without `--runxfail` and without XFAIL status).

2. Run all tests covering issue #54:

    ```bash
    pytest tests/unit/test_resume_parser.py -k "test_detect_sections or test_parse_single_column_resume_text or test_parse_resume_no_work_experience" -v
    ```

    **Expected output after fix:** All targeted tests pass with regular `PASSED` status, 0 `FAILED`, and 0 `XPASS(strict)`.

3. Run the full unit test suite:

    ```bash
    pytest tests/unit/test_resume_parser.py
    ```

    **Expected output after fix:** Clean test run with 0 failures and no unexpected passes.

## Risks and unknowns

- Permitting arbitrary leading whitespace could match a sentence or bullet if it starts with a section keyword; requiring standard line delimiters (`\s*$` or `\s*[:|-]`) mitigates this.
- If markdown tests (`test_parse_markdown_resume`, `test_strip_markdown_syntax`) rely on broader markdown grammar changes beyond indentation normalization, their xfail status must be evaluated carefully so only fully resolved tests have their markers removed.
- Tests marked `strict=True` will immediately fail CI as `XPASS` if made to pass without dropping their markers; each resolved test must have its decorator cleaned up.

## Deviations

None. The implementation followed the planned approach without modifications.
