# Needs, deferred from the first instance

Everything here was found while building `cosmic-wtp`, the first instance, and
deliberately **not** solved there. The instance is being built as a one-off so
the customer gets a working site; the general version of each problem is parked
here so it is not lost and not blocking.

Read this as a list of things the platform must eventually do, with the
evidence that says why. Nothing here is designed yet.

---

## 1. A cosmic is a secure key vault

The environment a cosmic runs on has to hold its own keys. Today the first
instance keeps them in Netlify's environment variables, hand-typed, and that
does not survive contact with a second customer.

**Evidence from the first instance.** Fifteen environment variables, set by
hand, of which:

- four pointed at a Supabase project that had been replaced, so every build
  failed and the failure said nothing about why;
- one, `HCP_API_KEY`, was **present with an empty value** — defined enough to
  pass an "is it set?" check and useless at the call;
- one, `ALLOW_PLACEHOLDER_NAP`, silently disabled a build guard **on
  production**, which is precisely the case it exists to stop;
- one, `PRELAUNCH_NOINDEX`, was set and honoured by nothing in the code.

None of that is unusual for hand-maintained configuration. It is what
hand-maintained configuration does. Multiply by eight instances.

**What is needed:** the environment's own secrets provisioned rather than
typed, held in the cosmic, versioned with it, and verifiable — so "is this
environment correctly configured" is a question with an answer.

**Supabase Vault is the obvious home and is not ready.** It is marked `beta`,
and `pgsodium`, the extension it was built on, is pending deprecation. That is
a timing objection rather than a design one; revisit when both settle.

### Two categories, and they do not belong in the same place

This was the mistake made on the first instance: treating every secret as one
kind of thing.

| | Examples | Property | Home |
| :--- | :--- | :--- | :--- |
| **Environment secrets** | the cosmic's own Supabase keys, Maps key, mail key, notify addresses | **regenerable** — lose them and reissue | the cosmic |
| **Access credentials** | registrar, cPanel, SSH, WordPress, Business Profile, Analytics, Search Console, field-service system | **not regenerable by us** — lose them and you ask the customer again | the custodian |

The second category carries grant, consent and revocation semantics. In LEBOSS
terms those are Access Grants and Delegation Chains — governance objects with
an authority chain and an audit record, not application data.

**Why the split is load-bearing, with evidence:** the first instance's Supabase
project was replaced this week. Everything stored in it went with it. Had the
customer's cPanel token and SSH key lived there, they would have had to be
requested again — and asking a customer a second time for access they already
granted is the expensive failure, not the technical one.

**A cosmic being a secure key vault does not mean it holds every key.** It
means it holds the keys to itself.

### The floor is one, not zero

Removing the external custodian entirely is not reachable: reading a secret out
of a cosmic requires a credential to that cosmic, and that one lives elsewhere
by construction. The realistic target is *one* credential outside, not none —
and once a custodian exists for one, keeping the access credentials there costs
almost nothing and buys audit, individual revocation, and survival of the
environment being rebuilt.

---

## 2. Lucky as the first CosmOS service

Lucky is currently a CLI an operator runs. The intended shape is a **service**
supporting cosmic instances, and the reason is that the CLI cannot be where a
customer goes.

**What the service is for:**

- **The customer signs in and hands over keys.** Somewhere that is not an
  operator's terminal and not an email thread. Every credential that arrives by
  phone, text or email is captured but not secured, and the gap between those
  two states is where credentials leak.
- **Or authorises our OAuth app**, so access is granted rather than pasted.
  This is the better path wherever a vendor offers it: nothing secret moves,
  the grant is attributable, and the customer can revoke it themselves without
  changing a password.
- **Consent and withdrawal**, which the roadmap already names as one
  customer-facing surface with two ends.

**It must be branded and recognisable.** A form asking a business owner to
authenticate is structurally identical to a phishing email. The only things
separating them are recognition and domain, which makes the brand a security
control rather than decoration.

**Prefer granting over pasting.** Most of what blocks an engagement is not a
credential we can receive — it is an action only the customer can take in a
console they own. A service can instruct, then verify, then show the light turn
green while they are still looking at it.

Design discussion belongs in `jwogrady/cosmos-docs`.

---

## 3. Provisioning, not configuration

The two above meet here. When a sale closes, standing up a cosmic should be one
operation: create the vault, collect or generate the keys, write the
environment, verify every one of them, and report what is still blocked and on
whom.

**Measured on the first instance:** of thirteen credentials, the tooling could
prove four. The other nine were proved by hand. That number is the cost of
provisioning, and it is paid again for every customer until it is zero.

The environment variables in section 1 are the same problem one level up: they
should be an output of provisioning, not an input typed into a dashboard.

---

## 4. Instance configuration must not be instance-specific code

The first instance's edge function hardcodes vault item IDs for one customer.
It works, and it is a one-cosmic design.

Anything that differs per customer — vault name, item references, domain,
canonical host, nameserver fleet — has to be configuration the instance
carries, not identifiers compiled into shared code. The shared half should be
identical across instances or it is not shared.

---

## 5. Smaller things worth not forgetting

- **A credential that cannot be read must never report as missing.** An
  unreadable reference and an absent credential look identical from the outside
  and have completely different fixes. This cost a day on the first instance.
- **Never collapse *never checked*, *failed*, and *passed six weeks ago* into
  one red light.** Three states, three messages, or people learn to ignore red.
- **Every failure needs to say whose move it is.** Only "blocked on the
  customer" produces a message to the customer, and a stalled round-trip is the
  real cost of provisioning.
- **Anything that verifies must itself be verified.** On the first instance the
  checker was wrong more often than the credentials were: of five initial
  failures, three were defects in the checking, not the thing checked.
