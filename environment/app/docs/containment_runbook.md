# Post-Rollout Containment Runbook

Required hardened state for resolver hosts touched by the failed DNS zone-failover rollout. The failover automation provisioned itself with persistence and escalation paths that must be removed during containment; the reconciler restore does not begin until the host matches this runbook.

## SSH persistence

The failover automation appended its deploy key to root's `authorized_keys`, identified by the key comment `zone-failover-automation`. Revoke it: no entry carrying that comment may remain in `/root/.ssh/authorized_keys`. Other entries in the file, if any, are legitimate and stay.

## Privilege escalation

The rollout installed a temporary sudoers drop-in at `/etc/sudoers.d/zone-failover` granting `svc-named` passwordless root. Remove the file entirely — do not comment it out or narrow it.

## Key exposure

The TSIG zone-transfer key at `/app/secrets/zonetransfer_tsig.key` was written world-readable. The key itself remains valid and the resolver still needs it in place: keep the file, owned `root:root`, mode `0600`.
