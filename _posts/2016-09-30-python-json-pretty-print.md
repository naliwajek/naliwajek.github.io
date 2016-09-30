---
layout: post
title: Pretty print JSON with Python's json.tool
category: Technical Notes
---

Pretty print any JSON using [json.tool](https://docs.python.org/3/library/json.html#module-json.tool) from Python's command line interface for `json` module.

~~~ bash
# Sometimes JSON can be readable when it's quite short
❯ echo '{ "foo": 123, "bar": 345 }'
{ "foo": 123, "bar": 345 }

# But for other just pipe in through json.tool
❯ curl http://api.joind.in | python -m json.tool
{
    "events": "http://api.joind.in/v2.1/events",
    "hot-events": "http://api.joind.in/v2.1/events?filter=hot",
    "upcoming-events": "http://api.joind.in/v2.1/events?...",
    "past-events": "http://api.joind.in/v2.1/events?filter=past",
    "open-cfps": "http://api.joind.in/v2.1/events?filter=cfp",
    "docs": "http://joindin.github.io/joindin-api/"
}
~~~