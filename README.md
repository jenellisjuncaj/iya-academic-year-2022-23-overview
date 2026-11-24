# Academic Year 2022–23 Enrollment Overview

USC Iovine and Young Academy. Fall 2022 (20223), Spring 2023 (20231), Summer 2023 (20232). Matches the existing 2023–24 dashboard layout and counting rules.

## GitHub setup

1. Create a separate blank repository named `iya-academic-year-2022-23-overview`.
2. Extract `Academic_Year_2022_23_Dashboard.zip`. Open the `academic-year-2022-23-overview` folder inside it.
3. Upload the nine root files: README.md, app.js, data.json, index.html, netlify.toml, overview.js, sources.json, styles.css, tests.js. Commit changes. Do not upload the ZIP or enclosing folder.
4. Create `scripts/refresh-data.js` using Add file → Create new file. Open the matching extracted file in Notepad and copy its entire contents into the editor. Commit.
5. Similarly create `.github/workflows/refresh-data.yml` from the matching extracted file. Commit.
6. Actions → Refresh 2022-23 historical snapshots → Run workflow → main. Check that the run succeeds and inspect the dashboard semester status cards.

## Netlify setup

Create a separate project using Add new project → Import an existing project → GitHub → iya-academic-year-2022-23-overview.

- Project name: `iya-academic-year-2022-23`
- Branch: `main`
- Base directory: blank
- Build command: `node scripts/refresh-data.js`
- Publish directory: `.` (one period)
- Node version: 22
- No environment variables, dependencies, or API key are needed.

If Netlify cannot see the repository, add only this repository to its GitHub app access while retaining existing repository selections. Deploy, then check the 2022–23 title, all three terms, program filter, course search, sorting, and semester status cards.

## Retrieved data

Retrieved November 23, 2026, from the USC public CoursesByTermSchoolProgram endpoint, with school=ACAD and programs ACAD, IDSN, and PRIN for each of the three terms. Exact timestamps are stored in data.json. The separate USC_2022_23_API_Snapshots.json deliverable contains all nine source responses, request URLs, and retrieval timestamps. Raw responses are not needed for deployment.

| Metric | Fall 2022 | Spring 2023 | Summer 2023 | Annual |
|---|---:|---:|---:|---:|
| Course enrollments | 1,100 | 1,155 | 236 | 2,491 |
| Student credit hours / units | 3,364 | 3,635 | 607 | 7,606 |
| Sections | 73 | 67 | 18 | 158 |
| Available seats | 473 | 526 | 112 | 1,111 |
| Seat capacity | 1,572 | 1,681 | 348 | 3,601 |

Annual capacity utilization: 69.2%. 71 distinct prefix+course-number rows.

Enrollment counts are course registrations, not unique students. These are currently available historical API records, not certified final or census-date counts. Retrieval time does not establish when USC changed a record.

Sections are deduplicated within each term and assigned to their scheduled ACAD/IDSN/PRIN prefix. Other-school scheduled cross-listings are outside scope. Four cancelled sections are excluded (two spring, two summer); IDs and reasons are in data.json. Missing/invalid seat counts and nonpositive capacity are also excluded. Empty individual program responses are valid when the term has in-scope sections.

Student credit hours use enrollment × exactly one published fixed-unit value. Unknown or variable units remain in enrollment but are excluded from SCH: 32 sections, representing 49 registrations. Known zero-unit sections contribute zero SCH. Available seats sum max(capacity − enrollment, 0) per section; overfilled sections do not offset open seats elsewhere.

## Refresh and validation

Daily GitHub Actions refresh at 15:25 UTC, manual Actions runs, and Netlify build refresh are included. A source failure preserves the term's last successful snapshot and labels it. Missing terms block annual totals. Retrievals older than 48 hours are marked outdated. Public GitHub schedules may stop after extended repository inactivity.

Run `node tests.js` with Node 22 to check annual sums, semester/course reconciliation, unique IDs, units, freshness and failure retention. The standalone `Academic_Year_2022_23_Preview.html` can be opened directly and uses a saved snapshot; it does not refresh. Use index.html through a web server or Netlify for the refreshing dashboard.

Validation completed: all nine saved API responses independently reconciled with semester totals; calculation tests passed; dashboard and standalone preview rendered in a browser; ACAD, IDSN, PRIN, all-program filters, search, reset, and sorting checked.
