# What a cosmic is

A **cosmic** is one customer's marketing infrastructure, owned by that customer,
assembled and operated by us.

It is three things and a purpose:

| | | |
| :--- | :--- | :--- |
| **Supabase** | their data | first-party records, in a store they own |
| **Netlify** | their site | built from that data |
| **A credential vault** | their access | scoped to one company, proving we can reach their systems |
| **→** | **the purpose** | **their marketing success** |

Everything else is scaffolding around those three.

---

## Why it is defined this way

Most agencies hold a customer's marketing in systems the customer cannot see,
cannot leave with, and cannot audit. When the relationship ends, the work does
not transfer — because it was never theirs.

A cosmic inverts that. The data store is the customer's. The site is built from
that store. The credentials are theirs, held in a vault scoped to them alone,
and every one of them is revocable by the customer without our help.

That is not generosity. It is the only arrangement where **"use the best data
available"** is a promise we can keep, because the best data is first-party, and
first-party data requires access we have to be trusted with.

---

## The three parts

### Supabase — their data

The destination. Everything extracted from the customer's own systems and from
vendors holding the customer's records lands here.

It matters that it is theirs rather than ours because vendor data is on loan.
Retention windows shorten, APIs deprecate, rate limits tighten, terms change.
Search Console holds a rolling window. An analytics product keeps what its
current plan says it keeps. None of it is guaranteed to exist next year.

Landing a copy in a store the customer owns is the difference between reporting
on a business and renting the ability to.

### Netlify — their site

The deliverable, built from the store rather than authored by hand.

A site built from data can be rebuilt, re-themed and re-templated without
retyping content, and every page it publishes is traceable to a record. A site
authored by hand is a set of files somebody has to maintain forever.

### The vault — their access

One vault per customer. **The only people who need those keys are us and them.**

It holds the credentials that prove we can reach the customer's own systems —
DNS, hosting, the application — and the vendor credentials that reach the
customer's records held elsewhere. It is provisioned and proved by `lucky`.

---

## What a cosmic is not

Naming the boundaries is most of the definition, because each of these was
built in the wrong place first.

**A cosmic is not the tooling that provisions it.** Collecting credentials,
securing them, proving they work, and reporting on them is `lucky`'s job.
A customer instance should contain the customer's site and the customer's data,
and nothing about how we got access to either.

**A cosmic is not the catalogue of what a credential unlocks.** *Lucky knows
the keys; connections knows the doors.* What a credential can reach is a
separate question from whether we hold it and whether it works.

**A cosmic is not a copy of a website.** What is reusable — layouts,
components, tokens, page templates, the schema a local business needs to rank —
is shared. An instance is the part that differs.

---

## What "working" means

A cosmic is not finished when it is deployed. It is working when four questions
have current answers, per credential:

1. **captured** — do we have it
2. **secured** — is it properly in the vault, and out of the email it arrived in
3. **works** — proved against the system it is for, by doing the thing
4. **when did we last check**

A credential that has never been checked, one that failed, and one that passed
six weeks ago are three different states and must never render as one red light.

Anything not working carries **whose move it is** — blocked on the customer,
or blocked on us — because only the first generates a message to the customer,
and a stalled round-trip is the whole cost of provisioning.

---

## The order the work goes in

**Marketability before visibility.**

*Visibility* is rankings, impressions, clicks — what most reporting measures.
*Marketability* is whether the thing is in a state where marketing can work at
all: ownership, access, integrity, crawlability, identity, conversion,
performance, content.

Measure the second first. A ranking report for a site whose domain lapses next
quarter, whose form submissions land nowhere anyone can query, and half of whose
crawl surface is undeclared is a precise number describing something that cannot
convert.

This ordering is also the only one available, which is what makes it more than
a slogan: **marketability is computable from first-party data alone**, while
visibility requires a vendor to grant access first.

---

## Capability arrives in tiers

A cosmic produces value before the customer has handed over anything, and more
as each source connects.

| Tier | Needs | Becomes possible |
| :--- | :--- | :--- |
| **0** | nothing — public DNS and the live site | Is the domain delegated correctly? Is mail authenticated? Is there a CDN? What does the site declare? |
| **1** | domain and DNS control | Ownership proved, records correct, delegation ours |
| **2** | hosting — cPanel, SSH | First-party data: the database, the form submissions, the raw access logs |
| **3** | vendor APIs | What people searched, what the search engine decided, what customers actually bought |

Each connection lights up features and pages that could not exist before it, and
the customer is told which — so access is a visible exchange rather than a
request they have to take on faith.

Tier 0 must be complete and useful with **zero** sources connected. That is what
makes the thing sellable before anyone hands over a password.

---

## The recurring failure this is built against

Across every system a cosmic touches, the same failure keeps appearing:

> **Something returns less without saying so.**

A CDN cache hit that never reaches the origin log. Row-level security filtering
rows behind a `200`. An API omitting fields nobody registered. A referrer-
restricted key refusing a server. A credential reported as missing when the
reference simply could not be parsed.

None of these announce themselves. All of them look like a smaller true answer.
A cosmic is built to notice the difference, and to say which one it is.
