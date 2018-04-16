This repository contains uniq single packet dumps which were narrowed down with help of TSHARK and AFL corpus minimization tool.

The samples were gathered as follows:

* Download known samples from wireshark sample capture website
```
wget --random-wait -e robots=off -nH -l 100000 -r --user-agent="Mozilla/5.0 (Macintosh; Intel Mac OS X 10.8; rv:21.0) Gecko/20100101 Firefox/21.0" https://wiki.wireshark.org/SampleCaptures
# Note - downloaded files need to be cleaned up from HTML sources and other junk that is not got anything to do with pcaps
```

* Split samples into single packet files
```
for i in *; do editcap -c 1 $i input-caps/file-$RANDOM-$RANDOM-; done
```

* Use AFL to gather unique samples by minimizing the corpus: 
```
/opt/afl-2.52b/afl-cmin -i input-caps/ -o split-input/ -t 9000 -m 99999999999999999 ./tshark -nVxr @@
```

# Stats

The following stats were observed from corpus minimization tool:

```
[*] Testing the target binary...
[+] OK, 13588 tuples recorded.
[*] Obtaining traces for input files in 'input-caps/'...
    Processing file 2302179/2302179...
[*] Sorting trace sets (this may take a while)...
[+] Found 85141 unique tuples across 2302179 files.
[*] Finding best candidates for each tuple...
    Processing file 2302179/2302179...
[*] Sorting candidate list (be patient)...
[*] Processing candidates and writing output files...
    Processing tuple 85141/85141...
[+] Narrowed down to 2944 files, saved in 'split-input-clean/'.
```