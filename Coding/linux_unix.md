# Ubuntu Command Line

## List all disks

```sudo lshw -class disk -class storage [ -html | -short | -xml | -businfo ]```

## Remove all GPT/MBR partition data from raw disk

<details open><summary>Single disk</summary>

```bash
sudo sgdisk -Z /dev/sdb
```

</details>

<details open><summary>Multiple disks</summary>

```bash
sudo lshw -class disk -class storage -short | grep -Eo '/dev/sd[b-z]' | while read -r; do echo "$REPLY" && sudo sgdisk -Z "$REPLY"; done
```

<details><summary>Interactive script</summary>

```bash
#!/bin/bash

declare -r DISK_PATTERN='/dev/sd[b-z]'
declare -a disks_to_clear
declare lshw_output

lshw_output=$( sudo lshw -class disk -class storage -short 2>/dev/null )
readarray -t disks_to_clear < <( echo "${lshw_output}" | grep --only-matching --extended-regexp "${DISK_PATTERN}" )

printf 'Found %s disks matching the pattern "%s":\n\n' "${#disks_to_clear[@]}" "${DISK_PATTERN}"
echo "${lshw_output}" | grep --color --extended-regexp "^.*${DISK_PATTERN}.*$|\$"
printf '\n'

if [ ${#disks_to_clear[@]} -eq 0 ]; then
    printf 'No matching disks found\n'
    printf 'Exiting\n'
    exit
fi

printf 'After this operation, the following disks will have all GPT/MBR partition data permanently wiped:\n\n%s\n\n' "${disks_to_clear[*]}"
read -r -p 'Do you want to continue? [Y/n] '
# use the ${var,,} syntax to convert to lowercase
if [[ ! ${REPLY,,} =~ ^(y|yes)$ ]]; then
    printf 'Exiting\n'
    exit
fi
printf '\n'

for disk in "${disks_to_clear[@]}"; do
    printf 'Clearing%s\n' "$disk"
    sudo sgdisk --zap-all "$disk"
    printf '\n'
done
```

</details>
___

[Back](./README.md)  
[Home](../README.md)  
