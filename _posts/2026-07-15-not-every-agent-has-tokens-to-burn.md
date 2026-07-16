---
layout: post
title:  "Not Every Agent Has Tokens to Burn"
date:   2026-07-15 06:39:32 -0400

categories: Musings AI LLM
---

I read a post called [Designing APIs for Agents](https://www.freestyle.sh/blog/opinion/designing-apis-for-agents) recently, about how you should design an API when the thing calling it is an LLM agent instead of a person. A lot of it I agree with. Name your fields something unambiguous (`displayName` vs `name` vs `slug`), fail loudly instead of quietly coercing `"1"` into `true`, stop worrying about progressive disclosure in your docs since an agent just reads the whole spec at once anyway. All fine points.

One line stuck with me though: "Defaults are bad." The reasoning is that an agent reads the entire API surface before it ever calls anything, so the whole reason defaults exist (saving a human from having to read every option) doesn't apply anymore. The agent already knows every field is there. So just make everything explicit and get rid of defaults.

I get where that's coming from, but I don't think it holds up once you think about who's actually paying for the call.

"The agent already read the whole spec" assumes that agent has a huge context window and someone else's API budget to burn. That's a fair assumption if you're building on whatever frontier model came out this month. It's a lot less fair if you're running something smaller, something cost constrained, or an agent that's going to make the exact same call five hundred times in one workflow.

Take a Stripe checkout session. `automatic_payment_methods.enabled` defaults to `true`, and that default is documented in their API reference. If I strip that default out and force every caller to spell it out, I've added one more field to every single request, forever. Do that across a workflow creating hundreds of sessions and you're paying, in tokens, in latency, and in one more chance for the agent to get a value wrong, just to restate the same thing over and over.

Sitting with this a bit longer, I think "defaults are bad" is really two separate claims wearing one sentence. First, that undocumented magic behavior is bad. Second, that because of that, an agent should never rely on a default and everything should be spelled out. I agree completely with the first one, for humans and agents both. The second one doesn't actually follow from it though.

A default that's documented and doesn't change is just as legible to an agent as writing the value out by hand. The agent read the docs either way, so it already knows what happens if it leaves the field out. What actually trips an agent up isn't "a default exists," it's when that default is undocumented, or when a field's meaning shifts depending on context. Those are the real failure modes, and neither one has much to do with whether a default exists.

So I'd rewrite the advice. Don't get rid of defaults, get rid of undocumented ones. Make sure every field means exactly one thing. Do that and a default saves you tokens on every call without costing you any clarity. Treating it as explicit and expensive versus implicit and vague is where the original point goes sideways for me.

Worth mentioning too, this advice comes from a company that sells sandboxed VM execution, and their whole pitch is "don't build an abstraction, just call exec directly." An API with more explicit fields and fewer defaults is a pretty convenient world if that's what you're selling. Doesn't make the naming or error handling advice wrong, but it's worth noticing whose incentives are baked into a post like this before adopting it wholesale.

If you're designing an API that agents are going to call, the token budget of whatever's calling it is part of the design now, same as latency and rate limits always were. Document your defaults, keep every field meaning one thing, and let the agent override what it actually needs to. That gets you explicit where it matters and cheap everywhere else, instead of picking one at the cost of the other.
