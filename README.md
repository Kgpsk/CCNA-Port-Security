# CCNA-Port-Security
! ============================================
## PORT SECURITY LAB - FULL COMMAND SUMMARY
! ============================================

# ---------- STEP 1: Basic Setup ----------
enable
configure terminal
hostname SW1
no ip domain-lookup

# ---------- STEP 2: Access Port for PC0 ----------
interface fa0/1
 switchport mode access
 switchport access vlan 1
 no shutdown
 exit

# ---------- STEP 3: Enable Port Security ----------
interface fa0/1
 switchport port-security
 switchport port-security maximum 1
 switchport port-security violation shutdown
 switchport port-security mac-address sticky
 exit

# ---------- STEP 4: Verify Initial State ----------
show port-security interface fa0/1
show port-security
show mac address-table interface fa0/1
show interfaces fa0/1 status

# ---------- STEP 5: Test Legit Connectivity ----------
 From PC0:
ping 192.168.1.11

# ---------- STEP 6: Trigger Violation ----------
 Packet Tracer:
 PC0 -> Config -> FastEthernet0 -> change MAC to 00E0.1111.2222
! PC0 -> Desktop -> Command Prompt -> ping 192.168.1.11

# ---------- STEP 7: Verify Violation ----------
show port-security interface fa0/1
show port-security
show interfaces fa0/1 status

# ---------- STEP 8: Recovery (Manual) ----------
configure terminal
interface fa0/1
 shutdown
 no shutdown
 end

# ---------- STEP 9: Clear Sticky / Counter ----------
clear port-security sticky interface fa0/1
clear port-security counter interface fa0/1
clear port-security all

# ---------- STEP 10: Verify Recovery ----------
show port-security interface fa0/1
show interfaces fa0/1 status

# ---------- STEP 11: Optional Auto-Recovery ----------
configure terminal
errdisable recovery cause psecure-violation
errdisable recovery interval 300
end

# ---------- VERIFICATION COMMANDS ----------
show port-security interface fa0/1
show port-security
show port-security address
show mac address-table interface fa0/1
show interfaces fa0/1 status
show errdisable recovery
