# Kubernetes-REST-Deprecated

Permanent home for CPAN redirect ("tombstone") stub modules covering
[Kubernetes::REST](https://metacpan.org/pod/Kubernetes::REST) modules that
were removed when it dropped its old v0 API.

PAUSE has no delete, and it indexes per **module name**, not per
distribution: once a module name is indexed against a release, it stays
there forever unless a *higher*-versioned release of that module name takes
it over. When a Kubernetes::REST module is removed within the
distribution's own later releases, the old name would otherwise keep
resolving to stale, superseded code with no hint a replacement exists. This
distribution ships a tiny stub package under each OLD name with a hand-set
`$VERSION` higher than the last release that shipped that name, taking over
the PAUSE index entry. The stub does nothing but `die` immediately on load
with a message naming the replacement.

## Current tombstones

### The old per-endpoint v0 API (1012 classes)

Kubernetes::REST's `1.000` "v1 rewrite" (2026-02-13) replaced its entire old
per-endpoint v0 API -- one generated `Call::*` class per API operation,
across every API group and version, plus 10 v0-only helper modules -- with
a single unified object-oriented API directly on the
[Kubernetes::REST](https://metacpan.org/pod/Kubernetes::REST) object
(`list`, `get`, `create`, `update`, `patch`, `delete`, `watch`, ...). Every
one of these 1012 classes shipped as a soft-deprecated warning stub (`warn`,
not `die`) continuously from `1.000` through `1.104` -- none were real
classes in that series, they only emitted a deprecation warning on load.
`1.104` was the last CPAN release to ship them; all 1012 were then dropped
from the distribution outright. Full list:
`lib/Kubernetes/REST/Deprecated.pm`'s POD (grouped summary by
apiVersion/group) or `t/01-tombstones.t`'s `@deprecated_classes` array (the
exhaustive canonical list).

### Considered but not tombstoned

The `Kubernetes::REST::V0Group` family (`V0Group` itself plus 17
subclasses: `Core`, `Apps`, `Batch`, `Networking`, `RbacAuthorization`, and
others) reads as deprecated in its own `ABSTRACT`/POD, same as the 1012
above -- but unlike them it is a working `AUTOLOAD`-based compatibility
shim that still translates old-style calls (e.g.
`$api->Core->ListNamespacedPod(...)`) into real calls against the new API,
and `Kubernetes::REST` itself still wires up `$api->Core` / `$api->Apps`
etc. as live accessors. No tombstone needed -- see
`lib/Kubernetes/REST/Deprecated.pm`'s POD for the full note.

## Adding a new tombstone

See the `kubernetes-rest-deprecated` skill
(`.claude/skills/kubernetes-rest-deprecated/SKILL.md`) for the step-by-step
procedure the next time a Kubernetes::REST module is renamed or removed --
including how to tell a genuine orphan apart from a class that's merely
soft-deprecated in place (still shipping) or, like the `V0Group` family,
deprecated in name only while still functional.

## License

This library is free software; you can redistribute it and/or modify it under
the same terms as Perl itself.
