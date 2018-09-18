This repository contains unique packet samples which were narrowed down with help of TSHARK and AFL corpus minimization tool. These files can be used as seed input for fuzzing various packet parsing programs (tshark/wireshark etc).

The samples were gathered as follows:

* Download known samples from wireshark and other captures website (use similar commands for other repositories) and clean up any non pcap files:
```
1) Download wireshark samples
wget --random-wait -e robots=off -nH -l 100000 -A pcap,cap,pcapng,cap.gz,pcap.gz,pcapng.gz -r --user-agent="Mozilla/5.0 (Macintosh; Intel Mac OS X 10.8; rv:21.0) Gecko/20100101 Firefox/21.0" https://wiki.wireshark.org/SampleCaptures

2) Download packetlife samples
wget --random-wait -e robots=off -nH -A pcap,cap,pcapng -l 100000 -r --user-agent="Mozilla/5.0 (Macintosh; Intel Mac OS X 10.8; rv:21.0) Gecko/20100101 Firefox/21.0" http://packetlife.net/captures/

3) Download ICS-pcap samples  
https://github.com/automayt/ICS-pcap
```

* Split samples into single packet files
```
for i in *; do editcap -c 1 $i split-input-clean/$(md5sum $f | cut -d " " -f 1); done
```

* Use AFL to gather unique samples by minimizing the corpus down: 
```
/opt/afl-2.52b/afl-cmin -i input-caps/ -o split-input-clean/ -t 9000 -m 99999999999999999 ./tshark -nVxr @@
```

* The following data sets were minimized using above method:
```
#Wireshark Packet Samples
https://wiki.wireshark.org/SampleCaptures

#PacketLife
http://packetlife.net/captures/

#ICS-pcap repository (git lfs)
https://github.com/automayt/ICS-pcap
```

# Stats

**Stats - Wireshark SampleCaptures PCAPS (2302179 packets minimized to 2944 samples)**

The following stats were observed from AFL's corpus minimization tool:

```
[*] Testing the target binary...
[+] OK, 13588 tuples recorded.
[*] Obtaining traces for input files in '/opt/uniq-wireshark/'...
    Processing file 2302179/2302179...
[*] Sorting trace sets (this may take a while)...
[+] Found 85141 unique tuples across 2302179 files.
[*] Finding best candidates for each tuple...
    Processing file 2302179/2302179...
[*] Sorting candidate list (be patient)...
[*] Processing candidates and writing output files...
    Processing tuple 85141/85141...
[+] Narrowed down to 2944 files, saved in 'split-input-clean-wireshark/'.
```

**Stats - PacketLife dumps (11714 packets minimized to 447 samples)**

The following stats were observed from AFL's corpus minimization tool:
```
[*] Testing the target binary...
[+] OK, 12847 tuples recorded.
[*] Obtaining traces for input files in '/opt/uniq-packetlife/'...
    Processing file 11714/11714...
[*] Sorting trace sets (this may take a while)...
[+] Found 31850 unique tuples across 11714 files.
[*] Finding best candidates for each tuple...
    Processing file 11714/11714...
[*] Sorting candidate list (be patient)...
[*] Processing candidates and writing output files...
    Processing tuple 31850/31850...
[+] Narrowed down to 447 files, saved in 'split-input-clean-packetlife/'.
```

**Stats - ICS-pcap dumps (3417184 packets minimized to 1760 samples)**

The following stats were observed from AFL's corpus minimization tool (split set to accommodate space requirements in sort process):
```

```

# Data Sets List

http://www.netresec.com/?page=PcapFiles


