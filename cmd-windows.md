
# cmd
 
## Create a Bootable USB Flash Drive For Windows
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

## Create a Bootable USB Flash Drive For Linux
```
diskpart
```
[https://www.youtube.com/watch?v=DC89AryJEE8](https://www.youtube.com/watch?v=DC89AryJEE8)

[https://rufus.ie/en/](https://rufus.ie/en/)

## tmpl
**item**
```
cmd     #
cmd     #
cmd     #
```
## need to add
