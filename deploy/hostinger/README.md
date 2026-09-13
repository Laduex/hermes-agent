# Hostinger deployment

Hostinger is an image deployment, not a source-checkout deployment. Build and
test the fork locally, push the exact commit, then build the core image on the
VPS with that commit as `HERMES_GIT_SHA`.

The VPS currently carries the private `hermes-decision-inbox` package as a
runtime-only extension. `Dockerfile.decision-inbox` preserves that package from
the last known-good image while taking the Hermes core from the newly-built
image. This keeps the extension outside Hermes core and avoids treating the
dirty VPS checkout as canonical.

For each release, record the following in the VPS deployment metadata:

- fork commit and upstream base commit;
- core and final image tags plus immutable image IDs;
- Decision Inbox package version;
- Compose revision and patch labels;
- pre-deploy health result and post-deploy health/result;
- rollback image tag and backup locations.

Use the Hostinger operations skill for the guarded workflow: verify the target,
preserve rollback state, reconcile Compose without taking down the whole stack,
and verify health and profile/session routing after replacement.
