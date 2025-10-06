# Release Process

## Creating a new release

1. **Update version in Cargo.toml**

   ```bash
   # Edit Cargo.toml and update the version number
   version = "0.1.0" → version = "0.2.0"
   ```

2. **Commit and tag**

   ```bash
   git add Cargo.toml
   git commit -m "chore: bump version to 0.2.0"
   git tag v0.2.0
   git push origin main
   git push origin v0.2.0
   ```

3. **GitHub Actions will automatically:**
   - Build binaries for macOS (Intel & ARM) and Linux (x86_64 & ARM64)
   - Create a GitHub release with all artifacts
   - Attach `.tar.gz` files for each platform

## Testing locally

```bash
# Build and test the binary
cargo build --release
./target/release/ut --help
./target/release/ut base64 encode "test"
```

## Quick release script

You can automate with:

```bash
#!/bin/bash
VERSION=$1
if [ -z "$VERSION" ]; then
  echo "Usage: ./release.sh 0.2.0"
  exit 1
fi

# Update Cargo.toml
sed -i '' "s/version = \".*\"/version = \"$VERSION\"/" Cargo.toml

# Commit and tag
git add Cargo.toml
git commit -m "chore: bump version to $VERSION"
git tag "v$VERSION"
git push origin main
git push origin "v$VERSION"

echo "Release v$VERSION pushed. Check GitHub Actions for build status."
```
