# Install Alpine Linux on Termux

## Step 1: Install QEMU
```sh
pkg install qemu-utils qemu-common qemu-system-x86_64-headless
```

## Step 2: Download Alpine Linux
```sh
mkdir alpine && cd $_
wget http://dl-cdn.alpinelinux.org/alpine/v3.12/releases/x86_64/alpine-virt-3.12.3-x86_64.iso
```

## Step 3: Create Disk Image
```sh
qemu-img create -f qcow2 alpine.img 4G
```

## Step 4: Boot with QEMU
```sh
qemu-system-x86_64 -machine q35 -m 1024 -smp cpus=2 -cpu qemu64 \
          -drive if=pflash,format=raw,read-only,file=$PREFIX/share/qemu/edk2-x86_64-code.fd \
          -netdev user,id=n1,hostfwd=tcp::2222-:22 -device virtio-net,netdev=n1 \
          -cdrom alpine-virt-3.12.3-x86_64.iso \
          -nographic alpine.img
```

## Step 5: Setup Network Interface
```sh
setup-interfaces
```

## Step 6: Enable Ethernet
```sh
ifup eth0
```

## Step 7: Download Answer File
```sh
wget https://gist.githubusercontent.com/oofnikj/e79aef095cd08756f7f26ed244355d62/raw/answerfile
```

## Step 8: Edit Setup Disk Configuration
```sh
sed -i -E 's/(local kernel_opts)=.*/\1="console=ttyS0"/' /sbin/setup-disk
```

## Step 9: Install Alpine Linux
```sh
setup-alpine -f answerfile
```

## Step 10: Boot into Alpine Linux
```sh
qemu-system-x86_64 -machine q35 -m 1024 -smp cpus=2 -cpu qemu64 \
          -drive if=pflash,format=raw,read-only,file=$PREFIX/share/qemu/edk2-x86_64-code.fd \
          -netdev user,id=n1,hostfwd=tcp::2222-:22 -device virtio-net,netdev=n1 \
          -nographic alpine.img
```

---
For the full tutorial video, you can watch it on YouTube:
[YouTube Channel: PocketAlpine](https://youtube.com/@PocketAlpine)

