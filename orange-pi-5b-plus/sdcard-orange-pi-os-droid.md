# Orange Pi 5B Plus - Orange Pi OS - Android - SD Card 

---

## Download

1. Grab `Opios-droid-aarch64-opi5plus-23.05.1-linux5.10.110-en.tar.gz`
   from [here](https://drive.google.com/drive/folders/1gyT4bwfOihKos8DXnfzT4ZHs7wiLbpNN) to your Windows machine/VM.
2. Grab `SDDiskTool` 1.72
   from [here](https://www.mediafire.com/file/8l91whezgd3bzut/SDDiskTool_v1.7-20220619T123327Z-001.zip/file) to your
   Windows machine/VM.
3. Grab `rk3588_spl_loader_v1.08.111.bin` from [here]() to your linux machine/VM.

## Build `rkdeveloptool` from source

I have an Ubuntu VM, so I used `rkdeveloptool` to flash the bootloader to the device instead of the Windows tool.

You will need to build the tool from source:

1. `git clone https://github.com/rockchip-linux/rkdeveloptool`
2. `cd rkdeveloptool`
3. `sudo apt-get install pkg-config libusb-1.0  libudev-dev libusb-1.0-0-dev dh-autoreconf`
4. `aclocal`
5. `autoreconf -i`
6. `autoheader`
7. `automake --add-missing`
8. `./configure`
9. Remove the `-Werror` flag from the Makefile(s):

``` 
find . -type f -name 'Makefile.*' -exec sed -i 's/-Werror//g' {} +
```

10. `make`

## Use `rkdeveloptool` to flash the bootloader to the device.

Provided you have the device in maskrom mode and connected via USB-C, you can now flash the bootloader:

```bash
sudo rkdeveloptool ld # list devices 
```

You should see something like this: `DevNo=1	Vid=0x2207,Pid=0x330c,LocationID=101	Maskrom`

Now flash the bootloader:

```bash
sudo rkdeveloptool db rk3588_spl_loader_v1.08.111.bin 
```

You should see a confirmation message once it is complete.

## Use `SDDiskTool` to flash the Android image to the SD Card

1. Decompress `Opios-droid-aarch64-opi5plus-23.05.1-linux5.10.110-en.tar.gz` to a folder on your Windows machine/VM.
2. Extract `SDDiskTool` to a folder on your Windows machine/VM.
3. Insert the SD Card into your Windows machine/VM.
4. Run `SDDiskTool.exe` as Administrator.
5. Select the SD Card from the drop down menu.
6. Check "SD Boot"
7. Click "Firmware"
8. Select the `Opios-droid-aarch64-opi5plus-23.05.1-linux5.10.110-en.img`
9. Click "Create"

Provided the process goes smoothly, you should see a confirmation message once it is complete. I did receive an error
once and just tried again and it worked.

## Boot the device

1. Insert the SD Card into the Orange Pi.
2. Connect a monitor via HDMI, and a keyboard via USB.
3. Power cycle/turn on the Orange Pi.
4. You should see the Android boot screen.
5. It will say "erasing" for a while, then reboot.
6. You should see the Android boot screen again.
7. For me, it booted into Android after this.


## Notes

If you have questions/comments about this, please open an issue on this repository, or hit up the Orange Pi [discord](https://discord.gg/pK4YHmTSeF).
