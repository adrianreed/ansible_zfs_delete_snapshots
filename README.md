# Delete ZFS snapshots
This playbook can be used to find snapshots based on include/exclude regular expressions and delete them.

## Description
- No inventory is required, but it could be included easily.
- Use the 'check' tag to do a "dry run" that will only display the snapshots your regex catches.
- Use the 'delete' tag to trigger the deletion run (it will prompt for confirmation before proceeding to delete).
- The 'save' variable should be a regex matching the snapshots you *do not* want to delete.
- The 'delete' variable should be a regex matching the snapshots you want to delete.
- Note that the usage of this playbook will highly depend on a solid naming standard in your ZFS snapshot system.

## Usage examples
Let's say your naming standard for snapshots is:
```
{path}/{YYYY}_{mm}_{dd}_{hh}_{mm}_restore
```
You could use commands similar to these:
```
# View all snapshots from 2021/02/23, in db1.mydomain.com
# ansible-playbook -i db1.mydomain.com, main.yml -e delete=.*2021_02_23.* --tags check

# View snapshots from 2021/01/22, except the ones with the word "logs" in their path, in db2.mydomain.com
# ansible-playbook -i db2.mydomain.com, main.yml -e save='.*logs.*' -e delete=.*2021_01_22.* --tags check

# Delete all snapshots from 2021/10/24, in db3.mydomain.com
# ansible-playbook -i db3.mydomain.com, main.yml -e delete=.*2021_10_24.* --tags delete

# Delete snapshots from 2021/06/08, except the ones with the words "info" or "tmp" in their path, in db4.mydomain.com
# ansible-playbook -i db4.mydomain.com, main.yml -e save='.*info|tmp.*' -e delete=.*2021_06_08.* --tags delete
```
