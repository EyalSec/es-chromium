![EyalSec es-chromium - a browser that sees XSS](https://raw.githubusercontent.com/EyalSec/es-chromium/main/banner.png)

# es-chromium

**A browser that tracks attacker controlled data as you browse and reports DOM-XSS as it happens.**

[![Website](https://img.shields.io/badge/website-eyalsec.com-e8b04b)](https://eyalsec.com)
[![Docs](https://img.shields.io/badge/docs-user%20guide-2a2c27)](https://eyalsec.com/docs)
[![Demo](https://img.shields.io/badge/demo-vulnerable--javascript-2a2c27)](https://github.com/EyalSec/vulnerable-javascript)
[![OpenSSF Scorecard](https://api.scorecard.dev/projects/github.com/EyalSec/es-chromium/badge)](https://scorecard.dev/viewer/?uri=github.com/EyalSec/es-chromium)

`es-chromium` is the EyalSec browser. You browse the application you are
testing with it; no JavaScript is injected into the page, no extension is
installed, and nothing is asked of the site. When data from a source an attacker can influence reaches a dangerous DOM
operation, the whole flow is reported to your dashboard: the source, the sink,
and the exact characters that were tainted.

**Who it is for:** penetration testers and AppSec teams testing web
applications for DOM-based cross-site scripting. It is a testing browser, not an
everyday one: it reports the page URLs you visit and the values that reached a
sink.

## Install and run

Once es-chromium is enabled on your account
([book a live demo](https://eyalsec.com/contact)), choose **es-chromium** under
**Product** on the **Machines** page, name the PC, click **Add machine**, then
**Install**. Copy the one-line command from the panel (one-time token, valid
for 10 minutes), run it in a terminal on that PC, then start the browser:

```bash
es-chromium http://your-target.example/
```

A finding reads as a flow on the es-chromium list of your events page, source
then sink, followed by the page it happened on:

```
location.hash -> Element.innerHTML
```

Full guide: **[es-chromium in the user guide](https://eyalsec.com/docs/es-chromium)**.

## How it works

- **Sources**: the places a page takes input from. The URL, `postMessage`,
  `window.name`, the referrer, cookies, web storage, fetched responses.
- **Sinks**: the operations that can turn that input into markup, code or
  navigation. `innerHTML`, `document.write`, `eval`, script URLs, attribute
  writes.
- **A finding** is a flow that really happened in the page you browsed, from
  the input to the sink that used it, with the attacker-controlled characters
  marked. Page weaknesses with no flow (a missing Content-Security-Policy, a
  cookie without `Secure`) are reported separately as posture.

## What it catches

DOM-XSS in the shapes it takes on real pages:

- URL fragment and query string into markup sinks (`innerHTML`, `outerHTML`,
  `insertAdjacentHTML`, `document.write`, `DOMParser`)
- `postMessage` from an embedded third party widget, rendered without an
  `event.origin` check
- Round trips through `localStorage`, `sessionStorage`, `document.cookie` and
  `window.name`, where the value is written on one visit and rendered on the next
- Dynamically constructed script and iframe URLs, and `setAttribute`, which is
  where event handler attributes get set
- `eval`, `new Function` and the string form of `setTimeout`, compiled from page state
- Request and connection targets built from page state: `fetch`,
  `XMLHttpRequest.open`, `WebSocket`, `EventSource`
- Navigation built from `document.referrer` or the URL
- Flows that cross into a shadow root

## Coverage you can re-run

[`EyalSec/vulnerable-javascript`](https://github.com/EyalSec/vulnerable-javascript)
is a deliberately vulnerable retail dashboard, hosted at a real origin, in which
**21 flows across 8 taint sources and 18 distinct sinks** are each driven by a
single URL and each observed firing. The harness that drives them is committed
alongside it, so the claim can be re-run. Every flow advertised here is one you
can watch fire.

## How it differs from a scanner

A scanner reads the page, or crawls it, from outside the session, so it reasons
about which sinks could be reached with which input. That is useful work, and
it is done without the third party widgets loaded, the saved theme in local
storage, or the cookie the last visit wrote.

`es-chromium` is the browser. It sees the page as your browser sees it, with
the widgets loaded and the last visit's state in place, so it reports the flow
that happened. That is why its findings come with the payload attached.

## Why the output is readable

- **The report marks the tainted span, not just the value.** The reported
  string wraps the attacker controlled characters in two invisible markers, so
  a flow into a large chunk of markup shows exactly which part came from the
  attacker rather than leaving you to work it out.
- **Posture findings are separated from flows.** Missing CSP, missing
  `X-Frame-Options` and the rest are reported as posture, with no flow attached,
  and they never masquerade as an injection.

## Scope

Measured on the release of 2026-08-04.

- **Taint is followed through the string operations pages use most:**
  concatenation, template literals, `slice`/`substring`, `trim`, `padStart`,
  `replace`, `JSON` round trips, percent encoding and splitting on a separator.
  Case folding (`toUpperCase`/`toLowerCase`), `atob`, and decomposing a string
  to individual characters and rejoining it end the trail, so a flow through
  one of them is not reported. The three cases are catalogued in the demo app
  so every release re-checks them.
- **A sanitized value is still reported as a flow**, because the taint survives
  the sanitizer. The event shows you the flow; you judge whether the sanitizer
  made it safe.
- **Response bodies are treated as attacker-influenced**, the right threat
  model for DOM-XSS, so an application that renders API data into the DOM
  reports those flows too and a data heavy site produces more of them.
- **It follows copied data.** A value selected by a branch rather than copied,
  or carried through a side channel, leaves no trail to follow.
- **It reports, and rules decide what you see.** Rules show or hide findings
  on the dashboard.
- **Runs on Linux x86_64.**

## Getting it

**[Book a live demo](https://eyalsec.com/contact)** and we run it on your own
application with you. There is no self-serve plan and no trial. `es-chromium`
arrives through the EyalSec dashboard, enabled per account as part of a plan.

## Learn more

- **[Documentation](https://eyalsec.com/docs)**: install, read events, write rules
- **[Research](https://eyalsec.com/research)**: write-ups from EyalSec
- **[Vulnerability guides](https://eyalsec.com/vulnerabilities)**: injection
  classes explained, with fixes

## Support and security

- Questions and problems: **support@eyalsec.com**, or an
  [issue](https://github.com/EyalSec/es-chromium/issues) here (see
  [CONTRIBUTING.md](CONTRIBUTING.md)).
- A vulnerability in EyalSec itself: **security@eyalsec.com**, under our
  [disclosure policy](https://eyalsec.com/legal/vulnerability-disclosure). See
  [SECURITY.md](SECURITY.md). es-chromium is built for security testing
  rather than everyday browsing; the policy says what that means for a report.

This repository is the public home and documentation for `es-chromium`; the
browser itself arrives through the dashboard.
