# clusterer
An ansible framework to encluster remote machines

## Installation

Ubuntu installation:
```sh
root@host:~# apt install ansible
```

ssh-copy-id

# Allow members of group sudo to execute any command
%sudo   ALL=(ALL:ALL) NOPASSWD:ALL

 sudo lxc-ls --fancy
 
sudo virsh -c lxc:/// domxml-from-native lxc-tools /var/lib/lxc/test-container-started/config

## Contributing

1. Fork the project repo
2. Create your feature branch: `git checkout -b myfeature`
3. Commit your changes: `git commit -am 'Added my feature'`
4. Push to the branch: `git push origin myfeature`
5. Submit a pull request

## Credits

Steve Marvell (SM) - Systems Architect - Unipart Digital

## History

TODO

## Comments

I had to use a different vagrant image because the currently
maintained one has a `hostname` and `ifdown` and patch workarounds are
unwise. Better to wait for Vagrant 1.8.2 and any appropriate updates
to Ununtu 16.04. - SM

Had to downgrade to wily64 to get multiple network cards to work.

## TODO

* Address the above ifdown issue
* Use a current vagrant image rather than user image when above is fixes
* Create keys for inter box ssh

## License

GLPv3 - see LICENSE

