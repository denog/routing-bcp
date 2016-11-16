basic route filters
===================

Use these route filters as first element in a chain of route policies to drop the bad stuff.


Usage
-----
Clone the repo::
    
    git clone https://github.com/denog/routing-bcp.git
    cd routing-bcp/personal-filters/fischa

Replace the ASN variable with your real ASN:

* search for "$YOUR_ASN" in reject-bad-routes-v4 and reject-bad-routes-v6
* replace with your real ASN (e.g. 65000)

Replace the prefix variable with your real prefix (read RIPE allocation):

* search for "$YOUR_PREFIX in reject-bad-routes-v4 and reject-bad-routes-v6
* replace with your real prefix (e.g. 192.0.2.0/24 or 2001:db8::/32)
* check if you're connected to more than DE-CIX and adapt the prefix-list accordingly

Upload the files to your router::

    scp reject-bad-routes-v4 user@router:
    scp reject-bad-routes-v6 user@router:

Login to router and load config::

    ssh user@router
    configure
    load merge reject-bad-routes-v4
    load merge reject-bad-routes-v6

Verify config and apply::

    show | compare
    commit check
    commit and-quit

Now you can use the filter at the beginning of your policy chain either below the peergroup or neighbor::

    configure
    edit protocols bgp group $some_peergroup neighbor $some_neighbor
    set import [ reject-bad-routes-v4 $some_policy $some_other_policy ]

    edit protocols bgp group $some_peergroup
    set import [ reject-bad-routes-v4 $some_policy $some_other_policy ]

NOTE: Remember to check if you got the right policy for IPv4 or IPv6 applied. Otherwise it won't work.
