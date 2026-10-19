# Contributing

Thank you for taking the time. The EyalSec repositories hold product
documentation and demonstration apps, not product source, so they take
**issues and fixes to documentation or to the demo apps**. The reports you send
are what improve the products, and we read every one.

## What helps most

| You have | Do this |
|---|---|
| A flow that should have been reported and was not | Open an issue. A ten-line program that reproduces it is the most useful thing you can send. |
| An event you believe is wrong, or noisy | Open an issue with the event JSON. |
| A library, driver or operation you want covered | Open an issue. Say which call, and what untrusted data reaches it in your app. |
| A mistake in a README, an example or a demo app | Open an issue, or a pull request with the fix. |
| A question about using a product | Ask in [Discussions](https://github.com/EyalSec/es-python/discussions), or email support@eyalsec.com. |
| A security vulnerability in EyalSec itself | **Do not open an issue.** See [SECURITY.md](https://github.com/EyalSec/.github/blob/main/SECURITY.md). |
| A question about your plan or account | sales@eyalsec.com |

## Before you post: three things to leave out

Issues here are public.

1. **Credentials.** Never paste an install command (its token is a
   credential for ten minutes), an API key, a password or a session cookie.
2. **Other people's data.** An event carries the value that reached the
   operation, taken from your program's memory. Read the event JSON before you
   paste it, and replace anything personal or secret with `[redacted]`.
3. **Unfixed vulnerabilities in someone else's project.** If a product
   showed you a flaw in a third-party library, report it to that project
   privately first. Describe the shape here, not the exploit.

## A good bug report

- which product and version, and the distribution and architecture
  (`uname -m`, and the `ID` and `VERSION_ID` lines of `/etc/os-release`);
- for es-python, the output of `es-python -V` and of `eyalsec info`, and which
  sources were on for the machine;
- the smallest program or page that shows the problem;
- what you expected and what happened;
- the event JSON (`GET /api/event/{id}`, see the
  [API guide](https://eyalsec.com/docs/api-events#event-detail)), or the sink
  label and severity as the dashboard shows them.

Before filing "nothing is reported", read
[No events](https://eyalsec.com/docs/troubleshooting#no-events); it settles
most cases in a minute.

## What happens next

We reply on the issue. If we can reproduce it, we say so and label it, and we
close it when a release carries the fix. If we cannot reproduce it, we ask one
question at a time until we can. If we decide not to change something, we say
why.

## Conduct

Everyone taking part is expected to follow the
[Code of Conduct](https://github.com/EyalSec/.github/blob/main/CODE_OF_CONDUCT.md).
