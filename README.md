<div align="center">

<img src="toolbox.png" alt="toolbox" width="200">

# Toolbox

This is a collection of tools, scripts, and other types of code, that I use for my daily work.<br>
It is meant to be a quick way for me to remember certain commands. I hope you find them useful too.

<br>
<br>
</div>


# Table of Contents

- [Table of Contents](#table-of-contents)
- [Linux](#linux)
    - [Folder Disk Usage](#folder-disk-usage)
    - [Get Public IP Address](#get-public-ip-address)
- [Python](#python)

---

# Linux

## Folder Disk Usage

To calculate the total storage of all folders in the current directory, you can use the `du` (disk usage) command. Here are two useful variations:

### 1. Display Disk Usage with Folder Sizes

The following command shows the disk usage of each folder in the current directory, including subdirectories up to one level deep:

```bash
du -h --max-depth=1
```

- `du`: Disk usage command.
- `-h`: Human-readable format (e.g., KB, MB, GB).
- `--max-depth=1`: Limits the display to the immediate folders in the current directory.

### 2. Display and Sort Disk Usage by Size

To sort the output by folder size in a human-readable format, you can use the following command:

```bash
du -h --max-depth=1 | sort -h
```

- `sort -h`: Sorts the output based on human-readable sizes.

This command not only shows the disk usage of each folder but also arranges them in ascending order based on size. Adjust the `--max-depth` option as needed to control the depth of the directory tree included in the output.


## Get Public IP Address

To retrieve your public IP address from the command line using `curl`, you can use the following command:

```bash
curl ipinfo.io
```

This command sends a GET request to ipinfo.io and prints the response, which contains your public IP address, as well as information about your ISP.<br>

Sample output:
```json
{
  "ip": "212.x.x.x",
  "city": "Amsterdam",
  "region": "North Holland",
  "country": "NL",
  "loc": "52.3740,4.8897",
  "org": "AS60781 LeaseWeb Netherlands B.V.",
  "postal": "1012",
  "timezone": "Europe/Amsterdam",
  "readme": "https://ipinfo.io/missingauth"
}
```