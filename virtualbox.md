# Windows host installation steps

- Download latest version of [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
- Download ISO file of the guest OS
([Ubuntu 22.04 LTS](https://ubuntu.com/download/desktop/thank-you?version=22.04.1&architecture=amd64)
in this example)
- Create a new VM, guided mode (4GiB RAM and fixed size disk, with enough space
  for your goals)
- Go to VM settings and set the following parameters
  + General -> Advanced -> Shared clipboard and Drag'n'Drop enabled
  + System -> Processor -> 2CPUs (or more)
  + Display -> Video Memory -> 32MiB (or more)
  + This is just a box for compiling, depending on the use case different
    settings may be required to achieve higher perf
- Go to VM settings -> Storage -> Storage devices -> Controller IDE -> Empty ->
  Optical drive -> Choose a disk file and set the path to the ISO file
  downloaded before. Press ok.
- Start the VM and proceed with the minimal installation
- Install basic tooling to build the kernel modules of VBox guest additions:
  > sudo apt-get install gcc perl make
- Install VBox guest additions
- Reboot the guest
- Check VBox guesst additions have been successfully installed (e.g., it should
  be possible to rescale the display)
- Power off the guest
- Go to VM settings -> Shared folders -> Add new shared folder
  + Choose a folder in host and enable auto-mount option
- Power on the guest
- Add current user to `vboxsf` group, e.g.:
  > sudo adduser $USER vboxsf
- Reboot the guest (depending on the guest OS, logging out may be enough)
- Current user should now have access to the shared folder located in `/media`
  with name `sf_<folder-name>`
- Install `openssh-server` and `ifconfig` (included in `net-tools` package):
  > sudo apt-get install openssh-server net-tools
- In the host, configure host-to-guest connection via ssh (follow this
 [step-by-step](https://medium.com/nycdev/how-to-ssh-from-a-host-to-a-guest-vm-on-your-local-machine-6cb4c91acc2e))
- In the guest OS install `clang`, `clang-format`, and `clang-tidy`:
  > sudo apt-get install clang clang-format clang-tidy
- [`optional`] In the host OS install
  [vscode remote development extension](https://code.visualstudio.com/docs/remote/ssh)
  + After the installation process, [set the default ssh port to the one used
    for the VM](https://dev.to/smac89/custom-ports-in-ssh-config-for-vscode-remote-ssh-505c)
    (e.g., if the aforementioned step-by-step was followed, this
    should be port `5679`)