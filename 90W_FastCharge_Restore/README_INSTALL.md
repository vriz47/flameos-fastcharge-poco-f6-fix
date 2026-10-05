# 90W FastCharge Restore - Install Guide

Module ini mengembalikan komponen charging Xiaomi (miCharge HAL + BAA configs + charger firmware) agar 90W Fast Charging bisa terdeteksi normal di AOSP-based ROM (ASCP, dsb.) untuk POCO F6 (peridot).

## Compatibility
- Device: POCO F6 (codename peridot) - ONLY
- ROM: AOSP-based (contoh ASCP v6.x). Tidak untuk ROM HyperOS stock yang udah ada komponennya.
- Root: KernelSU / Magisk (recommended KernelSU)

## Installation
1. Flash `90W_FastCharge_Restore_v1.0.0.zip` via KernelSU/Magisk
2. Reboot device
3. Colok charger 90W asli (PD) & tunggu ~10-30 detik

## Verifikasi (WAJIB)

Cek via Termux/ADB shell (root):

```bash
su -c '
echo "=== Adapter ==="
cat /sys/class/power_supply/usb/adapter_id
cat /sys/class/power_supply/usb/real_type
cat /sys/class/power_supply/usb/type
echo "---"
echo "input_current_limit (µA):"
cat /sys/class/power_supply/usb/input_current_limit
echo "---"
echo "Battery current/voltage/power:"
cat /sys/class/power_supply/battery/current_now
cat /sys/class/power_supply/battery/voltage_now
cat /sys/class/power_supply/battery/power_now
'
```

### Expected (berhasil)
- `adapter_id` berubah (bukan 0) saat colok 90W PD
- `input_current_limit` >= ~3000000 (3.0A) (bisa 3500000–4500000 tergantung charger/cable)
- `power_now` sekitar 70000000–90000000 (70–90W) di fase awal charging

### Tidak berhasil / anomali
Jika nilai tetap rendah (input_current_limit kecil, power ~30–45W):

1. Cek SELinux AVC
```bash
su -c 'dmesg | grep -i avc | tail -30'
su -c 'logcat -d | grep -i micharge | tail -40'
```

2. Jika ada AVC denial terkait `hal_micharge`/service: butuh `sepolicy.rule` tambahan. Copy log AVC tsb & bisa expand module (tambahkan sepolicy.rule).

## Troubleshooting

| Issue | Kemungkinan | Solusi |
|---|---|---|
| Bootloop setelah flash | Conflict dengan ROM (jarang) | Uninstall module (reboot restore) |
| micharge service tidak start | SELinux AVC | Tambah sepolicy.rule di module (bisa request log) |
| 90W masih nggak keluar | Charger/cable tidak bener (PD 90W) | Coba charger PD 90W asli + cable rated |
| Nilai fluktuatif | Thermal throttling | Normal, cek suhu |

## Notes
- Ambil dari ROM terpasang (FlameOS) device yg sama → high compatibility.
- Reversible: uninstall module via KSU/Magisk → kembali ke kondisi semula.
- Hanya untuk POCO F6 (peridot). Jangan pakai di device lain.
