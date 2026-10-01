# Week 6 Homework: Nashville Engineering Job & Sponsor Radar (Claude Skill)

A reusable **Claude skill** that runs my weekly job-search research as an F-1 international student and recent Electrical + Computer Engineering grad in Nashville, TN. After describing the research once, I can rerun all of it with one sentence:

> "Run my Nashville job radar skill."

The skill file itself (`nashville-eng-job-radar.skill`) is on the [`feature/skill`](../../tree/feature/skill) branch.

## Why I built it

I'm a recent BS EE + BS CompE graduate (Trevecca, May 2026) starting an MS in Applied AI at Lipscomb. I want hands-on work in **embedded systems, sensors and data acquisition, controls, and intelligent systems** near Nashville. On F-1, a matching job title isn't enough. I also need to know whether an employer has sponsored engineers before, and whether an internship could work under CPT. Checking all of that by hand every week is slow, so this skill automates it.

## Research questions it answers

1. **Employers and sponsors:** Which Nashville-area employers are hiring for embedded, sensor, hardware, controls or intelligent-systems roles, and which have a record of sponsoring engineers (H-1B Labor Condition Application filings)?
2. **Matching openings:** Which current openings fit a recent grad with my skills (C/C++ firmware, ADCs and sensors, data acquisition, Python, Raspberry Pi, controls)? Staffing agencies, clearance jobs and senior roles are removed.
3. **Work authorization:** Which matching employers offer internships or co-ops I could do on CPT, and which are confirmed in E-Verify (needed later for a STEM OPT extension)?

## What's inside the skill

| File | What it does |
|---|---|
| `SKILL.md` | Step-by-step instructions Claude follows: which pages to fetch, how to check sponsorship, how to score jobs, and how to format the report |
| `scripts/score_jobs.py` | Python script that scores and ranks every listing into tiers: *Apply now*, *Worth a look*, *Fallback* and *Excluded* |
| `reference/profile.json` | My skills, keyword weights, seniority rules, exclusions (staffing agencies, clearance) and flags (no-sponsorship wording, license required, co-op/CPT) |
| `data/employer_sponsorship.csv` | Employers checked so far, with Tennessee H-1B filing counts, a sponsor tier, and E-Verify status |
| `data/baseline/` | The first run (2026-09-30), so later runs can show which roles are new and which have closed |

## How a run works

1. **Collect** current Nashville-area listings and intern/co-op postings from public job pages.
2. **Check sponsorship** for any new employer using public DOL H-1B LCA disclosure data (via h1bdata.info).
3. **Score** each role with `score_jobs.py`:
   - Skill match: embedded firmware +3, sensors/test/DAQ +3, hardware design +2, controls/automation/robotics +2, Python +1
   - Entry-level, intern or co-op wording: +2; asking for 2+ years: −1
   - Sponsorship history: strong +2, some +1, out-of-state only +0.5
   - Building/power-distribution design (EE, but off my path): −2
   - Excluded: staffing agencies, clearance/citizen-only, senior titles, 5+ years
4. **Compare** with the previous run and report what's new.
5. **Report** the top picks, an employer and sponsor table, CPT-eligible internships, and caveats.

## First-run highlights (2026-09-30)

- **Top matches:** Amazon (Electrical & Control Design Engineer, Automation Hardware Dev Engineer), Quanta Manufacturing Nashville (Hardware Engineer, Test Engineer), Bridgestone EE co-ops, and Hazen and Sawyer (Assistant I&C Systems Engineer).
- **Strongest local sponsor trend:** Quanta Manufacturing Nashville in La Vergne, whose H-1B filings grew every year (7 → 14 → 26 → 37 from 2023 to 2026).
- **CPT options:** Bridgestone EE co-ops, JR Automation (Hitachi) controls internships, and an Amazon automation internship.

## Limitations

- Listings are read through a web page summarizer, so pay and experience details should be confirmed on the actual posting.
- H-1B LCA filings show an employer's willingness to sponsor, not approved petitions.
- E-Verify status is marked *unverified* because the official E-Verify search did not return results to automated tools. Check it manually at e-verify.gov.
- CPT eligibility depends on my program. Confirm with Lipscomb's international student office (DSO) before applying.

## Data sources

- [Indeed](https://www.indeed.com): public job listing pages for Nashville, TN
- [H1B Salary Database](https://h1bdata.info): indexes U.S. Department of Labor LCA disclosure data
- [DOL Office of Foreign Labor Certification](https://www.dol.gov/agencies/eta/foreign-labor/performance)
- [Study in the States: Understanding E-Verify](https://studyinthestates.dhs.gov/students/work/understanding-e-verify)
