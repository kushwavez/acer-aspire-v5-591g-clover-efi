# Acer Aspire V 15 V5-591G 黑苹果安装指南 [支持从 macOS Sierra 10.12.6 至 Big Sur 11 的所有版本以及 Windows 10]
 ## 适用于 Acer Aspire V 15 V5-591G 的 Clover EFI - macOS Big Sur 11
 ## 详情请参阅 Insanelymac 上的操作指南: https://www.insanelymac.com/forum/topic/345573-guide-acer-aspire-v-15-v5-591g-from-macos-sierra-10126-to-big-sur-11-windows-10
 <p align=center>
    <img src="https://www.insanelymac.com/uploads/monthly_2020_11/410405936_AspireV5-591G.png.16ecabb3ae2af876860f34c05d34a956.png">
</p>

<p align="center"><b>Acer Aspire V5-591G-XXXX</b></p>
<p align=center>macOS 安装指南 (Clover v5130)</p>
<p>有关详细的参数、方法与安装注意事项，请参阅 Insanelymac</p>

在此项目中，存在两个重要的文件夹：
- <b>bootpack</b>：在使用 USB 存储器进行安装，以及安装完成后所需的 EFI 文件
- <b>postinstall</b>：在系统安装完成后，用于修复线控耳机播放音质的文件

## 安装步骤
- 使用任何你所知道的方法创建一个 USB 安装器
    - 在 Windows 上创建安装器，请参考这个指南：<a href="https://www.insanelymac.com/forum/topic/346703-guide-creating-clover-macos-big-sur-installer-usb-on-windows/">[GUIDE] Creating Clover macOS Big Sur Installer USB on Windows</a>
    - 在新版 macOS 上创建安装器，请从 AppStore 下载系统，或使用<a href="https://github.com/corpnewt/gibMacOS">gibMacOS</a> ，然后通过 Apple 官方网站上的<a href="https://support.apple.com/en-in/HT201372">创建安装介质</a>的方法进行操作
    - 如果是 Sierra 或以下版本的系统，请从 Apple 官方网站下载 dmg 文件
- 挂载 EFI 分区，并从 bootpack 文件夹中复制 CLOVER 和 BOOT 文件夹至 EFI 分区
    - 在 Windows 上，可以通过 MiniTool Partition Wizard 来挂载（译者注：也推荐分区精灵 DiskGenius）
        - 打开 MiniTool ，选择 USB 设备的 EFI 分区，右键选择“更改卷标”并应用
    - 此后，你可以在管理员权限下通过 Explorer++ 管理EFI分区文件
    - 在 macOS 下，你可以使用系统终端来挂载，或者使用 EFI Mountain Snow
    - 如果在 EFI 分区的根目录下没有 EFI 文件夹，请先创建一个
 
文件结构应该是这样的：
<p align=center>
    <img src="https://i.ibb.co/5FZthw6/Picture-1.png">
</p>

- 从USB安装器启动
    - 作者推荐使用 config_debug.plist 来启动，该配置文件启用了 -v (verbose 模式)，可以输出详细的错误信息
    - 在 Clover 中，可以通过按 下NUM 0，或者依次选择 options, Configs, config_debug.plist 来启用 verbose 模式 
- 安装 macOS 
    - 重新启动后，请从USB安装器启动 
        - **如果是安装 Big Sur ，请从 Preboot 启动！**
- 安装完成后，你需要同时挂载 USB 安装器和系统内部的 EFI 分区，并将 USB 安装器的 CLOVER 和 BOOT 文件夹复制到系统的 EFI 分区内
    - 同上，如果在 EFI 分区的根目录下没有 EFI 文件夹，请先创建一个
- 取消挂载 EFI 分区，弹出 USB 安装器并重启系统 

## 安装完成后的步骤
- 使用 Clover Configurator 打开 config.plist 文件，从 SMBIOS 标签创建一个新的 SMBIOS，此后可以登录苹果内置的服务
- 要使音频和 WiFi（前提是拥有 BCM94352Z 网卡）工作，请在系统安装完成后额外安装一些 kext
- 要使线控耳机工作，需要安装 CodecCommander 和 ALCPlugFix 
    - 前往 `postinstall/ALCPlugFix` 文件夹, 将 CodecCommander.kext 安装至 `/Library/Extensions/` ，后使用 install.sh 安装 ALCPlugFix
        - 在终端中定位至 `postinstall/ALCPlugFix` ，然后使用 `sudo ./install.sh` 来安装 
    - 在安装完成后，系统会询问是否允许运行 ALCPlugFix，此时选择允许 
    - 如果这里没有任何弹窗或选项，请在访达中定位到 ALCPlugFix 文件，通过右键打开，此时就会弹出询问对话框。在运行 hda-verb 的时候甚至需要操作 2 次。
- 要修复WiFi，需要将 AirportBrcmFixup, BrcmBluetoothInjector, BrcmFirmwareData, BrcmPatchRAM3 安装到 `CLOVER/kexts/Other` ，它们在 `CLOVER/kexts/Off` 中被禁用  
- 如果一切均已安装完毕，请重设权限并清除 kext 缓存，最后重启系统。推荐使用 Hackintool 操作。
