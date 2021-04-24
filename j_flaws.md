---
layout: post
title: Security Flaws
description: 
image:
nav-menu: true
---

## Session Management Flaws
* Session cookies and authentication tokens unprotected (e.g. clear text) between client and server.
* Missing session invalidation at idle-timeout and user logout.
* New session token to prevent re-use for authentication.
* Non-secure storage in a session in cleartext.
* Lack of strong random generation of session cookies and identifiers.
* Lack of coordinated session between application tiers.

## Authentication Flaws
* Mandatory to restrict access to validated users.
* Strength depends on application risk/data classification.
* Compliant with regulations/standards.
* Provide for secure password and account management.
* Mitigates brute-forcing and credentials harvesting.
* Mitigates Man in The Middle Attacks (MiTM).

## Authorization Flaws
* [Flaws in Role-Based Access Controls (RBAC)](https://www.ionos.com/digitalguide/server/security/what-is-role-based-access-control-rbac/)
* [Flaws regarding horizontal and vertical privilege escalation & forced browsing.](https://portswigger.net/web-security/access-control)
