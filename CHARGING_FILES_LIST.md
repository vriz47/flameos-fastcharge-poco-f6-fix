# Charging-related files (current ROM — needed for 90W fastcharge on AOSP)

## Vendor (/vendor)
- bin/hw/vendor.xiaomi.hardware.micharge-service
- bin/batterysecret (related)
- etc/init/vendor.xiaomi.hardware.micharge-service.rc
- etc/init/hw/init.batterysecret.rc (if exists? check)
- etc/vintf/manifest/vendor.xiaomi.hardware.micharge.xml
- etc/charger_fw_fstab.qti
- etc/charger_diag.cfg
- lib64/vendor.xiaomi.hardware.micharge-V2-ndk.so
- lib64/libbaa_BasedOnCC_VolDown.so
- lib64/libbaa_ChargeInfo.so
- lib64/libbaa_ClosedSourceClass.so
- lib64/libbaa_ExampleClass.so
- lib64/libbaa_FreqChgFvDown.so
- lib64/libbaa_LowSohFvDown.so
- lib64/libbaa_common.so
- lib64/libaudiochargerlistener.so

## ODM (/odm)
- etc/charger/BAA_config_common.json
- etc/charger/BAA_config_peridot.json
- firmware/211_Charge_RTP.bin
- firmware/74_ChargeWire_RTP.bin
- firmware/75_ChargeWireless_RTP.bin

Note: init.batterysecret.rc exists? check below
- etc/init/hw/init.batterysecret.rc (confirmed)
