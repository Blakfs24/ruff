---
description: Analyze impact of upstream ruff changes on the APK project
---

# APK Impact Analysis After Upstream Merge

Run this after merging upstream ruff changes to assess impact on APK.

## Steps

1. Check if handler trait signatures changed:
```
git diff @{1}..HEAD -- crates/ty_server/src/server/api/traits.rs
```

2. Check if Session API changed:
```
git diff @{1}..HEAD -- crates/ty_server/src/session.rs | head -100
```

3. Check if new request handlers were added:
```
git diff @{1}..HEAD --name-status -- crates/ty_server/src/server/api/requests/
```

4. Check if completion handler changed (critical for APK):
```
git diff @{1}..HEAD -- crates/ty_server/src/server/api/requests/completion.rs
```

5. Build APK against the updated fork (run from APK repo root):
```
cargo check -p apk
```

6. If compile errors occur, common fixes:
   - Session method signature changed → update APK call site
   - Handler trait changed → update APK handler delegation
   - Type renamed → update APK imports

7. Run APK tests (run from APK repo root):
```
cargo test -p apk
```
