![Ansible Lint](https://github.com/johanneskastl/ansible-role-opensuse_zypper_repos/workflows/Ansible%20Lint/badge.svg)

# opensuse_zypper_repos

Configure (multiple) zypper repository definitions on SUSE/openSUSE

## Requirements

This obviously is meant to run on a SUSE or openSUSE system using zypper. As it only creates repository definitions, but does not install any packages, immutable systems like openSUSE MicroOS, SLE Micro, Leap Micro, Aeon or Kalpa are suitable.

## Role Variables

There is just one variable called `list_of_zypper_repos`. This variable can contain one or more repository definitions.

Each repository definition must contain values for:

- `name` (String): short name, to be used as file name and as repo name in brackets in the repository definition
- `baseurl` (String): URL of the repository (can be http:// or https://)

Using this basic definition, the repository definition file will be created in `/etc/zypp/repos.d/` with a file name matching the `name` variable. The repository will be **enabled**, autorefresh will be **enabled**, too.

```
[my-repo]
enabled=1
autorefresh=1
baseurl=https://example.org/pub/my-repo/
```

If you need to tweak the definition, you can add each of the variables, if desired:

- `long_name` (String): Description used inside the definition (this will appear as `Name` in `zypper lr -EP`)$
- `enabled` (Integer 0 or 1): whether this repository should be enabled (`1`) or disabled (`0`). This settings defaults to `1` if unset.
- `autorefresh` (Integer 0 or 1): whether auto-refreshing of the repository should be enabled (`1`) or disabled (`0`). This settings defaults to `1` if unset.
- `gpgcheck` (Integer 0 or 1): whether the signature on the repository's metadata should be checked using a GPG key
- `gpgkey` (String): URL to the GPG key that signed the repository metadata
- `priority` (Integer): an integer specifying the repository's priority (make sure to read up on zypper's handling of priorities)
- `path` (String): In case you need to define a path, normally this is omitted or set to `/`
- `type` (String): Normally this is not set, but it could be e.g. `rpm-md`
- `keeppackages` (Integer 0 or 1):  whether RPM file caching should be enabled (`1`) or disabled (`0`)

## Dependencies

None

## Example Playbook

```yaml
- hosts: servers
  roles:
    - role: 'johanneskastl.opensuse_zypper_repos'
      list_of_zypper_repos:
        - name: 'repo-oss'
          long_name: 'openSUSE-Tumbleweed-Oss'
          baseurl: 'http://download.opensuse.org/tumbleweed/repo/oss/'
          path: '/'
          type: 'rpm-md'
          keeppackages: 0
```

## License

BSD-3-Clause

## Author Information

I am Johannes Kastl, reachable via git@johannes-kastl.de
