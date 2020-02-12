# Android RAM management fixes by crok

## Tweak the memory management of the device, enable more background apps.. et cetera..

```
ro.config.fha_enable=true
ro.sys.fw.bg_apps_limit=32
ro.config.dha_cached_max=16
ro.config.dha_empty_max=42
ro.config.dha_empty_init=32
ro.config.dha_lmk_scale=0.545
ro.config.dha_th_rate=2.3
ro.config.sdha_apps_bg_max=64
ro.config.sdha_apps_bg_min=8
```

## Virtual memory tweaks
```
stop perfd
echo '0' > /sys/module/lowmemorykiller/parameters/enable_adaptive_lmk
echo '80' > /proc/sys/vm/vfs_cache_pressure
echo '0' > /proc/sys/vm/extra_free_kbytes
echo '128' > /sys/block/mmcblk0/queue/read_ahead_kb
echo '128' > /sys/block/mmcblk1/queue/read_ahead_kb
echo '4096' > /proc/sys/vm/min_free_kbytes
echo '0' > /proc/sys/vm/oom_kill_allocating_task
echo '5' > /proc/sys/vm/dirty_ratio
echo '20' > /proc/sys/vm/dirty_background_ratio
chmod 666 /sys/module/lowmemorykiller/parameters/minfree
chown root /sys/module/lowmemorykiller/parameters/minfree
echo '21816,29088,36360,43632,50904,65448' > /sys/module/lowmemorykiller/parameters/minfree
echo '5' > /proc/sys/vm/swappiness
rm /data/system/perfd/default_values
start perfd
```


These can be easily set via other tools or apps that support init.d scripts and build.prop editing but I use Magisk anyway.. so.. why not using it to do the job properly - with successful SafetyNet test    ( :

These changes are basic/fundamental changes in the behavior of Android system (ActivityManager, etc.) and the Linux kernel (LMK) - thus works on almost anything starting from Android 4+ as far as I can tell you. The only bottleneck is the RAM: I recommend at least 2GB or RAM, it has been tested on 3GB and 4GB with AEX 4.6 and ElectraBlue kernel and MIUI EU Dev ROM. 


*NOTE: If you are using MIUI ROM please disable MIUI optimization and MIUI memory optimization because it resets most of these settings. If you use any app that tweaks settings above please uninstall or at least disable them to run and ruin the module's settings.*



More info about Android/Linux memory management on Telagra.ph:
https://telegra.ph/Telegraph-posts-by-crok-05-28
Xiaomi Redmi Note 4X and memory management
https://telegra.ph/Xiaomi-Redmi-Note-4X-and-memory-management-05-28


_Quite honestly speaking I wrote this for myself only,
publicated long time ago and.. haters, please don't ever even install it,
I'm not advertising this module anywhere so please use other mods, hyped high AF.
This one is not doing fine-tuning by dynamic smart scripting or anything,
just my own oldschool settings coming from my of Android and Linux experiences..
I tried to document everything, if you disagree with any of the settings then either
do not install the mod or fork it and set it to your own value - freedom to do it is yours, too._
