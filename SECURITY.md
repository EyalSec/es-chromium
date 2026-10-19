# Security policy

Found a security problem in EyalSec? Thank you. Please report it privately,
not in a public issue or discussion.

**Report to: security@eyalsec.com**

The full policy, including scope, testing rules and the safe harbour for
good-faith research, is at
**[eyalsec.com/legal/vulnerability-disclosure](https://eyalsec.com/legal/vulnerability-disclosure)**.
This file is a summary; where they differ, the policy wins.

## What to send

- what the issue is and where (URL, endpoint, product and version, platform);
- clear steps to reproduce, and a proof of concept if you have one;
- the impact you believe it has;
- whether anyone else already knows about it;
- how you would like to be credited, if at all.

English or Hebrew are both fine. If you want to encrypt your report, say so
and we will arrange a key.

## What you can expect

| Stage | Our commitment |
|---|---|
| Acknowledgement | within 3 business days |
| Initial assessment | within 10 business days |
| Progress updates | at least every 14 days while the issue is open |
| Fix target | critical and high severity: 30 days; medium and low: the next reasonable release |
| Credit | public credit if you want it, anonymity if you prefer |

We ask you to keep the issue confidential for 90 days from your report, or
until a fix is published, whichever is sooner. We do not run a paid bug bounty,
and we will not ask you to sign a non-disclosure agreement to report.

## In scope

The EyalSec website and dashboard (`eyalsec.com`), the APIs, the products as
published by us, and the installer that delivers them. Test only against your
own account, your own data and your own installations.

es-chromium is built for security testing rather than everyday browsing, and
its defensive posture is set for that job. A report that it differs from a
mainstream consumer browser describes that design. A specific, exploitable
flaw in our own functionality is in scope and we want it.

## Not a vulnerability in EyalSec

- **A missed detection or a false positive** is a product issue and very
  welcome: open an issue with the bug report form, or email eyal@eyalsec.com.
- **A vulnerability es-python or es-chromium showed you in someone else's
  software** belongs to that project's maintainers. Report it to them
  privately. Please do not post it here.

## Supported versions

Fixes ship in the current published release. Reinstall a machine from your
dashboard to take it: see
[Reinstalling and updating](https://eyalsec.com/docs/running-es-python#update).
