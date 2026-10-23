# GoblinInn mail transport policy

Public, static MTA-STS policy only. This repository contains no marketplace source, mail contents, credentials, or customer information.

Canonical endpoint: https://mta-sts.goblininn.com/.well-known/mta-sts.txt

## Transition policy

The policy deliberately permits the existing Tuta MX and both SimpleLogin MX hosts during migration. `mode: enforce` remains enabled and `max_age` remains 86400 seconds.

Publishing this policy alone does not change inbound email routing. The domain's MX records control routing.

## Deployment requirements

- Publish the repository root, including `.well-known`, using GitHub Pages. `.nojekyll` prevents the static file from being omitted by Jekyll.
- Verify domain ownership for the correct GitHub account before moving the policy hostname.
- Configure the custom hostname and verify its HTTPS certificate and HTTP 200 response containing this exact text file, without authentication or redirects.
- Publish a new `_mta-sts` TXT policy ID only after the HTTPS endpoint serves the new policy correctly. A CNAME and TXT record cannot coexist at `_mta-sts`.
- Record publication times and DNS TTLs. Preserve the old Tuta mail route until the policy update can be discovered and the relevant old caches have expired. Use a conservative full old-policy cache window if cache propagation cannot be established.
- Verify the destination aliases, DNS authentication, and rollback route before switching MX to SimpleLogin.
- After successful receive/reply tests and expiry of the old MX caches, remove the retired Tuta MX from this policy, publish a new policy ID, and retire obsolete Tuta domain configuration in a coordinated step.

Do not remove this repository, disable Pages, or change its custom hostname while DNS still points to it. Keep domain ownership verification in place to prevent takeover.

Reference: RFC 8461, especially sections 3.3, 5.1 and 8.1.
