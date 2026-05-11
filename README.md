# Graphics Enhancement Driver & FPSGo

MediaTek graphics enhancement isn't typically a single "driver" file you install manually. Instead, it is a combination of hardware-level features and system-level software optimizations.

## Check Driver Availability
```bash
( Device Redmi 10 5G HyperOS A14 )

#Using Terminal Emulator or Termux ( GED )
su
ls /sys/module/ged/parameters/

#For result command ( supported GED ) 
ap_self_frc_detection_rate     ged_smart_boost
boost_amp                      gpu_block
boost_extra                    gpu_bottom_freq
boost_gpu_enable               gpu_bw_err_debug
boost_upper_bound              gpu_cust_boost_freq
cpu_boost_policy               gpu_cust_upbound_freq
deboost_reduce                 gpu_debug_enable
enable_cpu_boost               gpu_dvfs_enable
enable_game_self_frc_detect    gpu_idle
enable_gpu_boost               gpu_loading
g_fb_dvfs_threshold            gx_boost_on
g_gpu_timer_based_emu          gx_dfps
ged_boost_enable               gx_fb_dvfs_margin
ged_force_mdp_enable           gx_force_cpu_boost
ged_log_perf_trace_enable      gx_frc_mode
ged_log_trace_enable           gx_game_mode
ged_monitor_3D_fence_debug     gx_top_app_pid
ged_monitor_3D_fence_disable   is_GED_KPI_enabled
ged_monitor_3D_fence_systrace  target_t_cpu_remained

#Using Terminal Emulator or Termux ( FPSGo )
su
ls /sys/kernel/fpsgo

#For result command ( supported FPSGo )
common  fbt  fstb  minitop  xgf

#Using Terminal Emulator or Termux ( MTK PPM )
su
cat /proc/ppm/policy_status

#For result command ( supported MTK PPM )
[0] PPM_POLICY_PTPOD: disabled
[1] PPM_POLICY_UT: disabled
[3] PPM_POLICY_FORCE_LIMIT: disabled
[4] PPM_POLICY_PWR_THRO: disabled
[5] PPM_POLICY_THERMAL: disabled
[6] PPM_POLICY_DLPT: disabled
[7] PPM_POLICY_HARD_USER_LIMIT: enabled
[8] PPM_POLICY_USER_LIMIT: disabled
[9] PPM_POLICY_LCM_OFF: disabled
[2] PPM_POLICY_SYS_BOOST: enabled

Usage: echo <idx> <1/0> > /proc/ppm/policy_status

#Using Terminal Emulator or Termux ( GPUFREQ )
su
cat /proc/gpufreq/gpufreq_limit_table

#For result command ( supported GPUFREQ )
echo [id][up_enable][low_enable] > /proc/gpufreq/gpufreq_limit_table
ex: echo 3 0 0 > /proc/gpufreq/gpufreq_limit_table
means disable THERMAL upper_limit_idx & lower_limit_idx

         [name]  [id]     [prio]   [up_idx] [up_enable]  [low_idx] [low_enable]
         STRESS     0          8         -1          1         -1          1
           PROC     1          7         -1          1         -1          1
          PTPOD     2          6         -1          1         -1          1
        THERMAL     3          5         -1          0         -1          1
        BATT_OC     4          5         -1          0         -1          1
       BATT_LOW     5          5         -1          0         -1          1
   BATT_PERCENT     6          5         -1          0         -1          1
            PBM     7          5          0          0         -1          1
         POLICY     8          4         -1          1         -1          1


```

## What Has Been Changed:
```bash
#services.sh
#Tweak with Graphics Enhancement Driver And FPSGo
echo 1 > /sys/module/ged/parameters/gx_frc_mode
echo 1 > /sys/module/ged/parameters/gx_boost_on
echo 1 > /sys/module/ged/parameters/gx_game_mode
echo 1 > /sys/module/ged/parameters/cpu_boost_policy
echo 1 > /sys/module/ged/parameters/boost_extra
echo 1 > /sys/module/ged/parameters/ged_smart_boost
echo 1 > /sys/module/ged/parameters/ged_boost_enable
echo 0 > /sys/module/ged/parameters/is_GED_KPI_enabled
echo 101 > /sys/kernel/ged/hal/gpu_boost_level
echo 1 > /sys/kernel/fpsgo/fbt/boost_ta
echo 0 > /sys/kernel/fpsgo/fstb/adopt_low_fps
echo 2 > /sys/kernel/fpsgo/fbt/llf_task_policy

# RTMM
echo 800 > /sys/kernel/mm/rtmm/reclaim/auto_reclaim_max
echo 2048 > /sys/kernel/mm/rtmm/reclaim/global_reclaim_max
echo 80 > /sys/kernel/mm/rtmm/reclaim/default_reclaim_swappiness
echo true > /sys/kernel/mm/swap/vma_ra_enabled

#Sleep 2 minute
sleep 120

#MTK PPM
echo 2 1 > /proc/ppm/policy_status
echo 5 0 > /proc/ppm/policy_status
echo 4 0 > /proc/ppm/policy_status
echo 6 0 > /proc/ppm/policy_status

#GPUFREQ Disable Thermal
echo 3 0 1 > /proc/gpufreq/gpufreq_limit_table
echo 4 0 1 > /proc/gpufreq/gpufreq_limit_table
echo 5 0 1 > /proc/gpufreq/gpufreq_limit_table
echo 6 0 1 > /proc/gpufreq/gpufreq_limit_table
echo 7 0 1 > /proc/gpufreq/gpufreq_limit_table


```

## Compatibility

- Device with MediaTek processor and Mali GPU
- Android 10.0 or higher
- Kernel version 4.14 or higher
- Magisk 26 or higher

## Download

- Download from the [release page](https://github.com/sanndyrmdhn/Graphics-Enhancement-Driver/releases)
- Download and flash the zip in magisk manager ( Not tested in KSU and APatch )
- Reboot the device
