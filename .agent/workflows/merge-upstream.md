---
description: How to merge upstream ruff changes into our fork
---
// turbo-all

# Merge Upstream Ruff Changes

## Prerequisites
- Ensure upstream remote is configured:
  ```
  git remote add upstream https://github.com/astral-sh/ruff.git
  ```

## Steps

1. Fetch upstream changes:
```
git fetch upstream
```

2. Show what changed in ty_server upstream since our last merge:
```
git log --oneline HEAD..upstream/main -- crates/ty_server/ | head -30
```

3. Attempt merge:
```
git merge upstream/main --no-edit
```

4. If merge conflicts occur, check which files conflict:
```
git diff --name-only --diff-filter=U
```

5. For each conflicting file in `crates/ty_server/`:
   - Our changes are ONLY visibility modifications (`pub(crate)` → `pub`).
   - Accept upstream's logic changes, then re-apply our visibility widening.
   - Rule: if upstream kept `pub(crate)`, change it to `pub`. If upstream changed the signature/logic, keep upstream's logic but ensure visibility is `pub`.

6. After resolving conflicts, verify the build:
```
cargo check -p ty_server
```

7. Run ty_server tests:
```
cargo test -p ty_server
```

8. Check for new `pub(crate)` items that should be exposed:
```
grep -rn 'pub(crate)' crates/ty_server/src/session.rs crates/ty_server/src/server.rs crates/ty_server/src/server/api/traits.rs | head -20
```

9. If upstream added new files in `crates/ty_server/src/server/api/requests/`, ensure new handler structs are `pub`:
```
grep -rn 'pub(crate) struct' crates/ty_server/src/server/api/requests/
```

10. Commit the merge:
```
git add -A && git commit -m "merge: upstream ruff $(git log upstream/main -1 --format=%h)"
```

11. Run the `apk-impact` workflow to check if APK needs updates.
