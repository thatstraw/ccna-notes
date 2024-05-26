# Introduction to the Cisco IOS CLI

## What is a CLI?

- CLI stands for command-line interface.
- it's the interface you use to configure cisco devices like router, switches firewalls, etc.

## User EXEC Mode

- User EXEC mode is represented by the greater than sign, next to the hostname of the device
- User EXEC mode is very limited.
- Users can look at some things but can't make any changes to the configuration.
- Also called 'user mode'.

## Privileged EXEC Mode

- if you issue the `enable` command in User EXEC mode, you will be dropped to the Privilege EXEC Mode
- Priv EXEC Mode is represented by the pound sign(#), next to the hostname of the device.
- provides complete access to view device configurations, restart the device etc.
- cannot change the configuration, but can change the time of the device, save the configuration file etc.
- to view the commands available in these modes you can use the ? mark.

## Global Configuration Mode

- Some configurations to the cisco devices you need to be in the global configuration mode
- to switch to the global configuration mode you have to issue this command: `configure terminal`
- in this mode the hostname is followed by the word config.
- to running other configuration commands in this mode you have to precede the commands with `do`

```bash
router(config)#run
executes a priv exec command from global config mode.
```

## IOS Configuration files

There are two separate configuration files kept on the device at once, that is :

- running config
- startup config

**Running-config** - the current, active configuration file saved on the device.  As you enter the commands  in the CLI, you edit the active configuration.

**Startup-config -** the configuration file that will be loaded upon restart of the device.

```bash
# use the shwow running-config to view the running config file
# use show startup-config to view the startup config.
```

### Saving configuratons

They are actually three ways to save the running configuration to make it startup configuration.

- write
- write memory
- copy running-config startup-config

Note this commands are executed in the privilege exec mode.

### Enabling passwords

**For security purpose you might want to enable passwords on the global configuration mode:**

```bash
router(config)# enable password your_super_secret_password_here
```

To enable another layer of security we have to encrypt our passwords:

```bash
router(config)# service password-encryption
```

The above command will encrypt all the passwords in an unreadable formats. This method is cannot not guarantee security. Cisco provides a more secure way of creating passwords.

```bash
router(config)# enable secret <your password here>
```

The above method use md5 encryption algorithm, this can be still cracked. You just have to keep in mind that no password is invincible.

> To cancel or remove commands in cisco you have to use the `no`  in front of the commands.
> 

```bash
router(config)# no service password-encryption
```

If you `enable service password-encryption`:

- current passwords will be encrypted
- future passwords will be encrypted
- the enable secret will not be affected

If you disable `service password-encryption`:

- current passwords will not be decrypted, they will remain encrypted.
- future passwords will not be encrypted, they will remain in clear text.
- the enable secret will not be affected.

# Configuring IP Address

## View Router interfaces

```bash
Router# show ip interface brief
Router (config)# do show ip interface brief
```

## Configuring ip Address

```bash
Router (config)# interface g0/1
Router (config-if)# ip address <ip adress> <netmask>
```

## Add Description

```bash
Router (config-if)# description ## This interface is connected to.....##
```

## Enable the interface

```bash
Router (config-if)# no shutdown
```

# Configuring VLANS

## Show VLAN configurations

```bash
Switch# show vlan brief
Switch (config)# do show vlan brief
```

## Configure a VLAN

```bash
Switch(config)# vlan x
```

## Configure Access/Trunk

```racket
Switch(config)# interface g0/1
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan x

```

```bash
Switch(config)# interface g0/2
Switch(config-if)# switchport mode trunk
```

## Changing VLAN names

```bash
Switch(config)# vlan <Vlan ID>
Switch(config-vlan)# name  <your name here>
```