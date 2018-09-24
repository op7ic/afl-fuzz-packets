This repository contains unique packet samples which were narrowed down with help of TSHARK and AFL corpus minimization tool. These files can be used as seed input for fuzzing various packet parsing programs (tshark/wireshark etc). Overall 5731077 packets were reduced to 2749 unique samples. 

There are two folders in this repository:
- *samples/*  (folder with 5195 packets before final minimization)
- *final-samples/* (folder with 2749 packets after final minimization)

The samples were gathered as follows:

* Known samples from wireshark and other captures website were downloaded and cleaned up:
```
1) Download wireshark samples
wget --random-wait -e robots=off -nH -l 100000 -A pcap,cap,pcapng,cap.gz,pcap.gz,pcapng.gz -r --user-agent="Mozilla/5.0 (Macintosh; Intel Mac OS X 10.8; rv:21.0) Gecko/20100101 Firefox/21.0" https://wiki.wireshark.org/SampleCaptures

2) Download packetlife samples
wget --random-wait -e robots=off -nH -A pcap,cap,pcapng -l 100000 -r --user-agent="Mozilla/5.0 (Macintosh; Intel Mac OS X 10.8; rv:21.0) Gecko/20100101 Firefox/21.0" http://packetlife.net/captures/

3) Download ICS-pcap samples 
https://github.com/automayt/ICS-pcap
```

* Sample PCAP files were split into individual packets
```
for i in *; do editcap -c 1 $i split-input-clean/$ANDOM-$RANDOM-$RANDOM-; done
```

* AFL and tshark were used to minimize the corpus of individual packets: 
```
/opt/afl-2.52b/afl-cmin -i split-input-clean/ -o uniq-output-packets/ -t 9000 -m 99999999999999999 ./tshark -nVxr @@
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

**Stats - ICS-pcap dumps (3417184 packets minimized to 1804 samples)**

The following stats were observed from AFL's corpus minimization tool (split set to accommodate space requirements in sort process so below is just a sample from one of the sets):
```
[*] Testing the target binary...
[+] OK, 13427 tuples recorded.
[*] Obtaining traces for input files in '/opt/uniq-ics-1/'...
    Processing file 227812/227812...
[*] Sorting trace sets (this may take a while)...
[+] Found 33739 unique tuples across 227812 files.
[*] Finding best candidates for each tuple...
    Processing file 227812/227812...
[*] Sorting candidate list (be patient)...
[*] Processing candidates and writing output files...
    Processing tuple 33739/33739...
[+] Narrowed down to 627 files, saved in '/opt/uniq-ics-output-1/'.
```

**Stats - Final Reduction from all sets (5195 packets minimized to 2749 samples)**

The following stats were observed from AFL's corpus minimization tool after all the minimized samples were mixed together:
```
[*] Testing the target binary...
[+] OK, 13557 tuples recorded.
[*] Obtaining traces for input files in '/opt/final-packets/'...
    Processing file 5195/5195...
[*] Sorting trace sets (this may take a while)...
[+] Found 76447 unique tuples across 5195 files.
[*] Finding best candidates for each tuple...
    Processing file 5195/5195...
[*] Sorting candidate list (be patient)...
[*] Processing candidates and writing output files...
    Processing tuple 76447/76447...
[+] Narrowed down to 2749 files, saved in '/opt/uniq-samples-final/'.
```

# Data Sets List

http://www.netresec.com/?page=PcapFiles


