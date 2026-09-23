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

An idea and an environment, in roughly equal measure. The environment is the
three things above, running. The idea is that a business should own the record
of its own marketing, and that owning it is what makes the marketing work.

---

## Identity

A cosmic is a named thing, and it is **paired back to a company account**.

```
company account
  └── owner              assumed authorized
       └── cosmic        the environment: data, site, vault
            └── keys     one vault, scoped to this company
                 └── connected properties
```

**We assume the owner of the account is authorized.** That assumption is worth
stating plainly rather than leaving implicit, because everything downstream
rests on it: every grant, every consent record, every key handed over. If the
person who opened the account cannot actually speak for the business, nothing
below them in that chain is sound.

Lucky carries the machinery for this — who granted what, when, and on whose
authority, and the idea of a **prime**: the person who can speak for an account
when authority is in question. An account with no prime is an account nobody
can speak for. A cosmic inherits its authority from that chain rather than
asserting its own.

---

## The circle of trust

Lucky automates building a circle of trust around the **owner** — the
connected, verified properties that together say *this business exists, it is
who it claims to be, and these details agree*.

The useful part is that **one act does two jobs**. Connecting a property is
normally filed as an administrative chore, something to get through before the
marketing starts. It is not:

| Connecting a property | Gives data | Improves local visibility |
| :--- | :---: | :---: |
| Google Business Profile | reviews, Q&A, photos, hours, insights | **yes — the strongest local asset there is** |
| Domain and DNS | delegation, mail authentication, certificates | foundational; nothing ranks on a broken zone |
| Search Console | what people searched, what Google decided | verification and coverage |
| Hosting and application | logs, submissions, the real content model | speed, crawlability, correctness |
| Field-service system | what people actually bought, and where | the evidence for which pages should exist |

So the answer to *"why should I give you access to all this?"* is not "so we can
do our job". It is: **granting access is itself the first piece of the work.**
Each connection makes the business more legible to the systems that decide local
visibility, and starts a record that nobody is keeping today.

That reframing matters for how it is sold. Access stops being a cost the
customer pays up front for a benefit that arrives later, and becomes the benefit
arriving in instalments — which is also why each connection should be reported
back the moment it lands.

Note the one that is doing the most work here is the least exercised: **Google
Business Profile**. It is both the richest local-visibility asset and a genuine
data source, and on the first instance it has never been successfully read — the
service account is provisioned for it, but the only attempt so far returned a
quota error.

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
on a business and renting the ability to. See **we are packrats**, below, for
what that is worth in months.

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

## We are packrats

Vendors delete your data on a schedule. We do not.

| Source | What it keeps |
| :--- | :--- |
| GA4 | event-level data for **14 months** at most |
| Search Console | a **16-month** rolling window |
| Most vendor APIs | whatever the current plan says, changeable at their discretion |

Those are not outages. They are the products working as designed, quietly, on
a timer. A business that has been trading for nine years can ask its analytics
what happened in 2019 and be told nothing at all — not "no data", just a chart
that starts when the retention window starts.

**A cosmic does not have a retention window.** We collect on a schedule, we keep
what we collect, we back it up, and when a customer outgrows the store they are
in we move them into a bigger one. Nobody gets cut off at fourteen months.

This is the practical end of "the data store is theirs". A copy that gets
deleted on the vendor's timetable is not ownership; it is a longer loan. Keeping
everything is what makes the earlier claim true rather than rhetorical.

### The clock is the reason to start

Every month a source is not connected is a month of that source's data ageing
out of a window it never comes back from. This has already happened on the
first instance: Search Console has never successfully captured a file, and
sixteen months is the whole history it would have had.

**No amount of first-party access recovers it.** What people searched for and
what the search engine decided are the two things that live outside the
property by construction. When that window rolls, the record is simply gone.

Which means a crude scheduled capture today preserves things a perfect one next
quarter cannot. Where the two are in tension, collect first.

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
