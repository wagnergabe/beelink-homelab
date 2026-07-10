# Troubleshooting Notes

This page documents selected issues encountered during the home-lab build and the approach used to diagnose and resolve them.

## Tailscale Authentication and DNS Conflict

### Symptom

The Windows desktop could not authenticate to Tailscale. The service reported that it could not resolve the Tailscale control-plane hostname, while basic DNS lookups from a terminal sometimes succeeded.

### Investigation

- Confirmed the Tailscale service was running.
- Compared command-line DNS resolution with service-level behavior.
- Identified a competing VPN/WireGuard adapter and local DNS proxy.
- Verified that disabling the competing VPN restored Tailscale authentication and connectivity.

### Validation

- Confirmed both endpoints appeared online in Tailscale.
- Verified encrypted access to the virtualization host using a TCP connectivity test.
- Confirmed no router port forwarding was needed.

### Takeaway

DNS can behave differently across adapters, VPN clients, and Windows services. A successful `nslookup` alone does not guarantee that a background service can resolve or reach the same destination.

## Samba Share and Windows Mapping

### Goal

Make shared storage accessible from Windows while retaining a consistent Linux path for server-side services.

### Approach

- Configured a Samba share on the virtualization host.
- Created a dedicated service account for the share.
- Validated the Samba configuration before testing from Windows.
- Mapped the share as a persistent Windows drive.

### Validation

- Confirmed the share was visible in Windows File Explorer.
- Created and viewed folders from Windows.
- Confirmed the same files were available from the server-side storage path.

### Takeaway

Testing both the server configuration and the Windows client path helps isolate whether an issue is caused by Samba, credentials, permissions, or Windows discovery.

## External Storage and LXC Permissions

### Challenge

The external storage uses an exFAT filesystem and is passed into an unprivileged LXC container. exFAT does not support standard Linux ownership and permission behavior in the same way as ext4.

### Approach

- Mounted the drive using explicit user and group mapping options.
- Passed the host storage path into the LXC using a bind mount.
- Used a shared service account and group strategy for Samba and container workloads.
- Verified access from both the host and the container.

### Takeaway

Filesystem capabilities matter. Permission design has to account for the limitations of the underlying storage format, especially when combining Samba, containers, and bind mounts.
