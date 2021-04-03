# File searcing

//Find all duplicate file names in a dir
```bash
find /PATH/TO/FILES -type f -printf '%p/ %f\n' | sort -k2 | uniq -f1 --all-repeated=separate
```

//Demonstrating some of the options you have with this:
```bash
find  /PATH/TO/FILES -type f -printf 'size: %s bytes, modified at: %t, path: %h/, file name: %f\n' | sort -k15 | uniq -f14 --all-repeated=prepend
```

---

# Tracing file origin

## Who installed this file?
```bash
rpm -q --file /usr/libexec/tracker-miner-fs
```

## Package infos
```bash
rpm -qi tracker-miners-2.2.2-1.fc30.x86_64
```

## Find other related files
```bash
rpm -ql tracker-miners
```

## Check its dependencies
```bash
rpm -q --whatrequires tracker-miners
```

## Package removal
```bash
sudo dnf remove tracker-miners
```

