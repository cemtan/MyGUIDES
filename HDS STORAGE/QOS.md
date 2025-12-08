### QOS
---



VSP5000 serisi storage'larda LDEV bazında QoS tanımı da geldi.
Bu tanım yalnızca CCI üzerinden yapılabiliyor. **WWN bazlı QoS tanımı yapıldığında LDEV bazlı tanım yapılamıyor, LDEV bazlı tanım yapıldığında WWN bazlı tanım yapılamıyor.**


#### LDEV bazında tanım 
---

İki seçenek var. Ya QoS group tanımları üzerinden ilerleniyor ya da doğrudan LDEV bazında tanım yapılıyor. Tüm liste şöyle:

1. **QoS Group Bazında Tanım**
---

Toplamda 2048 QoS group yaratılabiliyor. Bir QoS grup içinde en fazla 4096 LDEV olabiliyor.
QoS gruba verilen limit, group içindeki LDEV'lerin toplam limiti oluyor.
Örneğin içinde 10 LDEV olan bir gruba 5000 IOPS izni verildiğinde 10 LDEV'in toplam IOPS değeri 5000'i geçemiyor.

Komutlar genel olarak şöyle:
###### Creating group and adding LDEV to the group:
        raidcom add qos_grp -qos_grp_id <qos group#> [-ldev_id <ldev#>] -request_id auto
###### Setting IO limits
		raidcom modify qos_grp -qos_grp_id <qos group#> {-upper_throughput_io <upper throughput io> | -upper_data_trans_mb <upper data trans mb> | -upper_alert_time <upper alert time>| -response_priority <#priority> | -response_alert_time <response alert time>} -request_id auto
###### Deleting group
		raidcom delete qos_grp -qos_grp_id <qos group#> [-ldev_id <ldev#>] -request_id auto
