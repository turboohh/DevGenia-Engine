# DevGenia 0.11.0 — beta preparation

This release strengthens account administration, delivery and recovery while preserving the existing gameplay and frame pacing.

- Staff actions require a recently confirmed authenticator code or unused recovery code. Set up your own authenticator at `/account/staff-security`; being an owner alone no longer unlocks staff tools. Recovery codes should be stored privately offline.
- Privacy, family help and getting-started pages link to private support requests. A private-profile shortcut hides connections and presence and disables new party invitations. It does not disable game chat.
- Player shows the installed application version and checks the signed release feed when its home window opens. Studio has Help → Updates & repair. Application updates require confirmation; published game versions still load when joining. Older applications without the feed need this installer once.
- The release manifest is signed with a separately stored Ed25519 key and the public key is pinned in the application. This does not replace a Windows Authenticode publisher certificate or guarantee the absence of SmartScreen warnings.
- Optional independent encrypted backups and local health monitoring have explicit restore checks and conservative capacity budgets. They do not buy storage or automatically scale the server.
- DevGenia+ payment handling now covers paid renewals, weekly grants, cancellation, failure and duplicate events. Stripe and new membership checkout stay disabled by default until real provider testing and activation are complete.
- A read-only launch inventory and three reproducible starter-game scenarios make outstanding checks visible.

The beta is not certified for an unrestricted public or paid launch. Verified parental consent/age assurance, reviewed legal/operator details, actual staff MFA enrollment, provider activation, independent assessment and outside beta testing remain separate requirements. See PUBLIC_LAUNCH_CHECKS.md and PUBLIC_BETA_READINESS.md for evidence and limits.
