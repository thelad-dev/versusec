# Report format

Keep the human report short. Confirmed findings only in the main list.

## Pass log (per section)

```text
section: <name>
seeker_vendor: <vendor family, not a pinned product string if avoidable>
counter_vendor: <different vendor family, or n/a if clean>
result: clean | findings
seeker_count: <n>
held: <n>
dropped: <n>
```

If `result: clean`, stop. No essay.

## Confirmed finding

```text
### <short title>
severity: critical|high|medium|low
section: <name>
cite: <path:line[, path:line...]>
claim: <what is wrong and who can abuse it>
seeker: <one or two sentences>
counter-reader: hold - <why it survived>
proposed_fix: <smallest change; not applied unless auto_fix on>
```

## Disputed / dropped (optional appendix)

Only if the invoker wants an audit trail. Never mix into confirmed findings.

```text
- dropped (<verdict>): <seeker title> @ <cite>
```

## Project summary

After all requested sections:

- sections run / clean / with held findings
- confirmed count
- `auto_fix` state
- remaining sections not yet run
