# Fundamentals

|   | `tmux kill-session -t <session_name>` | Menutup sesi tertentu. |
| - | ------------------------------------- | ---------------------- |

|   | `tmux kill-server` | Menutup semua sesi Tmux. |
| - | ------------------ | ------------------------ |

| **Scroll dan Copy** | `Ctrl+b [` | Memasuki mode scroll untuk melihat output sebelumnya. |
| ------------------- | ---------- | ----------------------------------------------------- |

|   | `Ctrl+b ]` | Menempelkan teks yang sudah disalin ke clipboard. |
| - | ---------- | ------------------------------------------------- |

| **Layout** | `Ctrl+b Space` | Mengubah layout pane. |
| ---------- | -------------- | --------------------- |

|   | `Ctrl+b q` | Menampilkan nomor pada setiap pane. |
| - | ---------- | ----------------------------------- |

|   | `Ctrl+b {` | Memindahkan pane ke kiri. |
| - | ---------- | ------------------------- |

|   | `Ctrl+b }` | Memindahkan pane ke kanan. |
| - | ---------- | -------------------------- |

| **Lainnya** | `Ctrl+b ?` | Menampilkan daftar shortcut di Tmux. |
| ----------- | ---------- | ------------------------------------ |

|   | `tmux source-file ~/.tmux.conf` | Memuat ulang konfigurasi Tmux tanpa menutup sesi. |
| - | ------------------------------- | ------------------------------------------------- |

## NMAP

```bash
nmap -sV -sC -p- 10.129.42.253
```

## GOBUSTER

```bash
gobuster dir -u http://83.136.253.44:50922 -w /usr/share/wordlists/dirb/common.txt
```

{% hint style="info" %}
full stops(.) are hidden directories
{% endhint %}



Other Resources:

{% embed url="https://medium.com/@joshthedev/step-5-getting-started-a586c57af17d" %}

{% embed url="https://swisskyrepo.github.io/InternalAllTheThings/cheatsheets/shell-reverse-cheatsheet/#tools" %}
