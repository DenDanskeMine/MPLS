


# Konfig af BGP / VPNv4

Dette kode bruger man til at konfigurer VPNv4 med BGP (IBGP)
```
router bgp 65400
neighbor 1.1.1.1 remote-as 65400
update-source lo 1
no auto-summary
address family vpnv4 # Router protocol bruger vpn ver 4 prefix
neighbor activate 4.4.4.4
```
Som man kan se på As numret så er det IBGP vi bruger, da vores nabo har samme AS nummer.

Koden øverest er ikke meget værd uden VRF. Dette konfigurer vi på routeren flere steder.
Vi starter med at angive navnet, på den virksomhed, der ønsker at have VPN'en:

``` 
ip vrf kunde-1
rd 65400:1
route-target both 65400:1
```
### ip vrf kunde-1:
 ```ip vrf kunde-1``` fortæller hvad vi ønsker kunden skal "hedde".<br>
 Dette kommer vi også til at bruge når vi skal forward det, og sende det igennem OSPF og BGP.
### rd 65400:1
``` rd 65400:1 ``` er det unikke ID vi ønsker at give til virksomheden.<br>
Dette ID vil isolere virksomhedens VRF fra andres. <br>
Dette tillader også at virksomhederne kan bruge samme IP, uden at der opstår en IP conflict.

ID'et kan i princippet være lige det du ønsker. Her har jeg dog valgt at have det AS nummret på, samt deres kundenummer (1)
Så bliver det mere overskuligt når man skalere det.

### route-target both 65400:1
Route-target kan have 3 værdiger Import, export, og Both. - De forklare sig selv i navnet.<br>
**Export** exportere sin tabel til andre, i det samme RT.<br>
**Import** impotere tabeller fra andre, i det samme RT<br>
!!! RT OG RD er ikke det samme, og ID'ET er ikke det "samme"!!! De er blot ens for at det er mere overskuligt, at vedligeholde og se sammenhæng i.

# Hvad er VPNv4 i BGP?

VPNV4 er enelig bare en protocol / funktion der gør det muligt at lave en VPN tunnel over et offentligt netværk der køere over BGP.
Hele iden med det er at det er en vigtig komponet til **MPLS,** som gør det nemt at lave scalerebare VPN'er

# Naboskab

Man kan se om naboskabet, med vpnv4 er oppe ved at skrive:
```
 sh bgp vpnv4 unicast all summary
```

# VRF Forwarding og deling af routings tabeller.
### OSPF > BGP
### BGP > OSPF

