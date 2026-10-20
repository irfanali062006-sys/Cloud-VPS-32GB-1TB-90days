# 32GB RAM + 1TB Storage VPS — 90-Day Setup Guide

**Language:** Simple Roman Hindi  
**Goal:** Android phone se ek remote Linux computer/VPS ko securely access karna, jisme plan ke hisaab se 32GB RAM aur lagbhag 1TB disk ho.

## Important reality check

- **GitHub repository VPS nahi hoti.** GitHub par ye files instructions/configuration ke liye hain; ye apne aap 32GB RAM ya 1TB storage create nahi karti.
- **Tailscale RAM ya storage nahi deta.** Ye sirf tumhare Android aur VPS ke beech private network connection banata hai.
- 32GB RAM + 1TB disk ke liye cloud provider par matching VM/disk plan lena hoga. 90 din ka free access guaranteed nahi hai.
- Trial credit, disk, snapshots, public IP, traffic aur taxes ki pricing provider ke hisaab se alag ho sakti hai. Create karne se pehle calculator mein **90 din ka total** check karo.
- Kabhi bhi GitHub repo mein cloud password, API key, SSH private key, Tailscale auth key ya billing details upload mat karna.

## Repo mein kya hai?

- `cloud-init.yaml` — Ubuntu VM first boot par basic packages install karta hai.
- `scripts/install-tailscale.sh` — Ubuntu VPS par Tailscale install karne ka script.
- `scripts/storage-check.sh` — RAM, disk aur mount information dikhata hai.
- `docs/90-day-cost-checklist.md` — billing aur safety checklist.

## Step-by-step (Android se)

### 1. GitHub par repository banao

1. GitHub app ya browser mein https://github.com kholo.
2. `+` → **New repository** par jao.
3. Repository name: `Cloud-VPS-32GB-1TB-90Days`
4. Public ya Private choose karo. Private bhi secrets rakhne ki jagah nahi hai.
5. **Create repository** dabao.
6. Is ZIP ko extract karke files repository mein upload karo, ya GitHub website par **Add file → Upload files** use karo.

### 2. Cloud provider par server choose karo

Ubuntu 22.04/24.04 LTS ka VM choose karo. Plan mein ye confirm karo:

- RAM: 32 GB
- Disk: kam se kam 1 TB (provider disk size ko GB/GiB mein alag tarah count kar sakta hai)
- Region aur CPU requirements
- 90 din ka total cost, tax aur disk charges
- Trial expiry aur data deletion policy

**Note:** Kuch providers boot disk aur extra data disk alag charge karte hain. VM stop karne par bhi disk ka charge lag sakta hai.

### 3. VM ko secure rakho

- SSH key se login ko preference do.
- SSH (port 22) ko sirf apne IP tak restrict karo, agar provider firewall ye option deta hai.
- SMB/Windows file-sharing ports ko public internet par expose mat karo.
- Root password ya private key GitHub par mat daalo.

### 4. Tailscale install karo

VPS ke terminal mein script chalane se pehle review karo. Ubuntu par:

```bash
sudo bash scripts/install-tailscale.sh
```

Agar file VPS par nahi hai, to Tailscale ki official Linux installation instructions follow karo:
https://tailscale.com/kb/1031/install-linux/

Phir VPS terminal mein:

```bash
sudo tailscale up
```

Terminal jo login URL de, use browser mein kholo aur apne Tailscale account se authorize karo. Auth key ko repo mein paste mat karna.

### 5. Android connect karo

1. Android par Tailscale install karo: https://play.google.com/store/apps/details?id=com.tailscale.ipn
2. Wahi Tailscale account use karo.
3. VPS ko Tailscale admin console mein online check karo.
4. VPS ka Tailscale IP `100.x.y.z` jaisa dikhega.

### 6. Storage aur RAM verify karo

VPS par:

```bash
bash scripts/storage-check.sh
free -h
lsblk
df -h
```

`free -h` RAM dikhata hai. `lsblk` aur `df -h` disk aur mounted storage dikhate hain. Sirf 1TB disk attach karna kaafi nahi—disk ko provider ke instructions ke mutabik partition/format/mount bhi karna hota hai.

**Caution:** Nayi disk format karne se uska existing data erase ho sakta hai. Device name confirm kiye bina `mkfs` ya formatting command mat chalana.

## 90 din ke baad

- Expiry se pehle important data ka backup lo.
- VM, disk, snapshots aur public IP ke charges alag ho sakte hain.
- Agar continue nahi karna, pehle data download/backup karo; phir provider dashboard se VM aur unneeded paid resources delete karo.
- Sirf VM stop karna billing ko poori tarah stop nahi karta.

## Official links

- Tailscale Linux install: https://tailscale.com/kb/1031/install-linux/
- Tailscale Android: https://play.google.com/store/apps/details?id=com.tailscale.ipn
- Google Cloud free trial details: https://cloud.google.com/free
- Cloud pricing calculator (Google Cloud): https://cloud.google.com/products/calculator

**Ye repo setup guide hai, free VPS ka promise nahi.** Actual resources aur price provider ke plan par depend karte hain.
