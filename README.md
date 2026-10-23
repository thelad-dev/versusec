# sectioned-security-sweep

A reusable agent skill for hunting security holes in a codebase without drowning
in invented findings.

The method is simple:

1. Cut the source into five to ten focused sections (auth, payments, API surface,
   UI, data store, and so on). Each pass looks at one section only. Focus beats
   breadth.
2. A frontier model searches that section at high reasoning depth. If it finds
   nothing worth reporting, the pass ends quietly.
3. If it finds something, a second frontier model from a different vendor reviews
   adversarially, with the stance "you may be overreading this". Only findings
   both models still hold reach a human.
4. Alternate which vendor leads the search across passes. Different models catch
   different holes.

Step 3 is the load-bearing piece. A model told to find bugs will always find
bugs. Without the counter-reader, you get a list of invented findings and stop
looking after two passes.

## Install

Copy `skills/sectioned-security-sweep/` into your agent's skills directory, or
add this repository with your agent's skill installer when one is available.

## Invoke

Ask the agent to run `sectioned-security-sweep` against a project path, or name
the skill in a slash/command invocation if your harness supports that.

Example:

> Run sectioned-security-sweep on ./my-app. Auto-fix off. Cover auth and the
> HTTP API first.

## Auto-fix

The skill finds, cites, and proposes. It never changes code unless you flip an
explicit per-run switch to allow fixes. Default is report-only.

## License

MIT. See [LICENSE](LICENSE).
