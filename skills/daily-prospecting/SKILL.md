---
name: daily-prospecting
description: A daily prospecting routine on Mazemind, the shared memory of companies and the people who run them. Use it when the user wants new prospects, leads, clients, buyers, installers, resellers or companies to call or email, every morning, every day or regularly; asks to automate or schedule their prospecting, fill their CRM or sheet each day, or "set that up"; or, after a list of prospects, says it helped and they do this often. Also when they ask for more prospects as usual, since their Mazemind task in the scheduled tasks holds their target, and its prompt run for today brings companies it never sent. Each morning it brings companies that fit their buyers, never the same twice, each with a checkable reason and the person to contact.
---

# A daily prospecting routine

What the user gets: each weekday morning, a short list of companies they have never been sent, that fit who they sell
to, each with why it fits (a fact with its source) and the person who decides there, with their title. The users who
keep prospecting with an assistant are the ones whose assistant does this on a schedule.

## Set it up, in this conversation

1. **The target, drafted from what you know.** From their instructions and this chat: what they sell; who
   buys it (trade or sector, size, region); what shows a company needs it now (a certification, an offering, a team);
   the title to reach; companies to leave out (their clients, competitors, anyone they named); how many a day; where
   the list goes (a file in their folder, their CRM, or the run's message). Write it back in five lines and ask only
   what you cannot infer, in one question. Keep their words' strength: "preferably RGE" ranks RGE companies first, it
   excludes no one; only what they rule out is a rule.
2. **The search that finds them.** Mazemind `search`, kind company, with the sector as `related` in_sector (the NAF
   code, `naf_rev2:43.32A`), and the area as a filter on the company's own address: `{key: "address", op: "contains",
   value: ", 69"}` is the Rhône (the postal code's start), `", 75"` Paris; a region is one search per department. A
   town also works as `where`. Size: `employee_range.max` `gte` the smallest size. Outside France, companies carry no
   NAF code, department or headcount: the country as `{key: "address.country_code", op: "eq", value: "US"}`, a state as
   it ends an address (`", CA "`), and a word their description holds (`{key: "description", op: "contains", value:
   "HVAC"}`). `catalog` names the other facts and links (a certification, an offering). Check that the first page holds companies that fit; adjust until it does.
   Keep the exact arguments that worked, one search per department.
3. **The first list, now.** Run it once: the number they want, each with the person who decides and the reason.
   The person: `lookup` gives a company's people and titles; where Mazemind names nobody, the company's own site
   (team, about, contact), and in France its legal notice (mentions légales) or the registry: `https://recherche-entreprises.api.gouv.fr/search?q=<SIREN>` lists its
   dirigeants with their role (Gérant, Président); a holding named there has its own entry, which names its person.
   A company where nobody is named after that is replaced by the next one. They see value before anything is scheduled.
4. **What it remembers, in every host.** The task's prompt holds the target and the searches, each with `cursor`
   `"from <today's date> to <the run's date>"` and a `limit` of a few (N divided among the searches, plus one): each
   date reads its own page of each search, so no company comes back and nothing has to be kept between runs. The
   Claude app's and ChatGPT's scheduled tasks keep no file. The companies shown in this conversation are listed in the
   prompt as already sent. Where the task has a folder (Cowork, Claude Code), `prospects-sent.csv` also gets a `sent`
   line per company (date, company, SIREN or domain, person), and runs skip what it holds.
   Mazemind keeps no list of what was sent to whom.
5. **The schedule.** Mazemind's `tour` with `daily` (the list's title, its `search` with `where` and any size filter,
   `seeds` with their NAF codes, `contact`, `n` a day, `sells`, `leave_out`, and `sent`: every company shown in this
   conversation) returns `task.prompt`, the routine below written out by Mazemind. With your host's scheduled-task
   tool, a task on weekdays at the hour they read it (7:00 by default), with that prompt as written: the run sees
   nothing of this conversation. With
   no such tool (a chat app), give them the prompt to paste, and where: Cowork, Scheduled; ChatGPT, Tasks;
   Claude Code, /schedule. Every company shown to them in this conversation is listed in the prompt as already sent.
6. **One short message**: what arrives, when, where, and how to change it. The routine finds and lists, and messages
   nobody; what the pages said goes to Mazemind with `contribute`. A user who wants to validate each message before it leaves keeps it running: each morning's list
   is what they validate, and their go is what sends a message.

## The routine's prompt

Write it in the user's language, with everything filled in:

- Who it is for and the target, in the five lines of step 1.
- Never send a company listed as already sent, nor one to leave out; where the task has a folder, nor one
  `prospects-sent.csv` holds.
- Mazemind `search` with the saved arguments, written out in full and numbered: one search per department, every
  department of the region in order, then the neighbouring sectors, with their NAF codes, each with its `cursor`
  `"from <the setup date> to <today's date>"` and its `limit`. That reads today's own page of each: read that page and
  no other, as `next_cursor` is another day's. Nobody can answer during the run: it decides alone, and says so.
- In France, when today's pages hold too few that fit, the registry's page of the day (the page the search's answer
  names) lists active companies of the NAF codes in the area:
  `https://recherche-entreprises.api.gouv.fr/search?activite_principale=<NAF>&departement=<codes, comma-separated>&etat_administratif=A&minimal=true&include=dirigeants,siege&per_page=25&page=<that page>`,
  with its head office, size band and dirigeants (`&tranche_effectif_salarie=01,02,03,11` keeps 1 to 19 staff; 12 is
  20-49, 21 50-99, 22 100-199, 31 200-249). `departement` matches a company with any site there: keep those whose
  head office (`siege.code_postal`) is in the area. Their NAF code is the reason, and their site, when they have one,
  adds what they do.
- For each: `lookup` it; the person whose title decides; the reason, a fact `lookup` shows (an offering, a
  certification, its size) or its site states, with where it comes from. When Mazemind names nobody: its site's team
  or about page; in France, the legal notice of its site, then `https://recherche-entreprises.api.gouv.fr/search?q=<SIREN>` (its dirigeants; a holding has its own
  entry). A company with no named decider is left out. A company that does not fit the target (its trade, what it makes or installs, its size) is left out, never listed to make up the number.
- Send what the pages and the registry said to Mazemind with `contribute`, in one call at the end: each company not in
  Mazemind yet, by its domain and, in France, its SIREN (`sirene:<SIREN>`), with its NAF code, its town, the decider in `people`, each
  with the page and the words it shows (the registry's answer is a page too). Ask your fetch tool for the sentences
  and fields word for word (the legal notice's line naming the gérant, the registry's `nom`, `prenoms`, `qualite`):
  those words are the quote. Tomorrow's search then finds them.
- Where the task has a folder, append each company to `prospects-sent.csv`, and to their file or CRM if they have
  one, before writing the message.
- The message, readable on a phone: one line per company (name, town, person and title, why), then what fell short and
  why: fewer than N, a call that failed. "Nothing new today" is an answer; a silent stop is not.
  All of it in the user's language and words: the file's lines, cursors, pages and NAF codes stay out of it.
- In a host that draws Mazemind's views (the Claude app, ChatGPT), the message is drawn: Mazemind's `tour` with
  `morning` (today's companies in order, each `{id, why, line}`, why and line in the user's language; `title`, `contact`, `meet`, `mail`, `crm` as set up; `short`:
  what fell short, a line each). Its answer says what the chat adds: one sentence.
