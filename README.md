# POCO F6 — Fix 90W Fast Charging di AOSP-based ROM (FlameOS)

Tujuan: dokumentasi riset + bundle restore komponen charging Xiaomi agar 90W fast charging berfungsi normal di AOSP-based ROM (FlameOS/ASCP).

## Problem
ROM AOSP-based (ASCP) kehilangan stack charging Xiaomi: miCharge HAL (vendor.xiaomi.hardware.micharge), BAA charger configs, serta beberapa blob terkait → detection 90W/PD bisa tidak optimal.

## Solusi
Restore komponen berikut dari ROM yang masih memiliki 90W berfungsi:

### Bundle: `restore_bundle/`
Berisi file siap dipakai (struktur vendor/odm sesuai partition):

**Vendor:**
- bin/hw/vendor.xiaomi.hardware.micharge-service
- bin/batterysecret
- lib64/vendor.xiaomi.hardware.micharge-V2-ndk.so
- lib64/libbaa_* (7 files) + libaudiochargerlistener.so
- etc/init/vendor.xiaomi.hardware.micharge-service.rc
- etc/init/hw/init.batterysecret.rc
- etc/vintf/manifest/vendor.xiaomi.hardware.micharge.xml
- etc/charger_fw_fstab.qti, etc/charger_diag.cfg

**ODM:**
- etc/charger/BAA_config_common.json, BAA_config_peridot.json
- firmware/211_Charge_RTP.bin, 74_ChargeWire_RTP.bin, 75_ChargeWireless_RTP.bin

## Cara Pemakaian (Rekomendasi: KernelSU/Magisk Module)
Buat module untuk overlay file2 ini ke /vendor dan /odm (safest, reversible). Pastikan permission + SELinux context terjaga.

## Dokumentasi
- `RISET-90W-FASTCHARGE.md` — analisis perbandingan ASCP vs ROM terpasang
- `CHARGING_FILES_LIST.md` — daftar lengkap file yang dibutuhkan
- `RESTORE_PLAN.md` — detail rencana restore + test checklist
