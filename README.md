# Windows Version Numbers

A current reference of Windows version, build and end-of-support dates:
Windows 11, Windows 10 and Windows Server. Live at
https://njshell2.github.io/windows-versions/

It recreates the long-running reference at
https://www.techthoughts.info/windows-version-numbers/ (last updated
1 Dec 2021, stops at Windows 11 22H2) with current data, and it is
rebuilt automatically every month.

## What the site carries

- Windows 11 release versions 21H2 through 26H2: version, OS build,
  release date, latest build with date and KB, and end of servicing for
  Home/Pro and for Enterprise/Education. Includes the 24H2 LTSC dates
  and the 26H1 note (preinstalled on select new devices only).
- Windows 10 release versions 1507 through 22H2: version, OS build,
  code name, marketing name, release date, and end of servicing per
  edition, plus LTSC/LTSB facts.
- Windows Server 2016, 2019, 2022 and 2025 (LTSC), plus the 23H2
  Annual Channel release: build, release date, latest build, mainstream
  and extended support end dates.
- Operating system version summary: the major.minor number Windows
  itself reports (Windows 11, Windows 10 and recent Server all report
  10.0; the build number is what tells them apart).
- How to check your own version (winver and a short PowerShell command).

## Sources (Microsoft Learn only for dates and builds)

- Windows 11 release information
- Windows 10 release information
- Windows Server release information
- Windows 11 and Windows 10 lifecycle pages (Home and Pro,
  Enterprise and Education)
- Windows 11, version 26H1
- Operating System Version

The code names and marketing names in the Windows 10 table follow the
original reference's naming. Every date and build number comes from a
Microsoft page. End dates are the last day of support; Microsoft's
lifecycle pages show retirement timestamps in Pacific Time, one day
after the last day of support.

## How it stays current

- `data/data.json` is the single source of truth for every table.
- `pipeline/generate.py` renders `pipeline/site-out/index.html`
  (plus sitemap.xml and robots.txt) from data.json. No network.
- `pipeline/update.py` fetches the Microsoft Learn release and
  lifecycle pages once a month, parses the servicing tables, and
  updates data.json only when the parse yields complete tables. A
  fetch or parse failure aborts with no writes, so a bad scrape can
  never publish stale data as new. It also adds brand-new versions
  that appear in a servicing table.
- `pipeline/rebuild.py` runs update.py, regenerates the site, and
  pushes to GitHub only when the built page changed. It prints
  NOCHANGE, CHANGED or FAILED.
- `pipeline/create_repo.py` creates the repo (one-time setup).
- Monthly run: hook `windows-versions-watch`, polls every 6 hours,
  runs rebuild.py once per month (first poll of the new month),
  silent on NOCHANGE, wakes an agent on CHANGED or FAILED.
  State: `~/hooks/state/windows-versions.json`.
  Log: `logs/rebuild-log.md`.

## Packet builder

No-fit, recorded. The monthly job is a script relay with external
live inputs (Microsoft Learn pages), the same class as the agenda and
newsroom relays: no agent reads the inputs, so there is nothing for a
packet to gate. The script itself diffs (published-hash comparison)
before publishing.

## History

- 8 Nov 2026: built from Microsoft Learn pages (last update stamp
  8 Nov 2026). Windows 11 26H2 is the latest release; Windows 10
  support ended 14 Oct 2025.
