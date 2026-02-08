# WSQ & CASL Course Proposal (CP) Generator

## Project Overview

Streamlit app for WSQ and CASL course proposals with AI content generation, Excel validation, and multi-format document export.

## Tech Stack

- **UI**: Streamlit
- **AI**: Claude Agent SDK (`claude_agent_sdk.query()`)
- **Documents**: python-docx (Word), fpdf (PDF), openpyxl (Excel reading)
- **Models**: Pydantic (`app/models.py`)

## Key Files

- `streamlit_app.py` — Main app with all pages and scheduling logic
- `app/ai_generator.py` — AI prompt templates and generation functions
- `app/generator_docx.py` — Word document generation (CP doc + audit report)
- `app/generator_lesson_plan.py` — Lesson plan Word document generation
- `app/generator_lesson_plan_pdf.py` — Lesson plan PDF generation
- `app/extractor.py` — Excel CP file data extraction
- `app/config.py` — Excel cell references and sheet names
- `app/models.py` — Pydantic data models

## Running

```bash
streamlit run streamlit_app.py
```

## Skills

- `.claude/skills/lesson_plan/SKILL.md` — Detailed rules for lesson plan schedule generation (topic duration, distribution, overflow handling, assessment placement). **Use this skill when working on schedule or timetable code.**
- `.claude/skills/generate_topics/SKILL.md` — Rules for generating course topics. In CASL mode, topics are generated using the skill description from `skills_description.csv`. **Use this skill when working on topic generation.**

## Key Rules (Quick Reference)

- 9:00 AM to 6:00 PM daily
- Topics get EQUAL time: `instructional_hours * 60 / num_topics` — never compress
- Lunch: fixed 45 mins, 12:30 PM - 1:15 PM
- Assessment: fixed 4:00 PM - 6:00 PM on last day
- Topics can split into 2 sessions (e.g. T2, T2 Cont'd) across lunch or day boundaries
- Fill remaining time with breaks to fit exactly 9am-6pm

## CP Quality Audit

- Compares uploaded CP Excel against saved course details
- Generates downloadable Word doc with all extracted key info
- Audit report includes data from both Excel extraction and AI-generated session content

## Session State Keys

| Key | Description |
|-----|-------------|
| `saved_course_title` | Course title |
| `saved_course_topics` | Markdown-formatted topics |
| `saved_course_duration` | Total hours (e.g. 16) |
| `saved_instructional_duration` | Instruction hours (e.g. 14) |
| `saved_assessment_duration` | Assessment hours (e.g. 2) |
| `saved_num_topics` | Number of topics |
| `saved_instr_methods` | List of selected instruction methods |
| `saved_assess_methods` | List of selected assessment methods |
| `saved_unique_skill_name` | CASL unique skill name |
| `saved_tsc_ref_code` | WSQ TSC reference code |
| `saved_tsc_title` | WSQ TSC title |
| `mer_text` | AI-generated minimum entry requirements |
| `jr_text` | AI-generated job roles |
| `im_results` | Dict of method -> instruction method descriptions |
| `am_results` | Dict of method -> assessment method descriptions |
| `about_course_text` | AI-generated "About This Course" text |
| `wyl_text` | AI-generated "What You'll Learn" text |
| `bg_text` | Background Part A text |
| `bgb_text` | Background Part B text |
