# Fork-Sync-All integration and collaboration proposal

## Status

This document is a proposal for discussion. It does not imply an existing
partnership, endorsement, maintainer commitment, or change to NativeLink's
project governance. Any implementation in NativeLink should be reviewed and
approved by NativeLink maintainers through the normal contribution process.

## Summary

The Fork-Sync-All (FSA) project proposes an optional integration with
NativeLink to provide distributed remote execution and shared build caching
for eligible repository-maintenance, validation, and build workloads.

- NativeLink upstream: https://github.com/harper-carroll/nativelink
- NativeLink fork maintained by the proposing party:
  https://github.com/Interested-Deving-1896/nativelink
- Fork-Sync-All: https://github.com/Interested-Deving-1896/fork-sync-all

NativeLink provides remote execution and build-cache services compatible with
the Remote Execution API ecosystem. Fork-Sync-All orchestrates Git-platform
automation, repository synchronization, validation, and CI workflows. The
proposed integration would connect these complementary responsibilities
without making either project dependent on the other.

## Proposed collaboration goals

1. Define a documented, opt-in integration profile for FSA jobs that can use
   NativeLink remote execution and/or its remote action cache.
2. Keep the integration protocol-oriented and provider-neutral. NativeLink
   should remain an optional execution/cache backend rather than becoming a
   hard-coded requirement in FSA.
3. Let repository owners choose local execution, existing CI runners, or a
   configured NativeLink endpoint according to workload, trust level, and
   resource availability.
4. Establish a maintainer-reviewed path for compatibility testing, operational
   documentation, and future implementation contributions.

## Proposed integration architecture

FSA would remain responsible for orchestration and policy:

- selecting eligible workflows and jobs;
- resolving repository, branch, commit, and workflow context;
- applying trust, approval, concurrency, quota, and secret-handling rules;
- dispatching work to an execution backend;
- collecting status, logs, artifacts, and provenance.

NativeLink would remain responsible for the remote build/execution services
that it already supports:

- Remote Execution API-compatible task execution;
- remote action caching and content-addressable storage;
- worker scheduling and execution isolation according to NativeLink's
  supported configuration;
- returning execution results to the calling build client.

The integration should use supported NativeLink interfaces and established
build-tool clients. FSA should not invent a parallel execution protocol or
require changes to NativeLink's core service merely to accommodate FSA.

## Suggested implementation phases

### Phase 1 — Design and compatibility review

- Confirm the supported NativeLink API and client configuration expected for
  the first integration.
- Identify a small, reproducible FSA workload suitable for remote execution.
- Document endpoint configuration, authentication, TLS, cache namespaces,
  worker requirements, and failure behavior.
- Agree on security boundaries and which workloads must remain on trusted
  runners.

### Phase 2 — Optional FSA adapter

- Add an opt-in, provider-neutral remote-execution adapter in FSA.
- Keep endpoint credentials in the existing secret-management mechanism;
  never write credentials into repository files, logs, artifacts, or PR text.
- Support explicit fallback to local or existing runner execution when the
  remote service is unavailable, subject to workflow policy.
- Record backend identity, execution result, cache outcome, and relevant
  provenance without exposing secrets.

### Phase 3 — Pilot and validation

- Start with non-privileged, reproducible build or test jobs.
- Compare correctness against local execution before evaluating performance.
- Test cache isolation, untrusted pull-request behavior, cancellation,
  retries, timeouts, and service-unavailable scenarios.
- Measure queue time, execution time, cache hit rate, resource consumption,
  and cost where those metrics are available.

### Phase 4 — Documentation and wider adoption

- Publish an opt-in setup guide and reference configuration.
- Add CI coverage and compatibility checks for the adapter.
- Expand to additional workflows only after maintainers and repository owners
  approve the security and operational model.

## Security and governance principles

- Remote execution must be opt-in; no existing FSA workflow should silently
  move to a remote service.
- Secrets and privileged deployment jobs must not be exposed to untrusted
  workers or untrusted pull-request code.
- Cache namespaces and credentials must be scoped to the trust boundary of
  the workload.
- The integration must respect NativeLink's supported security model and
  FSA's existing approval, quota, and workflow-governance controls.
- No changes to NativeLink's release process, roadmap, branding, ownership,
  or governance are requested by this proposal.
- NativeLink maintainers retain full discretion over whether to accept,
  modify, defer, or decline any proposed NativeLink-side contribution.

## Collaboration requested

We would welcome maintainer feedback on:

1. Whether this integration direction is useful and technically appropriate.
2. Which NativeLink-supported client/API path should be the initial target.
3. Whether maintainers would prefer an FSA-side adapter first, a small
   NativeLink-side documentation contribution, or a jointly reviewed
   compatibility prototype.
4. The appropriate contribution, review, and communication process.

The immediate request is for design feedback and a collaboration discussion.
It is not a request to merge this proposal as an endorsement or to commit
NativeLink maintainers to implementation work.

## Related repositories

- [NativeLink upstream](https://github.com/harper-carroll/nativelink)
- [Interested-Deving-1896 NativeLink fork](https://github.com/Interested-Deving-1896/nativelink)
- [Fork-Sync-All](https://github.com/Interested-Deving-1896/fork-sync-all)
