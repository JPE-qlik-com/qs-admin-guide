---
layout: default
title: Analyze Security Rules
nav_order: 2
grand_parent: Asset Management
parent: Security Rules
---

# Analyze Security Rules <i class="fas fa-dolly-flatbed fa-xs" title="Shipped | Native Capability"></i>  <i class="fas fa-tools fa-xs" title="Tooling | Pre-Built Solutions"></i>
{: .no_toc }

## Applicable Environments
{:.no_toc}
- All

## Goal
{:.no_toc}
The goal of this section is to analyze security rules for quality--ultimately increasing readability, performance, and scalability. This page will explore two options of doing so--one natively through the QMC (manual), and the other via an application created named the **qs-security-rules-analyzer**.

For auditing, please refer to [Audit User Access](../../audit/audit_user_access.md).

## Table of Contents
{:.no_toc}

* TOC
{:toc}
-------------------------

## Quality

When writing security rules, it is essential to think about how they will scale and how easy it is for them to be read and understood by others. There are of course times when rules _must_ become quite complex and might be rather difficult to read, but generally, these should be niche scenarios for very granular requirements.


### Readability

When writing a security rule, try to break up the formatting into multiple lines, to make the rule more easily digested by a human being. For example, here is a perfectly valid rule:

```
(resource.resourcetype = "App" and resource.stream.HasPrivilege("read") and resource.@AppLevelMgmt.empty()) or ((resource.resourcetype = "App.Object" and resource.published = "true" and resource.objectType != "app_appscript") and resource.app.stream.HasPrivilege("read"))
```

Now, here is that same rule, formatted differently:

```
(
    resource.resourcetype = "App" 
    and resource.stream.HasPrivilege("read") 
    and resource.@AppLevelMgmt.empty() 
) 
or
( 
    (
        resource.resourcetype = "App.Object" 
        and resource.published = "true" 
        and resource.objectType != "app_appscript" 
    ) 
    and resource.app.stream.HasPrivilege("read") 
)
```

The second is surely far easier to read.

Lastly, include a good _description_ of the security rule. It is common to simply write out the pseudocode.

### Scale

When writing a rule, one must ensure that it can scale. While `((user.name="Rodion Romanovich Raskolnikov") or (user.name="Sofya Semyonovna Marmeladov"))` works for a couple of users, as more and more users come into the system, that 1:1 rule style quickly becomes very difficult to maintain. Also, now imagine that Mr. Raskolnikov now changes roles, and should no longer see the resource that the security rule applies to. One would have to first know that he changed roles, and second, go remove him from that security rule. This simply is not maintainable. Instead, _groups_ should be used, be that `user.group` or `user.environment.group` or `user.@SomeCustomGroup`. This 1:many style rule is highly scalable, and is generally hands off, especially when using the former two options, as they are dynamic from the source, vs hardcoded as a custom property.

To summarize, instead of writing this:

```
((user.name="Rodion Romanovich Raskolnikov") or (user.name="Sofya Semyonovna Marmeladov"))
```

Write something like this, where the resource has a hardcoded custom property value assigned to it, and that matches up against a dynamic value from the _group_ property, say, from Active Directory. This rule is then completely hands off, and scales to many users dynamically.

```
((resource.@Department=user.group))
```

## QMC - Security Rules <i class="fas fa-dolly-flatbed fa-xs" title="Shipped | Native Capability"></i>

In the QMC, select **Security Rules**:
