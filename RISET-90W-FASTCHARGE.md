# Riset: Fix 90W Fast Charging POCO F6 (FlameOS AOSP-based)

Date: 2026-10-05
Device: POCO F6 (peridot) — FlameOS (AOSP-based), root KernelSU
Status: WIP

## Observasi Awal
Perlu cek kenapa di FlameOS kadang nggak dapet 90W full vs stock HyperOS.
Target: temukan perbedaan detection/limiter.

## TODO
- [ ] Dump sysfs charging: /sys/class/power_supply/{battery,usb,charger,*}
- [ ] Cek dmesg | grep -i charg
- [ ] Cek logcat saat colok charger
- [ ] Bandingin stock vs flameos (kalo ada log)
- [ ] Tes tweak input_current_limit / thermal
- [ ] Dokumentasi temuan + solusi
