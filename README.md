# Zymbit Raspberry Pi Hands On Workshop

THis is a complete hands on workshop designed to teach users how to build a Raspberry Pi from scratch, install a [Zymkey](https://zymbit.com/zymkey), configure the Zymkey and then install and configure Bootware(tm).

## Running the workshop

The workshop requires Hugo to build and deploy this workshop.

Make sure that Hugo is properly installed:

```bash
hugo version
hugo v0.133.0+extended darwin/arm64 BuildDate=2024-08-17T19:57:41Z VendorInfo=brew
```

If you don't have Hugo installed, you can install it with [Homebrew](https://homebrew.sh).

```bash
brew install hugo
```
> **Note:** There is a serious bug in v0.135 and (so far) in all of the v0.136.x versions. I recommend v0.133.0 and you will need the *extended* version.

Next, clone this repository:

```bash
git clone https://github.com/davidgs/pi-hands-on
cd pi-hands-on
```
Once you have the repo cloned, install the modules:

```bash
hugo mod tidy
```

From there, you can run

```bash
hugo serve -w
```

Which will build the site and serve it (typically at [http://localhost:1313/zymbit-workshop](http://localhost:1313/zymbit-workshop)).

You can then edit the files as you see fit, and the huge server will rebuild the site in real time. 

