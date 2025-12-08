#### ZONES
---
---



##### AKTIFBANK EXAMPLE
---

	CL1-A	50:06:0E:80:21:A7:51:00	SAN1
	CL1-B	50:06:0E:80:21:A7:51:01	SAN1
	CL2-A	50:06:0E:80:21:A7:51:10	SAN1
	CL2-B	50:06:0E:80:21:A7:51:11	SAN1
	CL3-A	50:06:0E:80:21:A7:51:20	SAN2
	CL3-B	50:06:0E:80:21:A7:51:21	SAN2
	CL4-A	50:06:0E:80:21:A7:51:30	SAN2
	CL4-B	50:06:0E:80:21:A7:51:31	SAN2

###### Switch1
---

	alicreate "A_Hitachi_E790_1A", "50:06:0E:80:21:A7:51:00"
	alicreate "A_Hitachi_E790_1B", "50:06:0E:80:21:A7:51:01"
	alicreate "A_Hitachi_E790_2A", "50:06:0E:80:21:A7:51:10"
	alicreate "A_Hitachi_E790_2B", "50:06:0E:80:21:A7:51:11"

	zonecreate 'Z_sprck101pesx01_E790', 'A_sprck101pesx01;A_Hitachi_E790_1A;A_Hitachi_E790_2A'
	zonecreate 'Z_sprck101pesx03_E790', 'A_sprck101pesx03;A_Hitachi_E790_1A;A_Hitachi_E790_2A'
	zonecreate 'Z_sprck101pesx04_E790', 'A_sprck101pesx04;A_Hitachi_E790_1A;A_Hitachi_E790_2A'
	zonecreate 'Z_sprck101pesx07_E790', 'A_sprck101pesx07;A_Hitachi_E790_1A;A_Hitachi_E790_2A'
	zonecreate 'Z_sprck101pesx08_E790', 'A_sprck101pesx08;A_Hitachi_E790_1A;A_Hitachi_E790_2A'
	zonecreate 'Z_sprck101pesx10_E790', 'A_sprck101pesx10;A_Hitachi_E790_1A;A_Hitachi_E790_2A'
	zonecreate 'Z_sprck101pesx11_E790', 'A_sprck101pesx11;A_Hitachi_E790_1A;A_Hitachi_E790_2A'
	zonecreate 'Z_sprck101pesx12_E790', 'A_sprck101pesx12;A_Hitachi_E790_1A;A_Hitachi_E790_2A'
	zonecreate 'Z_sprck101pesx14_E790', 'A_sprck101pesx14;A_Hitachi_E790_1A;A_Hitachi_E790_2A'
	zonecreate 'Z_sprck101pesx15_E790', 'A_sprck101pesx15;A_Hitachi_E790_1A;A_Hitachi_E790_2A'
	zonecreate 'Z_sprck101pesx18_E790', 'A_sprck101pesx18;A_Hitachi_E790_1A;A_Hitachi_E790_2A'
	zonecreate 'Z_sprck101pesx25_E790', 'A_sprck101pesx25;A_Hitachi_E790_1A;A_Hitachi_E790_2A'
	zonecreate 'Z_sprck101pesx26_E790', 'A_sprck101pesx26;A_Hitachi_E790_1A;A_Hitachi_E790_2A'

	zonecreate 'Z_sprck101pesx02_E790', 'A_sprck101pesx02;A_Hitachi_E790_1B;A_Hitachi_E790_2B'
	zonecreate 'Z_sprck101pesx05_E790', 'A_sprck101pesx05;A_Hitachi_E790_1B;A_Hitachi_E790_2B'
	zonecreate 'Z_sprck101pesx06_E790', 'A_sprck101pesx06;A_Hitachi_E790_1B;A_Hitachi_E790_2B'
	zonecreate 'Z_sprck101pesx09_E790', 'A_sprck101pesx09;A_Hitachi_E790_1B;A_Hitachi_E790_2B'
	zonecreate 'Z_sprck101pesx13_E790', 'A_sprck101pesx13;A_Hitachi_E790_1B;A_Hitachi_E790_2B'
	zonecreate 'Z_sprck101pesx16_E790', 'A_sprck101pesx16;A_Hitachi_E790_1B;A_Hitachi_E790_2B'
	zonecreate 'Z_sprck101pesx17_E790', 'A_sprck101pesx17;A_Hitachi_E790_1B;A_Hitachi_E790_2B'
	zonecreate 'Z_sprck101pesx19_E790', 'A_sprck101pesx19;A_Hitachi_E790_1B;A_Hitachi_E790_2B'
	zonecreate 'Z_sprck101pesx20_E790', 'A_sprck101pesx20;A_Hitachi_E790_1B;A_Hitachi_E790_2B'
	zonecreate 'Z_sprck101pesx21_E790', 'A_sprck101pesx21;A_Hitachi_E790_1B;A_Hitachi_E790_2B'
	zonecreate 'Z_sprck101pesx22_E790', 'A_sprck101pesx22;A_Hitachi_E790_1B;A_Hitachi_E790_2B'
	zonecreate 'Z_sprck101pesx23_E790', 'A_sprck101pesx23;A_Hitachi_E790_1B;A_Hitachi_E790_2B'
	zonecreate 'Z_sprck101pesx24_E790', 'A_sprck101pesx24;A_Hitachi_E790_1B;A_Hitachi_E790_2B'

	cfgsave
	cfgadd 'cfg_ODMSANSW01', 'Z_sprck101pesx01_E790'
	cfgadd 'cfg_ODMSANSW01', 'Z_sprck101pesx03_E790'
	cfgadd 'cfg_ODMSANSW01', 'Z_sprck101pesx04_E790'
	cfgadd 'cfg_ODMSANSW01', 'Z_sprck101pesx07_E790'
	cfgadd 'cfg_ODMSANSW01', 'Z_sprck101pesx08_E790'
	cfgadd 'cfg_ODMSANSW01', 'Z_sprck101pesx10_E790'
	cfgadd 'cfg_ODMSANSW01', 'Z_sprck101pesx11_E790'
	cfgadd 'cfg_ODMSANSW01', 'Z_sprck101pesx12_E790'
	cfgadd 'cfg_ODMSANSW01', 'Z_sprck101pesx14_E790'
	cfgadd 'cfg_ODMSANSW01', 'Z_sprck101pesx15_E790'
	cfgadd 'cfg_ODMSANSW01', 'Z_sprck101pesx18_E790'
	cfgadd 'cfg_ODMSANSW01', 'Z_sprck101pesx25_E790'
	cfgadd 'cfg_ODMSANSW01', 'Z_sprck101pesx26_E790'
	cfgadd 'cfg_ODMSANSW01', 'Z_sprck101pesx02_E790'
	cfgadd 'cfg_ODMSANSW01', 'Z_sprck101pesx05_E790'
	cfgadd 'cfg_ODMSANSW01', 'Z_sprck101pesx06_E790'
	cfgadd 'cfg_ODMSANSW01', 'Z_sprck101pesx09_E790'
	cfgadd 'cfg_ODMSANSW01', 'Z_sprck101pesx13_E790'
	cfgadd 'cfg_ODMSANSW01', 'Z_sprck101pesx16_E790'
	cfgadd 'cfg_ODMSANSW01', 'Z_sprck101pesx17_E790'
	cfgadd 'cfg_ODMSANSW01', 'Z_sprck101pesx19_E790'
	cfgadd 'cfg_ODMSANSW01', 'Z_sprck101pesx20_E790'
	cfgadd 'cfg_ODMSANSW01', 'Z_sprck101pesx21_E790'
	cfgadd 'cfg_ODMSANSW01', 'Z_sprck101pesx22_E790'
	cfgadd 'cfg_ODMSANSW01', 'Z_sprck101pesx23_E790'
	cfgadd 'cfg_ODMSANSW01', 'Z_sprck101pesx24_E790'

	cfgsave
	cfgenable 'cfg_ODMSANSW01'


