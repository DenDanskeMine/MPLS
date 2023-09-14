



- router bgp 65400
- neighbor 1.1.1.1 remote-as 65400
- update-source lo 1
- no auto-summary
- address family vpnv4 # Router protocol bruger vpn ver 4 prefix
- neighbor activate 4.4.4.4