# cmd
 
## Create a Bootable USB Flash Drive
```
diskpart

list disk
select disk <number>
clean
create partition primary
format fs=ntfs quick
active

exit
```
[https://docs.microsoft.com/en-us/windows-server-essentials/install/create-a-bootable-usb-flash-drive](https://docs.microsoft.com/en-us/windows-server-essentials/install/create-a-bootable-usb-flash-drive)

## tmpl
**item**
```
cmd     #
cmd     #
cmd     #
```
## need to add
