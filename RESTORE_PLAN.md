# Plan Restore Charging Components ke AOSP ROM (ASCP)

## Tujuan
Mengembalikan komponen Xiaomi charging (miCharge HAL + BAA configs + blobs) agar 90W fastcharge terdeteksi normal di AOSP-based ROM.

## Komponen Wajib Dipulihkan

### 1. Vendor - miCharge HAL
Files:
- vendor/bin/hw/vendor.xiaomi.hardware.micharge-service (service binary)
- vendor/etc/init/vendor.xiaomi.hardware.micharge-service.rc (init)
- vendor/etc/vintf/manifest/vendor.xiaomi.hardware.micharge.xml (manifest - VINTF)
- vendor/lib64/vendor.xiaomi.hardware.micharge-V2-ndk.so (HAL lib)

Action: copy ke vendor partition (atau vendor.img overlay/system patch sesuai metode ROM)

### 2. Vendor - BAA libs (Battery/Charger strategy)
Files:
- vendor/lib64/libbaa_*.so (7 files: BasedOnCC_VolDown, ChargeInfo, ClosedSourceClass, ExampleClass, FreqChgFvDown, LowSohFvDown, common)
- vendor/etc/charger_fw_fstab.qti
- vendor/etc/charger_diag.cfg
- vendor/lib64/libaudiochargerlistener.so

### 3. Vendor - BatterySecret (opsional tapi terkait)
- vendor/bin/batterysecret
- vendor/etc/init/hw/init.batterysecret.rc

### 4. ODM - Charger configs + firmware
Files:
- odm/etc/charger/BAA_config_common.json
- odm/etc/charger/BAA_config_peridot.json
- odm/firmware/211_Charge_RTP.bin
- odm/firmware/74_ChargeWire_RTP.bin
- odm/firmware/75_ChargeWireless_RTP.bin

## Metode Restore
Ada beberapa opsi tergantung ROM (ASCP AOSP):
1. Magisk/KernelSU module overlay (system/vendor/odm overlay via module atau OverlayFS) — safest, tidak modifikasi ROM image langsung
2. Patch vendor.img/odm.img saat build (jika source)
3. Flash addon zip (custom)

Recommended: **Module approach** (reversible). Buat module yang taruh files di path yang tepat + set permission + selinux context.

## Cek Prerequisite
- Pastikan ROM support dynamic partitions atau bisa mount vendor/odm rw (atau via overlay)
- Tes setelah restore: colok 90W, cek adapter_id, input_current_limit, charge_type, power_now
- Verifikasi: tidak muncul thermal throttle abnormal, charging stabil ke 90W

## Test Checklist
- [ ] Boot normal setelah restore
- [ ] Charger 90W terdeteksi (adapter_id berubah, input_current_limit ~3500000–4500000mA range sesuai PD)
- [ ] Power draw ~90W (voltage*current)
- [ ] Tidak ada FC related charging

## Risiko
- Salah SELinux/context bisa bikin micharge service tidak start (logcat: avc denials)
- ROM beda base bisa butuh sepolicy tambahan (rare)
- Pastikan file dari ROM yang sama device (peridot) — files diatas dari ROM terpasang (device match)

## Next Step
1. Extract files tsb ke bundle terpisah (copy dengan permission)
2. Buat struktur module (META-INF + system/vendor/odm)
3. Buat README install (flash via KSU/Magisk)
