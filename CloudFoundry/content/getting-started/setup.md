---
menu:
  main:
    parent: getting-started
title: Setup
weight: -100
---

## Installing the CLI

Interacting with Cloud Foundry is easiest through the `cf` command line interface.

### OSX

```bash
brew tap pivotal/tap
brew install cloudfoundry-cli
```

### Linux

Download and uncompress the binary, and move it somewhere in your `$PATH`:

```bash
wget 'https://cli.run.pivotal.io/stable?release=linux64-binary&source=github' -O cf.tgz
tar -zxvf cf.tgz
sudo mv cf /usr/local/bin
```

### Manual download

You can also download the binaries [here](https://github.com/cloudfoundry/cli#downloads).


### Confirm the installation

```bash
cf -v
```

As of this writing the current cf CLI version is `6.13.0-e68ce0f`.

## Setting up your account

You will need a Cloud Foundry account before continuing. Your password is in your welcome email.

### Log in

```bash
cf login -a api.trial.cf.paas.alphagov.co.uk
```

Once you log in for the first time, you'll probably want to change your password with:

```bash
cf passwd
```

*Note:* depending on your network configuration you might need to set an ```HTTP_PROXY``` environment variable for the CLI to connect. Contact your network administrators to work out the correct settings for your configuration.

