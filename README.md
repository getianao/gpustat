`gpustat`
=========

[![pypi](https://img.shields.io/pypi/v/gpustat.svg?maxAge=86400)][pypi_gpustat]
[![license](https://img.shields.io/github/license/wookayin/gpustat.svg?maxAge=86400)](LICENSE)

Just *less* than nvidia-smi?

![Screenshot: gpustat -cp](https://github.com/wookayin/gpustat/blob/master/screenshot.png)

NOTE: This works with NVIDIA Graphics Devices only, no AMD support as of now. Contributions are welcome!

Self-Promotion: A web interface of `gpustat` is available (in alpha)! Check out [gpustat-web][gpustat-web].

[gpustat-web]: https://github.com/wookayin/gpustat-web



Quick Installation
------------------

Install from [PyPI][pypi_gpustat]:

```
pip install gpustat
pip install -e .
pip install git+https://github.com/getianao/gpustat.git
```

If you don't have root (sudo) privilege, please try installing `gpustat` on user namespace: `pip install --user gpustat`.

To install the latest version (master branch) via pip:

```
pip install git+https://github.com/wookayin/gpustat.git@master
```


### NVIDIA Driver and `pynvml` Requirements

>[!IMPORTANT]
> **DO NOT:** `pip install pynvml`, nor include [`pynvml`][pypi_wrong] as a dependency in your python project. This will not work.
>
> Instead: `pip install nvidia-ml-py`. [nvidia-ml-py][pypi_pynvml] is NVIDIA's the official python binding for NVML.

- gpustat 1.2+: Requires `nvidia-ml-py >= 12.535.108` ([#161][gh-issue-161])
- gpustat 1.0+: Requires NVIDIA Driver **450.00** or higher and `nvidia-ml-py >= 11.450.129`.
- If your NVIDIA driver is too old, you can use older `gpustat` versions (`pip install gpustat<1.0`). See [#107][gh-issue-107] for more details.


### Python requirements

- gpustat<1.0: Compatible with python 2.7 and >=3.4
- gpustat 1.0: [Python >= 3.4][gh-issue-66]
- gpustat 1.1: Python >= 3.6


Usage
-----

`$ gpustat`

Options (Please see `gpustat --help` for more details):

* `--color`            : Force colored output (even when stdout is not a tty)
* `--no-color`         : Suppress colored output
* `-u`, `--show-user`  : Display username of the process owner
* `-c`, `--show-cmd`   : Display the process name
* `-f`, `--show-full-cmd`   : Display full command and cpu stats of running process
* `-p`, `--show-pid`   : Display PID of the process
* `-F`, `--show-fan`   : Display GPU fan speed
* `-e`, `--show-codec` : Display encoder and/or decoder utilization
* `-P`, `--show-power` : Display GPU power usage and/or limit (`draw` or `draw,limit`)
* `-a`, `--show-all`   : Display all gpu properties above
* `--id`              : Target and query specific GPUs only with the specified indices (e.g. `--id 0,1,2`)
* `--no-processes`    : Do not display process information (user, memory) ([#133][gh-issue-133])
* `--watch`, `-i`, `--interval`   : Run in watch mode (equivalent to `watch gpustat`) if given. Denotes interval between updates.
* `--json`             : JSON Output ([#10][gh-issue-10])
* `--print-completion (bash|zsh|tcsh)` : Print a shell completion script. See [#131][gh-issue-131] for usage.


### Tips

- Try `gpustat --debug` if something goes wrong.
- To periodically watch, try `gpustat --watch` or `gpustat -i` ([#41][gh-issue-41]).
    - For older versions, one may use `watch --color -n1.0 gpustat --color`.
- Running `nvidia-smi daemon` (root privilege required) will make querying GPUs much **faster** and use less CPU ([#54][gh-issue-54]).
- The GPU ID (index) shown by `gpustat` (and `nvidia-smi`) is PCI BUS ID,
  while CUDA uses a different ordering (assigns the fastest GPU with the lowest ID) by default.
  Therefore, in order to ensure CUDA and `gpustat` use **same GPU index**,
  configure the `CUDA_DEVICE_ORDER` environment variable to `PCI_BUS_ID`
  (before setting `CUDA_VISIBLE_DEVICES` for your CUDA program):
  `export CUDA_DEVICE_ORDER=PCI_BUS_ID`.


[pypi_gpustat]: https://pypi.org/project/gpustat/
[pypi_pynvml]: https://pypi.org/project/nvidia-ml-py/#history
[pypi_wrong]: https://pypi.org/project/pynvml/
[gh-issue-10]: https://github.com/wookayin/gpustat/issues/10
[gh-issue-41]: https://github.com/wookayin/gpustat/issues/41
[gh-issue-54]: https://github.com/wookayin/gpustat/issues/54
[gh-issue-66]: https://github.com/wookayin/gpustat/issues/66
[gh-issue-107]: https://github.com/wookayin/gpustat/issues/107
[gh-issue-131]: https://github.com/wookayin/gpustat/issues/131
[gh-issue-133]: https://github.com/wookayin/gpustat/issues/133
[gh-issue-161]: https://github.com/wookayin/gpustat/issues/161#issuecomment-1784007533

Default display
---------------

```
[0] GeForce GTX Titan X | 77°C,  96 % | 11848 / 12287 MB | python/52046(11821M)
```

- `[0]`: GPU index (starts from 0) as PCI_BUS_ID
- `GeForce GTX Titan X`: GPU name
- `77°C`: GPU Temperature (in Celsius)
- `96 %`: GPU Utilization
- `11848 / 12287 MB`: GPU Memory Usage (Used / Total)
- `python/...`: Running processes on GPU, owner/cmdline/PID (and their GPU memory usage)

Changelog
---------

See [CHANGELOG.md](CHANGELOG.md)


License
-------

[MIT License](LICENSE)
