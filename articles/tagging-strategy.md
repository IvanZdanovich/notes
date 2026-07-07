# The Tagging Strategy That Needs No Tags: Filter Tests by Structure, Not Labels

Every test suite eventually drowns in tags. `@smoke`, `@regression`, `@critical` — they multiply until no one remembers
what belongs where. The common advice says tag everything so you can slice runs any way you like. That advice quietly
creates the mess it promises to solve.

Here is the myth: tags make selection flexible. In practice, a tag encodes one person's judgment at one moment. `@smoke`
means whatever the author felt was important that day, and when priorities shift, the label lies — no framework will
warn you.

Think of tags like sticky notes on moving boxes. Move a box and the note stays put unless you peel it off and rewrite it
by hand. Move a test to another suite and its `@regression` tag doesn't follow the intent — you edit it manually, one
file at a time. Miss one, and a stale tag silently drops the test from a run that needed it. That isn't flexibility;
it's untracked debt, and it drives testers to invent ever-narrower tags to patch the ambiguity.

## Filter by what a test actually is

Tests already carry stable facts: what they exercise and how. Select on those instead of on opinion — clear functional
categories such as API, UI, E2E, or component tests. These don't drift, because they describe mechanics, not priority.
Embed the type in the file name (`login.api.spec.ts`, `checkout.e2e.spec.ts`) and your runner filters by path. Move the
file, and its classification moves with it automatically.

A few labels still earn their place — `@negative`, `@performance`, `@accessibility` — because they mark *kinds* of
verification that cut across the structure. Use them sparingly, not as a substitute for organizing the suite.

So before adding a tag, ask whether the folder structure and file name already answer the question. Usually they do. Let
where a test lives decide when it runs, and the sticky notes take care of themselves.