![EyalSec es-chromium - a browser that sees XSS](https://raw.githubusercontent.com/EyalSec/es-chromium/main/banner.png)

# es-chromium

**A browser that tracks attacker controlled data as you browse and reports DOM-XSS as it happens.**

[![Website](https://img.shields.io/badge/website-eyalsec.com-e8b04b)](https://eyalsec.com)
[![Docs](https://img.shields.io/badge/docs-user%20guide-2a2c27)](https://eyalsec.com/docs)
[![Demo](https://img.shields.io/badge/demo-vulnerable--javascript-2a2c27)](https://github.com/EyalSec/vulnerable-javascript)

`es-chromium` is a browser from [EyalSec](https://eyalsec.com). The tracking is
part of the browser itself; no JavaScript is injected into the page, no
extension is installed, and nothing is asked of the site. You browse an
application with it, and
when data from a source an attacker can influence reaches a dangerous DOM
operation, the whole flow is reported to your dashboard: the source, the sink,
and the exact characters that were tainted.

It is the browser counterpart of
[`es-python`](https://github.com/EyalSec/es-python), and it answers the same
question one layer out: not "could this be exploitable" but "did attacker
controlled data actually get there".

## What it catches

DOM-XSS in the shapes it really occurs in, rather than in the shapes a test
suite likes:

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

## Coverage

The detector registry declares **241 detectors**: 135 taint, 57 posture and 49
behavioral, of which 169 are armed by default.

That is what is declared. What is **demonstrated** is the number that matters,
and it is published rather than asserted:
[`EyalSec/vulnerable-javascript`](https://github.com/EyalSec/vulnerable-javascript)
is a deliberately vulnerable retail dashboard, hosted at a real origin, in which
**21 flows across 8 taint sources and 18 distinct sinks** are each driven by a
single URL and each observed firing. The harness that drives them is committed
alongside it, so any claim here can be re-run against any build:

```bash
./run-eschromium.sh /path/to/es-chromium https://eyalsec.github.io/vulnerable-javascript/
```

A detector that is declared but not demonstrated is not advertised here as
coverage.

## How it differs from a scanner

A scanner reads the page, or crawls it, and reasons about which sinks might be
reachable with which input. It has to guess about reachability, and it guesses
without the third party widgets loaded, without the saved theme in local
storage, and without the cookie the last visit wrote.

`es-chromium` is the browser. It sees the page as the user's browser sees it,
with everything loaded and every previous visit's state in place, and it reports
a flow that happened. That is why its findings come with the payload attached.

## Why the output is readable

- **The report marks the tainted span, not just the value.** The reported
  string wraps the attacker controlled characters in two invisible markers, so
  a flow into a large chunk of markup shows exactly which part came from the
  attacker rather than leaving you to work it out.
- **Posture findings are separated from flows.** Missing CSP, missing
  `X-Frame-Options` and the rest are reported as posture, with no flow attached,
  and they never masquerade as an injection.
- **Telemetry is not a finding.** A source being read, with no sink and no
  flow, is not shown as a security event.

## Limitations

Stated here rather than discovered later. All of these were measured against
the build published 2026-08-04.

- **Taint does not survive every string operation.** It is preserved through
  concatenation, template literals, `slice`/`substring`, `trim`, `padStart`,
  `replace`, `JSON` round trips, percent encoding and splitting on a separator.
  It is **lost** through case folding (`toUpperCase`/`toLowerCase`), through
  `atob`, and through decomposing a string to individual characters and
  rejoining it. A page that lower cases a parameter before rendering it is
  therefore a real flow that goes unreported. The three cases are catalogued in
  the demo app so any future build re-checks them.
- **It is sanitizer blind.** A value passed through a sanitizer before
  `innerHTML` is still reported, because the taint survives the sanitizer. The
  finding is correct about the flow and wrong about the risk.
- **Response bodies are tainted unconditionally**, so an application that
  renders API data into the DOM reports flows that are working as intended.
  That is the right threat model for DOM-XSS and it is also why a data heavy
  site is noisier than a simple one.
- **A taint tracker cannot see what it cannot follow.** A value selected by a
  branch rather than copied, or carried through a side channel, leaves no trail.
- **Linux x86-64 today.**

`es-chromium` is a research prototype in active development. False positive
rate and performance have not been measured across a broad sample of real
sites.

## See it working

**[EyalSec/vulnerable-javascript](https://github.com/EyalSec/vulnerable-javascript)**
is a deliberately vulnerable single page dashboard, live at a real HTTPS origin.
Open it in stock Chrome and it is a normal application that quietly runs the
payload. Open the same unchanged page in `es-chromium` and every flow is
reported, with the source, the sink and the payload.

## Getting it

`es-chromium` is delivered through the EyalSec dashboard rather than downloaded
from here. Sign in at [eyalsec.com](https://eyalsec.com), add a machine of kind
*browser*, and run the one line installer it gives you. The build reports to
your dashboard using that machine's token.

Full guide: **[eyalsec.com/docs](https://eyalsec.com/docs)**.

## About this repository

This repository is the public home and documentation for `es-chromium`. The
browser itself is the product and is delivered through the dashboard; there is
nothing to build here. Start at **[eyalsec.com](https://eyalsec.com)**.

---

Chromium is a project of The Chromium Authors. EyalSec is not affiliated with
or endorsed by Google or the Chromium project.
