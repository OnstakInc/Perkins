# Perkins
Perkins Coie UCSD Project
**********
README.txt

Folder Name: Perkins_Latest
Workflows: 
		DR_Initiate Fail-Over
		Break NFS SNAP Mirrors
		Break_CIFS_SNAP_Mirrors
		Break_ISCSI_SNAP_Mirrors
		Break_ISCSI_Snapmirrors_BreakRelation
		Update DNS
		Activate SQL AG and DR Copies
		VM Multi Selector PowerOff
		VM Multi Selector PowerOn
		RestoreSnapshot_GetClusterVolumeSnapshot
		RestoreSnapshot_GetSnapshotname
		Find_Restore_Snapshot

Workflow Name: DR_Initiate Fail-Over
	Workflow Description: Main workflow used to be initiated 
		Dependent: -
		Successor:  Break NFS SNAP Mirrors
					Break_CIFS_SNAP_Mir5tghn4rors
					Break_ISCSI_SNAP_Mirrors
					Break_ISCSI_Snapmirrors_BreakRelation
					Update DNS
					Activate SQL AG and DR Copies
					VM Multi Selector PowerOff
					VM Multi Selector PowerOn
	Workflow Inputs: 
					vCenterList - Single Select from dropdown.
					vCenterUser - Username 
					vCenterPassword - Password
					VM_Operation - On or off
					FolderName - Name of the complete folder structure
					BatchSize - Number of VMs to be acted 
					Delay - Delay the execution in secs
					Run_NFS_Break - True or False
					Run_CIFS_Break - True or False
					Run_ISCSI_Break - True or False
					DRCopies - True or False
					DNSUpdate - True or False
					DestinationVolumePrefix - Prefix to be used for DestinationVolume
					SVMName - Prefix to be used for SVMName
					
Workflow Name: Break NFS SNAP Mirrors
	Workflow Description: Workflow to break NFS Snapmirrors
		Dependent: DR_Initiate Fail-Over
		Successor:  
		
	Workflow Inputs: 
					IsStepToPerformTrue - false
					TDR_SnapMirror_A 
					TDR_SnapMirror_B
					SM_Priority_Normal
					SM_Xfer_Rate
					SM_Action_Resync
					SM_Action_Break
					
Workflow Name: Break_CIFS_SNAP_Mir5tghn4rors
	Workflow Description: Workflow to break CIFS Snapmirrors
						  Perkins team will add the required tasks here.
		Dependent: DR_Initiate Fail-Over
		Successor:  
		
	Workflow Inputs: 
	
Workflow Name: Break_ISCSI_SNAP_Mirrors
	Workflow Description: Workflow to break ISCSI Snapmirrors
		Dependent: DR_Initiate Fail-Over
		Successor: Break_ISCSI_Snapmirrors_BreakRelation
		
	Workflow Inputs: SVMName
					 DestinationVolume
					 DescriptionVolume-Prefix
					 
Workflow Name: Break_ISCSI_Snapmirrors_BreakRelation
	Workflow Description: Workflow to break ISCSI Snapmirror relationship, this is called from a java script task from dependent workflow.
		Dependent: Break_ISCSI_SNAP_Mirrors
		Successor: 
		
	Workflow Inputs: SnapMirrorName

Workflow Name: UpdateDNS
	Workflow Description: Workflow to update DNS records, Perkins team to populate the required tasks.
		Dependent: DR_Initiate Fail-Over
		Successor: 
		
	Workflow Inputs: DNS Server
					 DNS Username
					 DNS Password
	
Workflow Name: Activate SQL AG and DR Copies
	Workflow Description: Workflow to perform SQL activities, Perkins team to update the required tasks in the workflow.
		Dependent:DR_Initiate Fail-Over
		Successor: 
		
	Workflow Inputs: -
	
Workflow Name: VM Multi Selector PowerOff
	Workflow Description: Workflow to perform power off operation to multiple VMs in a batch in each folder structure given as input.
						  Initiated by a powershell called from DR_Initiate Fail-Over as the last step.
		Dependent:DR_Initiate Fail-Over
		Successor: 
		
	Workflow Inputs: - Select VMs
	
Workflow Name: VM Multi Selector PowerOn
	Workflow Description: Workflow to perform power on operation to multiple VMs in a batch in each folder structure given as input.
						  Initiated by a powershell called from DR_Initiate Fail-Over as the last step.
		Dependent:DR_Initiate Fail-Over
		Successor: 
		
	Workflow Inputs: - Select VMs
	
Workflow Name: RestoreSnapshot_GetClusterVolumeSnapshot
	Workflow Description: Workflow to get volume identity. An API call to get required cluster volume identity. 
						  A java script takes the input of cluster volume identity and initiates the RestoreSnapshot_GetSnapshotname workflow.
		Dependent:DR_Initiate Fail-Over
		Successor: RestoreSnapshot_GetSnapshotname
		
	Workflow Inputs: - SVMName	
					   DestinationVolume
					   DescriptionVolume-Prefix
					   SnapshotNamePrefix

Workflow Name: RestoreSnapshot_GetSnapshotname
	Workflow Description: Workflow to get latest snapshot name taking in a prefix. 
						  Once the volume identity is identified, for each volumeidentity, 
						  latest snapshotname is checked and restore action is initiated.
		Dependent: RestoreSnapshot_GetClusterVolumeSnapshot
		Successor: Find_Restore_Snapshot
		
	Workflow Inputs: - SVMName	
					   DestinationVolume
					   ReportContextID
					   SnapshotNamePrefix

Workflow Name: Find_Restore_Snapshot
	Workflow Description: Workflow to perform restore action to the latest snapshot.
		Dependent: RestoreSnapshot_GetSnapshotname
		Successor: 
		
	Workflow Inputs: - volumename
					   snapshotname
	
