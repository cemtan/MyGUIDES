#### FAIL OVER
---
---



Use the following procedure to reverse the GAD P-VOL and S-VOL when sharing GAD volumes with UR in a GAD 3DC delta resync (GAD+UR) configuration.

##### 1. Suspend the GAD pair by specifying the S-VOL (swap suspend).
	pairsplit -g oraHA -RS -IH0

	
##### 2. Re-synchronize the GAD pair by specifying the S-VOL (swap re-sync).
	pairresync -g oraHA -swaps -IH0
	
If a failure occurs after the one volume capacity of a GAD pair can be expanded, the creation, resync, swap resync, and horctakeover operations of the GAD pair cannot be performed because the capacity of both the volumes is not the same. Make sure to expand the other volume capacity so that the capacity of both the volumes is the same, and then retry the operation.
The volume on the primary storage system changes to a P-VOL, and the volume on the GAD secondary storage system changes to an S-VOL.


##### 3. Keep updating I/O from the server to the P-VOL or S-VOL of the GAD pair for about two minutes.


##### 4. Confirm that the delta UR P-VOL pair status is PSUS.
	pairdisplay -g oraDELTA -fxce -IH0
	
	Group		PairVol(L/R) 	(Port#,TID, LU),	Seq#, 		LDEV#.P/S,		Status,		Fence, 		%, 		P-LDEV# 	M 	CTG 	JID 	AP 	EM 	E-Seq# 	E-LDEV# 	R/W
	oraDELTA 	dev2(L)  	(CL1-A-1, 0, 1) 		511111 	2222. P-VOL 	PSUS 		ASYNC ,	0 	6666 	- 	0 		0 		- 	- 		- 			- 			-/-
	oraDELTA 	dev2(R)		(CL1-A-1, 0, 1) 		544444 	6666. S-VOL	SSUS 		ASYNC ,	0		2222		- 	0 		0 		- 	- 		- 			- 			-/-

**Note**: To check the status of a pair in Device Manager - Storage Navigator, select Refresh All in the File menu, update the information displayed on Device Manager - Storage Navigator, and then view the pair status. The status of the UR delta resync pairs changes from HOLDING to HOLD.


##### 5. Confirm that the mirror status of the journal of the UR delta resync pair is PJNS.
	pairdisplay -g oraDELTA -v jnl -IH0
	
	JID		MU	CTG	JNLS	AP	U(%)	Q-Marker	Q-CNT	D-SZ(BLK)	Seq#	Num	LDEV#
	000 	1		1		PJNS 	4	21		43216fde	30		512345	62500	1		39321

	
##### 6. Confirm that no failure SIMs are displayed.
