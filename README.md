# sriov-createvf

物理マシン起動時にSystemdからSR-IOV対応デバイスのバーチャルファンクションを作成します。

## インストール

env/sriov-createvfはDebian/Ubuntuでは/etc/defaultに、RHEL系は/etc/sysconfigに置くと良いと思います。以下はDebian/Ubuntuでのファイル配置です。

    $ sudo cp -p sriov-createvf /usr/local/bin/
    $ sudo cp -p env/sriov-createvf /etc/default/
    $ sudo cp -p unit/sriov-createvf.service /etc/systemd/system/

スクリプトに実行権限を与えます。

    $ sudo chmod +x /usr/local/bin/sriov-createvf

RHEL系は/etc/systemd/system/sriov-createvf.serviceのEnvironmentFileのパスを変更してください。

    EnvironmentFile=-/etc/sysconfig/sriov-createvf

## バーチャルファンクション設定

/etc/default/sriov-createvfを編集し、DEVICESに対象デバイスのPCIデバイス番号と作成するバーチャルファンクション数を'#'で挟んで記述します。PCIデバイス番号はlspci -Dのフォーマット DOMAIN:BUS:DEVICE.FUNCTION に準拠します。

    DEVICES="0000:06:00.0#4"
 
空白文字区切りで複数のデバイスを指定できます。

    DEVICES="0000:08:00.0#2 0000:08:00.1#2"

## Systemdで有効にする

systemctlコマンドで有効化します。

    $ sudo systemctl enable sriov-createvf.service

