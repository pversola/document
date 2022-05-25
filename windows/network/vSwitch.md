```
> Get-NetNat

Name                             : NAT-Network
ExternalIPInterfaceAddressPrefix :
InternalIPInterfaceAddressPrefix : 192.168.200.0/24
IcmpQueryTimeout                 : 30
TcpEstablishedConnectionTimeout  : 1800
TcpTransientConnectionTimeout    : 120
TcpFilteringBehavior             : AddressDependentFiltering
UdpFilteringBehavior             : AddressDependentFiltering
UdpIdleSessionTimeout            : 120
UdpInboundRefresh                : False
Store                            : Local
Active                           : True
```

```
> Get-NetNat | Remove-NetNat

Confirm
Are you sure you want to perform this action?
Performing operation Delete on Target NAT-Network PolicyStore Local
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "Y"): Y
```

```
> New-VMSwitch –SwitchName "NAT_vSwitch" –SwitchType Internal –Verbose
VERBOSE: New-VMSwitch will create a new virtual switch "NAT_vSwitch".
Name        SwitchType NetAdapterInterfaceDescription
----        ---------- ------------------------------
NAT_vSwitch Internal

> Get-VMSwitch

Name           SwitchType NetAdapterInterfaceDescription
----           ---------- ------------------------------
Default Switch Internal
WSL            Internal
NAT_vSwitch    Internal
```

```
> New-NetIPAddress –IPAddress 192.168.200.1 -PrefixLength 24 -InterfaceIndex 17 –Verbose

VERBOSE: Performing operation "New" on Target "NetIPAddress -IPv4Address 192.168.200.1 -InterfaceIndex 106 -Store
Active"
VERBOSE: Performing operation "New" on Target "NetIPAddress -IPv4Address 192.168.200.1 -InterfaceIndex 106 -Store
Persistent"

IPAddress         : 192.168.200.1
InterfaceIndex    : 106
InterfaceAlias    : vEthernet (NAT_vSwitch)
AddressFamily     : IPv4
Type              : Unicast
PrefixLength      : 24
PrefixOrigin      : Manual
SuffixOrigin      : Manual
AddressState      : Tentative
ValidLifetime     : Infinite ([TimeSpan]::MaxValue)
PreferredLifetime : Infinite ([TimeSpan]::MaxValue)
SkipAsSource      : False
PolicyStore       : ActiveStore

IPAddress         : 192.168.200.1
InterfaceIndex    : 106
InterfaceAlias    : vEthernet (NAT_vSwitch)
AddressFamily     : IPv4
Type              : Unicast
PrefixLength      : 24
PrefixOrigin      : Manual
SuffixOrigin      : Manual
AddressState      : Invalid
ValidLifetime     : Infinite ([TimeSpan]::MaxValue)
PreferredLifetime : Infinite ([TimeSpan]::MaxValue)
SkipAsSource      : False
PolicyStore       : PersistentStore

> Get-NetIPAddress -InterfaceAlias "vEthernet (NAT_vSwitch)"


IPAddress         : fe80::4c2f:afc9:c4e1:7cfb%106
InterfaceIndex    : 106
InterfaceAlias    : vEthernet (NAT_vSwitch)
AddressFamily     : IPv6
Type              : Unicast
PrefixLength      : 64
PrefixOrigin      : WellKnown
SuffixOrigin      : Link
AddressState      : Preferred
ValidLifetime     : Infinite ([TimeSpan]::MaxValue)
PreferredLifetime : Infinite ([TimeSpan]::MaxValue)
SkipAsSource      : False
PolicyStore       : ActiveStore

IPAddress         : 192.168.200.1
InterfaceIndex    : 106
InterfaceAlias    : vEthernet (NAT_vSwitch)
AddressFamily     : IPv4
Type              : Unicast
PrefixLength      : 24
PrefixOrigin      : Manual
SuffixOrigin      : Manual
AddressState      : Preferred
ValidLifetime     : Infinite ([TimeSpan]::MaxValue)
PreferredLifetime : Infinite ([TimeSpan]::MaxValue)
SkipAsSource      : False
PolicyStore       : ActiveStore
```

```
> New-NetNat –Name NATNetwork –InternalIPInterfaceAddressPrefix 192.168.200.0/24 –Verbose

Name                             : NATNetwork
ExternalIPInterfaceAddressPrefix :
InternalIPInterfaceAddressPrefix : 192.168.200.0/24
IcmpQueryTimeout                 : 30
TcpEstablishedConnectionTimeout  : 1800
TcpTransientConnectionTimeout    : 120
TcpFilteringBehavior             : AddressDependentFiltering
UdpFilteringBehavior             : AddressDependentFiltering
UdpIdleSessionTimeout            : 120
UdpInboundRefresh                : False
Store                            : Local
Active                           : True
```
