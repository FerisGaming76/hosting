1. SCRIPT MONITORING USER HOTSPOT
   
    On Login :
   
    :put (",,0,,,,Disable,");
    :local mac $"mac-address";
    :local Mwp [/ip hotspot user get [find name="$user"] uptime];
    :local dvc [/ip dhcp-server lease get [find mac-address="$mac"] host-name];
    :local Mpkt [/ip hotspot user get [find name="$user"] profile];
    :local datetime "$[/system clock get date] $[/system clock get time]";
    :local Md [/ip hotspot user get [find name="$user"] bytes-in];
    :local Mu [/ip hotspot user get [find name="$user"] bytes-out];
    :local limit [/ip hotspot user get [find name="$user"] limit-bytes-total];
    :local totq [(($limit)/1024)];
    :local Mt [((($Md)+($Mu))/1048576)];
    :local sisa [($totq-($Md+$Mu)/1048576)];
    :local exp [/ip hotspot user get [find name="$user"] comment];
    :local Ma [/ip hotspot active print count-only];
    :tool fetch url="https://api.telegram.org/bot(TOKEN BOT)/sendMessage?chat_id=(ID TELEGRAM)&text====================================%0A\F0\9F\9B\85         MONITORING USER LOG-IN        \F0\9F\9B\85%0A===================================%0A\F0\9F\94\B9 Voucher  : $user%0A\F0\9F\94\B9 Package : $Mpkt%0A\F0\9F\94\B9 Device : $dvc%0A\F0\9F\94\B9 IP Address : $address%0A\F0\9F\94\B9 Mac Phone : $mac%0A\F0\9F\94\B9 Login : $datetime%0A\F0\9F\94\B9 Used Time : $Mwp%0A\F0\9F\94\B9 Speed Up : $Mu%0A\F0\9F\94\B9 Speed Down : $Md%0A\F0\9F\94\B9 Used Quota: $Mt Mb / $totq Gb%0A\F0\9F\94\B9 expired : $exp%0A\F0\9F\94\B9 User Online : $Ma Users%0A===================================%0A            (NAMA OWNER)%0A===================================" mode=http keep-result=no;
    
    On Logout:
   
    :local nama "$user";
    :local exp [/ip hotspot user get [find name="$nama"] comment];
    :local Ma [/ip hotspot active print count-only];
    :local datetime "$[/system clock get time] $[/system clock get date]";
    :local mac $"mac-address";
    :set mac [:ip dhcp-server lease get [:ip dhcp-server lease find mac-address="$mac"] mac-address];
    :local host [/ip dhcp-server lease get [find address="$host"] host-name];
    :local bin [/ip hotspot user get [find name="$nama"] bytes-in];
    :local bout [/ip hotspot user get [find name="$nama"] bytes-out];
    :local qterpakai [((($bin)+($bout))/1048576)];
    :tool fetch url="https://api.telegram.org/(TOKEN BOT)/sendMessage?chat_id=(ID TELEGRAM)&text==================================%0A\F0\9F\93\B4   MONITORING USER LOG-OUT   \F0\9F\93\B4%0A=================================%0A\F0\9F\94\B8 Voucher        : $user%0A\F0\9F\94\B8 IP                  : $address%0A\F0\9F\94\B8 MAC              : $mac%0A\F0\9F\94\B8 Device           : $host%0A\F0\9F\94\B8 Used Quota   : $qterpakai Mb%0A\F0\9F\94\B8 Active Time  : $exp%0A\F0\9F\94\B8 User Active   : $Ma Users%0A===================================%0A            (NAMA OWNER)%0A===================================" keep-result=no;





2. SCRIPT MONITORING PPPOE SERVER
   
    On Login :
   
    :local nama "$user";
    :local bot "(TOKEN BOT)";
    :local chat "(ID TELEGRAM)";
    :local ips [/ppp active get [find name=$nama] address];
    :local up [/ppp active get [find name=$nama] uptime];
    :local caller [/ppp active get [find name=$nama] caller-id];
    :local service [/ppp active get [find name=$nama] service];
    :local active [/ppp active print count];
    :local datetime1 "Tanggal: $[/system clock get date]";
    :local datetime2 "Jam: $[/system clock get time]";
    :local lastdisc [/ppp secret get [find name=$user] last-disconnect-reason];
    :local lastlogout [/ppp secret get [find name=$user] last-logged-out];
    :local lastcall [/ppp secret get [find name=$user] last-caller-id];
    /tool fetch url="https://api.telegram.org/bot$bot/sendMessage?chat_id=$chat&text==================================%0A\F0\9F\9B\85 MONITORING USER PPPOE \F0\9F\9B\85%0A=================================%0A\E2\9C\85 PPPoE LOGIN%0A\F0\9F\94\B9 $datetime1%0A\F0\9F\94\B9 $datetime2%0A\F0\9F\94\B9 User: $user%0A\F0\9F\94\B9 IP Client: $ips%0A\F0\9F\94\B9 Caller ID: $caller%0A\F0\9F\94\B9 Uptime: $up%0A\F0\9F\94\B9 Total Active: $active Client%0A\F0\9F\94\B9 Service: $service%0A\F0\9F\94\B9 Last Disconnect Reason: $lastdisc %0A\F0\9F\94\B9 Last Logout: $lastlogout %0A\F0\9F\94\B9 Last Caller ID: $lastcall%0A=================================" mode=http keep-result=no;
    
    On Logout:
   
    :local bot "(TOKEN BOT)";
    :local chat "(ID TELEGRAM)";
    :local lastdisc [/ppp secret get [find name=$user] last-disconnect-reason];
    :local lastlogout [/ppp secret get [find name=$user] last-logged-out];
    :local lastcall [/ppp secret get [find name=$user] last-caller-id];
    :local active [/ppp active print count];
    :local datetime1 "Tanggal: $[/system clock get date]";
    :local datetime2 "Jam: $[/system clock get time]";
    /tool fetch url="https://api.telegram.org/bot$bot/sendmessage\?chat_id=$chat&text==================================%0A\F0\9F\93\B4 MONITORING USER PPPOE \F0\9F\93\B4%0A=================================%0A\E2\9D\8CPPPOE-LOGOUT %0A\F0\9F\94\B8 $datetime1%0A\F0\9F\94\B8 $datetime2%0A\F0\9F\94\B8 USER: $user%0A\F0\9F\94\B8 Last Disconnect Reason: $lastdisc %0A\F0\9F\94\B8 Last Logout: $lastlogout %0A\F0\9F\94\B8 Last Caller ID: $lastcall %0A\F0\9F\94\B8 Total active: $active Client%0A=================================" keep-result=no;


