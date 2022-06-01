set policy-options policy-statement FROM-DIRECT-1 from protocol direct
set policy-options policy-statement FROM-DIRECT-1 then accept

set routing-instances ROUTER1 protocols rip group GRUPA1 export FROM-DIRECT-1

set routing-instances ROUTER1 protocols rip group GRUPA1 neighbor ge-0/0/2.173
set routing-instances ROUTER1 protocols rip group GRUPA1 neighbor ge-0/0/2.172

set policy-options policy-statement FROM-RIP-1 from protocol rip
set policy-options policy-statement FROM-RIP-1 then accept

set routing-instances ROUTER1 protocols rip group GRUPA1 export FROM-RIP-1

show | compare