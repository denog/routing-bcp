# Filters of AS196610

## Introduction

AS196610 (DE-CIX Academy) uses [Peering Manager](https://peering-manager.readthedocs.io/en/stable/) to configuer its routers (well, currently only one router running CISCO IOS XR).

So all filters are auto-generated using templates. However, all filters contain a generic part filtering out unwanted prefixes, and this one is documented here.

## Explanation of filter

1. We block all RPKI invalid prefixes.
1. This policy is applied to both IPv6 and IPv4 neighbors. Reason: With BGP you can easily announce IPv4 prefixes over an IPv6 session and vice versa. So you cannot know from the type of session what prefixes need to be checked.
1. *ipv4-unwanted* contains the following:
    - prefixes and more specifics of them are blocked
    - no default route
    - no prefixes with first digit zero
    - no loopback ip
    - no prefixes from the [IANA IPv4 reserved prefixes list](https://www.iana.org/assignments/iana-ipv4-special-registry/iana-ipv4-special-registry.xhtml).
    - no multicast IPv4 addresses
    - no addresses of IXP peering LANs my AS is connected to
    - block *my own* prefixes and all more specifics.
1. *ipv6-unwanted* is similar:
    - no default route
    - no link local, ULA etc.
    - no prefixes from the [IANA IPv6 reserved prefixes list](<https://www.iana.org/assignments/iana-ipv6-special-registry/iana-ipv6-special-registry.xhtml>). There is still some adjustment in IPv6 - so check this page regularly and adjust your filter!
    - no prefixes from IXP peering LANs
    - my own prefixes
1. The list of *private-as-numbers* looks cryptic, but thats only because Cisco uses regular expressions for these lists traditionally and not numeric ranges:
    - We check against invalid AS numbers *anywhere* in the AS path
    - no AS0
    - no AS23456 (this was used for the 16- to 32-bit AS transition)
    - no private ASes and no ASes reserved for documentation
    - again this information can be found at the [IANA AS number registry](https://www.iana.org/assignments/as-numbers/as-numbers.xhtml)
