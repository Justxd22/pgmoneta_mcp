# RPM Support

This directory contains the necessary files to build an RPM package for **pgmoneta-mcp**.

## Building

### Using `rpmbuild`

To build the RPM package locally, you need `rpm-build` and `rust` installed.

1. Prepare the environment:
   ```bash
   mkdir -p rpmbuild/{BUILD,RPMS,SOURCES,SPECS,SRPMS}
   ```

2. Update the version in the spec file and create the source tarball:
   ```bash
   # Assuming you are in the project root
   VERSION=$(grep '^version =' Cargo.toml | head -n 1 | cut -d '"' -f 2)
   sed -i "s/^Version:.*/Version:        $VERSION/" contrib/rpm/pgmoneta-mcp.spec

   tar --exclude=rpmbuild --transform "s|^|pgmoneta-mcp-$VERSION/|" -czf rpmbuild/SOURCES/v$VERSION.tar.gz .
   ```

3. Build the RPM:
   ```bash
   rpmbuild --define "_topdir $(pwd)/rpmbuild" \
            --define "_unitdir /usr/lib/systemd/system" \
            --define "_bindir /usr/bin" \
            --define "_sysconfdir /etc" \
            -bb contrib/rpm/pgmoneta-mcp.spec
   ```

The generated RPMs will be in `rpmbuild/RPMS`.

## Installation

Install the generated RPM using `dnf` or `rpm`:

```bash
sudo dnf install rpmbuild/RPMS/x86_64/pgmoneta-mcp-<version>-1.x86_64.rpm
```

## Configuration

The configuration files are located at:
- `/etc/pgmoneta-mcp/pgmoneta-mcp.conf`: Main configuration.
- `/etc/pgmoneta-mcp/pgmoneta-mcp-users.conf`: User configuration.

## Running

Enable and start the service using `systemctl`:

```bash
sudo systemctl enable --now pgmoneta-mcp
```

Check the status:

```bash
sudo systemctl status pgmoneta-mcp
```
