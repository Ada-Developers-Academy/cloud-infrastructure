# Backup & Disaster Recovery

## Learning Goals

- Describe patterns and types of tools available to support backup and disaster recovery in the cloud

## Key Terms

## What is disaster recovery

## Common backup & recovery tools

### Snapshot solutions

easy to schedule & set policies for, 

### Disaster recovery solutions 

continuously updates replica in one or more regions

## Solution Tradeoffs
network gateway for backing up from on-prem to cloud

Creating snapshots as backups (generally across regions and accounts), that can be restored vs keeping an up to date replica that can be quickly promoted in case of disaster in one or more regions
- May have individual tools and/or a centralized service for backing up across cloud offerings
- snapshots: data loss may happen in disaster if last snapshot scheduled was not right before disaster.
- replicas: very little data loss, but more cost overhead of data use 
Certain applications may be tolerant of some data loss (losing some likes on a post) while others are mission critical (records of orders yet to be fulfilled)

## Check for Understanding

Given a set of needs, would snapshotting be acceptable or is a stronger replication strategy required?

Given "x" feature, which resources would we need to include in a backup plan to ensure we don't lose significant time or data in a disaster event?