###### Switch2
---

	alicreate "A_Hitachi_E790_3A", "50:06:0E:80:21:A7:51:20"
	alicreate "A_Hitachi_E790_3B", "50:06:0E:80:21:A7:51:21"
	alicreate "A_Hitachi_E790_4A", "50:06:0E:80:21:A7:51:30"
	alicreate "A_Hitachi_E790_4B", "50:06:0E:80:21:A7:51:31"

	zonecreate 'Z_sprck101pesx01_E790', 'A_sprck101pesx01;A_Hitachi_E790_3B;A_Hitachi_E790_4B'
	zonecreate 'Z_sprck101pesx03_E790', 'A_sprck101pesx03;A_Hitachi_E790_3B;A_Hitachi_E790_4B'
	zonecreate 'Z_sprck101pesx04_E790', 'A_sprck101pesx04;A_Hitachi_E790_3B;A_Hitachi_E790_4B'
	zonecreate 'Z_sprck101pesx07_E790', 'A_sprck101pesx07;A_Hitachi_E790_3B;A_Hitachi_E790_4B'
	zonecreate 'Z_sprck101pesx08_E790', 'A_sprck101pesx08;A_Hitachi_E790_3B;A_Hitachi_E790_4B'
	zonecreate 'Z_sprck101pesx10_E790', 'A_sprck101pesx10;A_Hitachi_E790_3B;A_Hitachi_E790_4B'
	zonecreate 'Z_sprck101pesx11_E790', 'A_sprck101pesx11;A_Hitachi_E790_3B;A_Hitachi_E790_4B'
	zonecreate 'Z_sprck101pesx12_E790', 'A_sprck101pesx12;A_Hitachi_E790_3B;A_Hitachi_E790_4B'
	zonecreate 'Z_sprck101pesx14_E790', 'A_sprck101pesx14;A_Hitachi_E790_3B;A_Hitachi_E790_4B'
	zonecreate 'Z_sprck101pesx15_E790', 'A_sprck101pesx15;A_Hitachi_E790_3B;A_Hitachi_E790_4B'
	zonecreate 'Z_sprck101pesx18_E790', 'A_sprck101pesx18;A_Hitachi_E790_3B;A_Hitachi_E790_4B'
	zonecreate 'Z_sprck101pesx25_E790', 'A_sprck101pesx25;A_Hitachi_E790_3B;A_Hitachi_E790_4B'
	zonecreate 'Z_sprck101pesx26_E790', 'A_sprck101pesx26;A_Hitachi_E790_3B;A_Hitachi_E790_4B'

	zonecreate 'Z_sprck101pesx02_E790', 'A_sprck101pesx02;A_Hitachi_E790_3A;A_Hitachi_E790_4A'
	zonecreate 'Z_sprck101pesx05_E790', 'A_sprck101pesx05;A_Hitachi_E790_3A;A_Hitachi_E790_4A'
	zonecreate 'Z_sprck101pesx06_E790', 'A_sprck101pesx06;A_Hitachi_E790_3A;A_Hitachi_E790_4A'
	zonecreate 'Z_sprck101pesx09_E790', 'A_sprck101pesx09;A_Hitachi_E790_3A;A_Hitachi_E790_4A'
	zonecreate 'Z_sprck101pesx13_E790', 'A_sprck101pesx13;A_Hitachi_E790_3A;A_Hitachi_E790_4A'
	zonecreate 'Z_sprck101pesx16_E790', 'A_sprck101pesx16;A_Hitachi_E790_3A;A_Hitachi_E790_4A'
	zonecreate 'Z_sprck101pesx17_E790', 'A_sprck101pesx17;A_Hitachi_E790_3A;A_Hitachi_E790_4A'
	zonecreate 'Z_sprck101pesx19_E790', 'A_sprck101pesx19;A_Hitachi_E790_3A;A_Hitachi_E790_4A'
	zonecreate 'Z_sprck101pesx20_E790', 'A_sprck101pesx20;A_Hitachi_E790_3A;A_Hitachi_E790_4A'
	zonecreate 'Z_sprck101pesx21_E790', 'A_sprck101pesx21;A_Hitachi_E790_3A;A_Hitachi_E790_4A'
	zonecreate 'Z_sprck101pesx22_E790', 'A_sprck101pesx22;A_Hitachi_E790_3A;A_Hitachi_E790_4A'
	zonecreate 'Z_sprck101pesx23_E790', 'A_sprck101pesx23;A_Hitachi_E790_3A;A_Hitachi_E790_4A'
	zonecreate 'Z_sprck101pesx24_E790', 'A_sprck101pesx24;A_Hitachi_E790_3A;A_Hitachi_E790_4A'

	cfgsave
	cfgadd 'cfg_ODMSANSW02', 'Z_sprck101pesx01_E790'
	cfgadd 'cfg_ODMSANSW02', 'Z_sprck101pesx03_E790'
	cfgadd 'cfg_ODMSANSW02', 'Z_sprck101pesx04_E790'
	cfgadd 'cfg_ODMSANSW02', 'Z_sprck101pesx07_E790'
	cfgadd 'cfg_ODMSANSW02', 'Z_sprck101pesx08_E790'
	cfgadd 'cfg_ODMSANSW02', 'Z_sprck101pesx10_E790'
	cfgadd 'cfg_ODMSANSW02', 'Z_sprck101pesx11_E790'
	cfgadd 'cfg_ODMSANSW02', 'Z_sprck101pesx12_E790'
	cfgadd 'cfg_ODMSANSW02', 'Z_sprck101pesx14_E790'
	cfgadd 'cfg_ODMSANSW02', 'Z_sprck101pesx15_E790'
	cfgadd 'cfg_ODMSANSW02', 'Z_sprck101pesx18_E790'
	cfgadd 'cfg_ODMSANSW02', 'Z_sprck101pesx25_E790'
	cfgadd 'cfg_ODMSANSW02', 'Z_sprck101pesx26_E790'
	cfgadd 'cfg_ODMSANSW02', 'Z_sprck101pesx02_E790'
	cfgadd 'cfg_ODMSANSW02', 'Z_sprck101pesx05_E790'
	cfgadd 'cfg_ODMSANSW02', 'Z_sprck101pesx06_E790'
	cfgadd 'cfg_ODMSANSW02', 'Z_sprck101pesx09_E790'
	cfgadd 'cfg_ODMSANSW02', 'Z_sprck101pesx13_E790'
	cfgadd 'cfg_ODMSANSW02', 'Z_sprck101pesx16_E790'
	cfgadd 'cfg_ODMSANSW02', 'Z_sprck101pesx17_E790'
	cfgadd 'cfg_ODMSANSW02', 'Z_sprck101pesx19_E790'
	cfgadd 'cfg_ODMSANSW02', 'Z_sprck101pesx20_E790'
	cfgadd 'cfg_ODMSANSW02', 'Z_sprck101pesx21_E790'
	cfgadd 'cfg_ODMSANSW02', 'Z_sprck101pesx22_E790'
	cfgadd 'cfg_ODMSANSW02', 'Z_sprck101pesx23_E790'
	cfgadd 'cfg_ODMSANSW02', 'Z_sprck101pesx24_E790'

	cfgsave
	cfgenable 'cfg_ODMSANSW02'
