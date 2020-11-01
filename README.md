# Notes on I/O ordering

Regarding the example presented by Arm distinguished engineer [Will Deacon](https://www.linkedin.com/in/will-deacon-47946520/) on the presentation [Uh-oh; it's I/O ordering](https://elinux.org/images/a/a8/Uh-oh-Its-IO-Ordering-Will-Deacon-Arm.pdf)

[[`source`](https://elinux.org/images/a/a8/Uh-oh-Its-IO-Ordering-Will-Deacon-Arm.pdf#%5B%7B%22num%22%3A60%2C%22gen%22%3A0%7D%2C%7B%22name%22%3A%22Fit%22%7D%5D)]
#### Example: store buffering
##### Initially,*x and *y are 0 in memory; foo and bar are local (register) variables:
```shell
CPU0                           CPU1
a: WRITE_ONCE(*x, 1);          c: WRITE_ONCE(*y, 1);
b: foo = READ_ONCE(*y);        d: bar = READ_ONCE(*x);
```

That inspire me to do some experiments regarding I/O ordering and related memory model, then this respository comes.

## Install herdtools7
- opam
- herdtools7

Before install `herdtools7`, we need install `opam` first
```shell
$ sudo apt install opam
$ opam init
$ eval `opam config env`
```

When you see message below, please wait...
```shell
=-=- Updating package repositories =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
[default] synchronized from https://opam.ocaml.org/1.2.2
```

When you see message below, use `y`
```shell
Do you want OPAM to modify ~/.profile and ~/.ocamlinit?
(default is 'no', use 'f' to name a file other than ~/.profile)
```

```shell
$ opam switch --all
$ opam update
$ opam upgrade
```

```shell
$ opam install herdtools7
```

## Experiments
- [SB (Store Buffer)](https://github.com/jeffrey-minwei/io-ordering/blob/main/arm/sb_dmbs.md)
