---
title: 'Updating remotely'
date: 2024-10-25T13:28:18-04:00
weight: 1
reading_time: 1 minute
---

## Using a remote end-point for your updates

In this section we will use `zbcli` to configure a remote end-point (something other than your USB Drive), and then trigger an update from that endpoint.

### Updating the configuration

Since we previously configured Bootware to update from your USB Drive, we will need to update the configuration in order to define a new end-point. Just like we did before, we will run the following command:

```bash
sudo zbcli update-config
```

You can accept all of the default answers, until you get to the `Enter update endpoint` prompt. Here, you will enter the following:

```bash
https://bootware.s3.amazonaws.com/zymbit_bookworm64_1.1.zi
```

> [!TIP]
> If you want to try a different image (say, Ubuntu) there are other images available. See the [documentation](https://docs.zymbit.com/bootware/image-files/) for a list of available images.
>

This tells Bootware that, at the next update, pull the update image from that URL (instead of the previously configured `/dev/sda1` location.)

### Triggering the update

Just like we did previously, we will have to tell Bootware to update the device. Unlike before, we won't have the `public_key.pem` file handy, so we will need to retrieve it from the server:

```bash
curl -L -o pub_key_1.2.pem https://bootware.s3.amazonaws.com/1.2/pub_key_1.2.pem
```


Now, it's time to trigger the update from the remote endpoint:

```bash
sudo zbcli update
```

Once again, you'll be shown the current configuration:

```bash
   Validated bootware installation
	---------
	Pi Module:         Raspberry Pi 4
	Operating System:  Rpi-Bookworm
	Zymbit module:     Zymkey
	Kernel:            kernel8.img
	---------
     Cleaned '/etc/zymbit/zboot/update_artifacts/tmp'
       Found update configs
? Proceed with current configs? These can be modified through 'zbcli update-config'
	---------
 	Update endpoint   https://bootware.s3.amazonaws.com/zymbit_bookworm64_1.1.zi
 	Update name       z-image-1
 	Endpoint type     LOCAL
 	Partition layout  A/B
 	Update policy     UPDATE_BOTH
 	---------
     Created temporary directory (/etc/zymbit/zboot/update_artifacts/tmp/.tmpCfhm6c)
✔ Enter public key file (Pem format) ·
```

For the public key, enter `./pub_key_1.2.pem` and proceed with the reboot.

As the Pi reboots, Bootware will download the new image from the Amazon S3 bucket we gave it, and reboot to that partition.

> [!CAUTION]
> If you set your username/password combination on your Pi to something other than zymbit/zymbit that username/password combination will no longer work. The images supplied above all have the username/password set to zymbit/zymbit
>

### Congratulations

You have now successfully configured your Raspberry Pi to update itself from a remote endpoint rather than a USB Drive!

Well done!

{{< pagenav prev=".." next="../page2/" >}}