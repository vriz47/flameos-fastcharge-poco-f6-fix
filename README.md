# POCO F6 — Fix 90W Fast Charging di AOSP-based ROM (FlameOS)

Tujuan: mendokumentasikan hasil riset + solusi biar 90W fast charging detek & jalan normal pas pakai AOSP-based ROM (FlameOS) dibandingkan stock HyperOS.

## Latar Belakang
POCO F6 (peridot) support fast charging 90W. Di beberapa AOSP-based/custom ROM (termasuk FlameOS recook) kadang nilai input current/limit, thermal atau detection charger beda → charging turun (mis. 30–45W) atau nggak max 90W.

## Fokus Riset
- Charger detection (USB PD / QC?) & input current limit
- Kernel/driver charging (charger IC, fuel gauge)
- Props / sysfs terkait charging
- MIUI vs AOSP difference
- Possible workaround: tweak sysfs, override input_current_limit, thermal policy, or module/Xposed? 

## Status
WIP — belum ada temuan final.

## File
- `RISET-90W-FASTCHARGE.md` — log step by step (hasil observasi, test, fix)
