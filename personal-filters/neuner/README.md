# BGP sanity checks for IOS XR

Some sanity checks for IOS XR, adapted/simplified from BelWü, AS553. Insert your own prefixes.

## features
 - removes "our" communities
 - drops martians (private/reserved/etc.) prefixes
 - drops bogon ASNs
 - drops "our" prefixes from peers
 - drops prefixes from peering LANs and peering links
 - split up in different parts to add flexibility (e.g. add community tagging in various places)

## structure
 1) community-set, prefix-sets are defined
 2) route policies for those things are defined
 3) the sanity-check policy is defined, which uses the above ones

## usage
 - insert your own communities, prefixes, peering-LANs
 - "pass" as default policy for route-policies for readablity, use with caution