3. SCRIPT MONITORING NETWACH
   
    On Login :
   
    :local id [/system identity get name]
    :local dat [/system clock get date]
    :local clk [/system clock get time]
    :local hst $host
    :local cmnt [/tool netwatch get value-name=comment [find host=$hst] comment]
    /tool fetch url="https://api.telegram.org/bot(TOKEN BOT)/sendMessage?chat_id=(ID TELEGRAM)&text==================================%0A\F0\9F\9B\85 MONITORING ACCESS POINT \F0\9F\9B\85%0A=================================%0A\F0\9F\94\B9 Identity  : $id %0A\F0\9F\94\B9 Tanggal  : $dat %0A\F0\9F\94\B9 Waktu    : $clk %0A\F0\9F\94\B9 IP/Host  : $hst %0A\F0\9F\94\B9 User      : $cmnt %0A\F0\9F\94\B9 Status    : Acces Point UP \E2\9C\85%0A=================================" keep-result=no
    
    On Logout:
   
    :local id [/system identity get name]
    :local dat [/system clock get date]
    :local clk [/system clock get time]
    :local hst $host
    :local cmnt [/tool netwatch get value-name=comment [find host=$hst] comment]
    /tool fetch url="https://api.telegram.org/bot(TOKEN BOT)/sendMessage?chat_id=(ID TELEGRAM)&text==================================%0A\F0\9F\93\B4 MONITORING ACCESS POINT \F0\9F\93\B4%0A=================================%0A\F0\9F\94\B8 Identity  : $id %0A\F0\9F\94\B8 Tanggal  : $dat %0A\F0\9F\94\B8 Waktu    : $clk %0A\F0\9F\94\B8 IP/Host  : $hst %0A\F0\9F\94\B8 User      : $cmnt %0A\F0\9F\94\B8 Status    : Acces Point DOWN \E2\9D\8C%0A=================================" keep-result=no


3. SCRIPT MONITORING BANDWIDCH  Bandwidth
   
    :local bot "(TOKEN BOT)";
    :local chatID "(ID TELEGRAM)";
    :local varDate;
    :local varDay;
    :local INT ether1;
    :local TOTQuota 2000;
    :local Router [/system identity get name];
    :local RXByteCur [/interface get $INT rx-byte];
    :local TXByteCur [/interface get $INT tx-byte];
    :local RXTot ($RXByte+$RXByteCur);
    :local RXMB ($RXTot / 1024 / 1024);
    :local RXGB ($RXTot  / 1024 / 1024 / 1024);
    :local TXTot ($TXByte+$TXByteCur);
    :local TXMB ($TXTot / 1024 / 1024);
    :local TXGB ($TXTot / 1024 / 1024 / 1024);
    :local RXTX ($RXTot+$TXTot);
    :local RXTXMB ($RXMB+$TXMB);
    :local RXTXGB ($RXGB+$TXGB);
    :local percent ($RXTXGB*100 / $TOTQuota);
    :local datetime1 "$[/system clock get date]";
    :local datetime2 "$[/system clock get time]";
    :set varDate [/system clock get date];
    :set varDay [:pick $varDate 4 6];
    /tool fetch url="https://api.telegram.org/bot$bot/sendMessage\?chat_id=$chatID&text====================================%0A\F0\9F\8C\90         BANDWIDTH MONITORING        \F0\9F\8C\90%0A===================================%0A\F0\9F\93\86 Tanggal        = $datetime1%0A\F0\9F\95\90 Jam             = $datetime2%0A\F0\9F\91\A4 Identity        = $Router%0A\F0\9F\93\A1 Interface       = $INT%0A\F0\9F\94\BA Live TX        = $TXByteCur kbps%0A\F0\9F\94\BB Live RX       = $RXByteCur kbps%0A\F0\9F\94\BA Total TX       = $TXGB GB / $TXMB MB%0A\F0\9F\94\BB Total RX      = $RXGB GB / $RXMB MB%0A\F0\9F\93\9F Total TX/RX = $RXTXGB GB / $RXTXMB MB%0A\F0\9F\93\B6 Used Quota   = $percent% from $TOTQuota GB%0A===================================%0A            Rasendria Network Solutions%0A===================================" mode=http keep-result=no;
    
