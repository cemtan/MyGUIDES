### FILES
---
---



#### Command.txt
---

	svpip xxx.xxx.xxx.xx            ; Specifies IP address of SVP <===== Change to IP Address of SVP
	login userid "password"         ; Logs user into SVP          <===== Change to predefined Userid/password for exclusive use by export tool
	show                            ; Outputs storing period & gethering interval to standard output
	
	;  +---------------------------------------------------------------------------------------------------+
	;  | Group commands define the data to be exported.
	;  +---------------------------------------------------------------------------------------------------+
	;  | For Physical, there is no need to specify Long or Short - the following will get Long and Short
	;  | Of course, if you specify Short, you only get short!
	;  +---------------------------------------------------------------------------------------------------+
	group PhyPG                     ; Parity Groups
	group PhyLDEV                   ; Logical Volumes
	group PhyProc                   ; Micro-Processor usage
	group PhyProcDetail             ; MPU Performance Information
	group PhyExG                    ; External Volume Group usage
	group PhyExLDEV                 ; External Volume usage
	group PhyMPU                    ; Access Paths and Write Pending
	;  +---------------------------------------------------------------------------------------------------+
	group PG                        ; Parity Group Statistics
	;group LDEV                     ; LDEV usage in PGs, External Volume Groups or V-VOL Groups
	;                               ; Not required when using LDEVEachOfCU
	group Port                      ; Port usage
	group MFPort                    ; MFPort usage
	group PortWWN                   ; Stats for HBAs connected to ports.
	group LU                        ; LDEV usage Summarised by LU Path
	group PPCGWWN                   ; Stats about HBAs
	group RemoteCopy                ; Remote Copy Usage Summarized by Subsystem
	group RCLU                      ; Remote Copy Usage Summarized by LU path
	group RCLDEV                    ; Remote Copy Usage Summarized by LDEV
	group UniversalReplicator       ; Remote Copy Usage by UR Summarized by Subsystem
	group URJNL                     ; Remote Copy Usage by UR Summarized by Journal Group
	group URLU                      ; Remote Copy Usage by UR Summarized by LU Path
	group URLDEV                    ; Remote Copy Usage by UR Summarized by LDEV
	group LDEVEachOfCU              ; LDEV usage in CUs - Recommended
	;  +---------------------------------------------------------------------------------------------------+
	;  | end of group statements
	;  +---------------------------------------------------------------------------------------------------+
	
	;  +---------------------------------------------------------------------------------------------------+
	;  | To limit the data collection within a date/time range, use the following sub-commands:-
	;  | shortrange start_timestamp:end_timestamp
	;  | longrange start_timestamp:end_timestamp
	;  |
	;  | Where start_timestamp and end_timestamp are in the format:- yyyyMMddHHmm
	;  |
	;  | For example:-
	;  |            yyyyMMddHHmm:yyyyMMddHHmm
	;  | shortrange 201907101200:201907111159
	;  | longrange  201907101200:201907111159
	;  |
	;  | The above example will collect shortrange and longrange data between 12:00 on 10th July 2019
	;  | and 11:59 on 11th July 2019
	;  |
	;  | NB - this is the time on the SVP - not on your server - and it may very well be GMT!
	;  |
	;  | Example below says get the latest 24 hours
	;  |            (hhmm format)
	;  | shortrange -2400:
	;  |
	;  | Example below says get the latest 72 hours
	;  |           (ddhhmm format)
	;  | longrange -030000:
	;  +---------------------------------------------------------------------------------------------------+
	shortrange  -2400:
	longrange -030000:
	;  +---------------------------------------------------------------------------------------------------+
	;  | end of time statements
	;  +---------------------------------------------------------------------------------------------------+
	
	outpath out                     ; Specifies the sub-directory in which files will be saved
	option compress                 ; Specifies whether to compress files
	apply                           ; Executes processing for saving monitoring data in files



#### Links
---

https://knowledge.hitachivantara.com/Support_Information/Data_Collection/Storage/Performance_Data_Collection_-_How_to_Run_the_Export_Tool
https://knowledge.hitachivantara.com/Knowledge/Storage/How_to_Download_the_Appropriate_Export_Tool_Version_Specific_to_Array_Microcode
Hitachi Compute Systems Manager