###### Getting group information
		raidcom get qos_grp [-qos_grp_id <qos group#>] [-key <resource | monitor>][-time_zone <time zone>]

###### Örnek:
		raidcom add qos_grp -qos_grp_id 1 -request_id auto
		raidcom add qos_grp -qos_grp_id 1 -ldev_id 101 -request_id auto
		raidcom add qos_grp -qos_grp_id 1 -ldev_id 102 -request_id auto
		raidcom add qos_grp -qos_grp_id 1 -ldev_id 103 -request_id auto
		raidcom modify qos_grp -qos_grp_id 1 -upper_data_trans_mb 100 -request_id auto

2. **LDEV Bazında Tanım**
---
   
Komutlar genel olarak şöyle:
###### Setting limit
		raidcom modify ldev -ldev_id <ldev#> {-status <status> [<level>] [-forcible -password <One Time Password>] | -ldev_name <ldev naming> | -mp_blade_id <mp#> | -ssid <value> | -command_device {y | n} [Security value] | -quorum_enable <serial#> <id> -quorum_id <quorum id> | -quorum_disable | -alua {enable|disable} | -capacity_saving <capacity saving> | -capacity_saving_mode <saving mode>| -compression_acceleration {enable | disable} -request_id auto | -upper_throughput_io<upper throughput io> -request_id auto | -upper_data_trans_mb <upperdata trans mb> -request_id auto | -upper_alert_time <upper alert time> -request_id auto | -lower_throughput_io <lower throughput io> -request_id auto | -lower_data_trans_mb <lower data trans mb> -request_id auto | -lower_alert_time <lower alert time> -request_id auto | -response_priority <priority> -request_id auto | -response_alert_time <response alert time> -request_id auto  | -ese {enable | disable} -request_id auto}
###### Getting limit
		raidcom get ldev {-ldev_id <ldev#> ... [-cnt <count>] | -grp_opt <group option> -device_grp_name <device group name> [<device name>] | -ldev_list <ldev list option>} [-key <keyword>][{-check_status | -check_status_not} <string>... [-time <time>]] [-time_zone <time zone>]

###### Örnek:
	* Setting the upper limit of the data transfer volume to 100 MB/s for LDEV ID: 200:
		raidcom modify ldev -ldev_id 200 -upper_data_trans_mb 100 -request_id auto
	* Setting the lower limit of the throughput per second to 1500 IOPS for LDEV ID: 200:
		raidcom modify ldev -ldev_id 200 -lower_throughput_io 1500 -request_id auto
	* Getting Qos:
		raidcom get ldev –ldev_id 200 –key qos
		raidcom get ldev -ldev_id 200 -key qos_monitor



#### SPM Group Bazında Tanım
---

###### 1. Porttaki WWN lerin priorty'leri No yapılır.
    raidcom modify spm_wwn -port CL1-D -spm_priority n -hba_wwn 51402EC0017547D6 -I003
    raidcom modify spm_wwn -port CL2-D -spm_priority n -hba_wwn 51402EC0017547D6 -I003
    raidcom modify spm_wwn -port CL3-C -spm_priority n -hba_wwn 51402EC001CACD6E -I003
    raidcom modify spm_wwn -port CL4-C -spm_priority n -hba_wwn 51402EC001CACD6E -I003

###### 2. WWN lerin priority bilgi kontrolü
    raidcom get spm_wwn -port CL1-D -hba_wwn 51402EC0017547D6 -I003
    raidcom get spm_wwn -port CL2-D -hba_wwn 51402EC0017547D6 -I003
    raidcom get spm_wwn -port CL3-C -hba_wwn 51402EC001CACD6E -I003
    raidcom get spm_wwn -port CL4-C -hba_wwn 51402EC001CACD6E -I003

###### 3. SPM (QOS) grubuna ekleme.Her WWN için bir portta yapmak yeterli.Aynı WWN başka portlarda var ise onlar otomatik ekleniyor.
    SPM name must be unique.If more than one WWN exists on same port add some prefix to end of name.
    raidcom add spm_wwn -port CL1-D -spm_name MNG_SUBETNETDG_1D -hba_wwn 51402EC0017547D6 -I003
    raidcom add spm_wwn -port CL3-C -spm_name MNG_SUBETNETDG_3C -hba_wwn 51402EC001CACD6E -I003

###### 4. Yapılan tanımın detaylarını görme
    raidcom get spm_wwn -port CL1-D -I003
    raidcom get spm_wwn -port CL2-D -I003
    raidcom get spm_wwn -port CL3-C -I003
    raidcom get spm_wwn -port CL4-C -I003

    Örnek çıktı:
    [root@HDS-EF-GBZ-OPSCENTER ~]# raidcom get spm_wwn -port CL1-D -I003
    PORT    SPM_MD           SPM_WWN  NICK_NAME        GRP_NAME          Serial#
    CL1-D   WWN     51402ec0017547d6  MNG_SUBETNETDG_1D MNG_SUBETNETDG     541003

###### 5. Yukarıdaki adımlarda tanımlanan WWN lerin SPM grubuna atanması.Her porttaki tüm WWN ler için olması gerekiyor.
    raidcom add spm_group -port CL1-D -spm_group MNG_SUBETNETDG -hba_wwn 51402EC0017547D6 -I003
    raidcom add spm_group -port CL2-D -spm_group MNG_SUBETNETDG -hba_wwn 51402EC0017547D6 -I003
    raidcom add spm_group -port CL3-C -spm_group MNG_SUBETNETDG -hba_wwn 51402EC001CACD6E -I003
    raidcom add spm_group -port CL4-C -spm_group MNG_SUBETNETDG -hba_wwn 51402EC001CACD6E -I003

###### 6. IO limitini ayarlamak için kullanılan komut.
    SPM grubun içindeki bir port için yapmak yeterli.Gruba dahil tüm port/WWN lere aynı tanımı uyguluyor.
    raidcom modify spm_group -spm_group MNG_SUBETNETDG -port CL1-D -spm_priority n -limit_io 5000 -I003
    raidcom modify spm_group -spm_group MNGESX -port CL1-D -spm_priority n -limit_io 50000 -I003

###### 7. Tanımlanan IO limit kontrolü.
    raidcom get spm_group -port CL1-D -spm_group MNG_SUBETNETDG -I003
    raidcom get spm_group -port CL2-D -spm_group MNG_SUBETNETDG -I003

    Porta tanımlı tüm spm groupları görmek için
    raidcom get spm_wwn -port CL1-D -I003

###### 8. Yapılan tanımların real time olarak datasını görme.
    raidcom monitor spm_group -spm_group MNG_SUBETNETDG -I003
    raidcom monitor spm_wwn -hba_wwn 51402EC0017547D6 -I003
    raidcom monitor spm_wwn -hba_wwn 51402EC001CACD6E -I003
    raidcom monitor spm_wwn -hba_wwn 51402ec00182466e -I003

    raidcom get spm_ldev -hba_wwn 51402EC0017547D6 -I003

###### 9. Tanımlanmış limiti değiştirme.Online işlem.
    raidcom modify spm_wwn -port CL1-D -spm_priority n -limit_io 1650 -hba_wwn 51402EC0017547D6 -I003

