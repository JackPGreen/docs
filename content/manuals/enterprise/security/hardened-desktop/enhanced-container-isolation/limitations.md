---
title: Limitations
description: Limitations of Enhanced Container Isolation
keywords: enhanced container isolation, security, sysbox, known issues, Docker Desktop
toc_max: 2
weight: 50
aliases:
 - /security/for-admins/hardened-desktop/enhanced-container-isolation/limitations/
---

### ECI support for WSL

> [!NOTE]
>
> Docker Desktop requires WSL 2 version 2.1.5 or later. To get the current
> version of WSL on your host, type `wsl --version`. If the command fails or if
> it returns a version number prior to 2.1.5, update WSL to the latest version
> by typing `wsl --update` in a Windows command or PowerShell terminal.

ECI on WSL is not as secure as on Hyper-V because:

- While ECI on WSL still hardens containers so that malicious workloads can't
  easily breach Docker Desktop's Linux VM, ECI on WSL can't prevent Docker
  Desktop users from breaching the Docker Desktop Linux VM. Such users can
  trivially access that VM (as root) with the `wsl -d docker-desktop` command,
  and use that access to modify Docker Engine settings inside the VM. This gives
  Docker Desktop users control of the Docker Desktop VM and lets them bypass Docker Desktop configs set by administrators via the
  [settings-management](../settings-management/_index.md) feature. In contrast,
  ECI on Hyper-V does not let Docker Desktop users to breach the Docker
  Desktop Linux VM.

- With WSL 2, all WSL 2 distributions on the same Windows host share the same instance
  of the Linux kernel. As a result, Docker Desktop can't ensure the integrity of
  the kernel in the Docker Desktop Linux VM since another WSL 2 distribution could
  modify shared kernel settings. In contrast, when using Hyper-V, the Docker
  Desktop Linux VM has a dedicated kernel that is solely under the control of
  Docker Desktop.

The following table summarizes this.

| Security feature                                   | ECI on WSL   | ECI on Hyper-V   | Comment               |
| -------------------------------------------------- | ------------ | ---------------- | --------------------- |
| Strongly secure containers                         | Yes          | Yes              | Makes it harder for malicious container workloads to breach the Docker Desktop Linux VM and host. |
| Docker Desktop Linux VM protected from user access | No           | Yes              | On WSL, users can access Docker Engine directly or bypass Docker Desktop security settings. |
| Docker Desktop Linux VM has a dedicated kernel     | No           | Yes              | On WSL, Docker Desktop can't guarantee the integrity of kernel level configs. |

In general, using ECI with Hyper-V is more secure than with WSL 2. But WSL 2
offers advantages for performance and resource utilization on the host machine,
and it's an excellent way for users to run their favorite Linux distribution on
Windows hosts and access Docker from within.

### ECI protection for Docker builds with the "docker" driver

Prior to Docker Desktop 4.30, `docker build` commands that use the buildx
`docker` driver (the default) are not protected by ECI, in other words the build runs
rootful inside the Docker Desktop VM.

Starting with Docker Desktop 4.30, `docker build` commands that use the buildx
`docker` driver are protected by ECI, except when Docker Desktop is configured to use WSL 2
(on Windows hosts).

Note that `docker build` commands that use the `docker-container` driver are
always protected by ECI. 

### Docker Build and Buildx have some restrictions

With ECI enabled, Docker build `--network=host` and Docker Buildx entitlements
(`network.host`, `security.insecure`) are not allowed. Builds that require
these won't work properly.

### Kubernetes pods are not yet protected

When using the Docker Desktop integrated Kubernetes, pods are not yet protected
by ECI. Therefore a malicious or privileged pod can compromise the Docker
Desktop Linux VM and bypass security controls.

As an alternative, you can use the [K8s.io KinD](https://kind.sigs.k8s.io/) tool
with ECI. In this case, each Kubernetes node runs inside an ECI-protected
container, thereby more strongly isolating the Kubernetes cluster away from the
underlying Docker Desktop Linux VM (and Docker Engine within). No special
arrangements are needed, just enable ECI and run the KinD tool as usual.

### Extension containers are not yet protected

Extension containers are also not yet protected by ECI. Ensure you extension
containers come from trusted entities to avoid issues.

### Docker Debug containers are not yet protected

[Docker Debug](/reference/cli/docker/debug.md) containers
are not yet protected by ECI. 

### Native Windows containers are not supported

ECI only works when Docker Desktop is in Linux containers mode (the default,
most common mode). It's not supported when Docker Desktop is configured in
native Windows containers mode (i.e., it's not supported on Windows hosts, when
Docker Desktop is switched from its default Linux mode to native Windows mode).

### Use in production

In general users should not experience differences between running a container
in Docker Desktop with ECI enabled, which uses the Sysbox runtime, and running
that same container in production, through the standard OCI `runc` runtime.

However in some cases, typically when running advanced or privileged workloads in
containers, users may experience some differences. In particular, the container
may run with ECI but not with `runc`, or vice-versa.
