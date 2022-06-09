# Creation, Submission and Monitor of a Campaign (an example)

In this Markdown page an example of the creation, test and launch of a new production campaign is reported. The main steps are: 
- [Clone an existing campaign](#clone-an-existing-campaign)
- [Add or remove stages and relative dependencies](#add-or-remove-stages-and-relative-dependencies)
- [Configure Campaign](#configure-campaign)
- [Launch test campaign](#launch-test-campaign)
- [Launch campaign](#launch-campaign)
- [Monitor campaign](#monitor-campaign)
- [Kick Campaign Stage to Complete](#kick-campaign-stage-to-complete)
- [Define Samweb dataset](#define-samweb-dataset)

## Clone an existing campaign

The easiest way to create a campaign is to clone an existing one. Once in [POMS Production Webpage](https://pomsgpvm01.fnal.gov/poms/index/icarus/production) click **Campaigns** in the left column. 

![image](images/Campaigns.png)

Then choose an exisitng campaign and click on the symbol in the **Clone Campaign** column,

![image](images/CloneCampaign.png)

type a name for the new campaign (1) and click on **Clone** button (2).

![image](images/CampaignName.png)

Now a new campaign has been cloned from an existing one.

## Add or remove stages and relative dependencies

In order to customize the new campaign, click on the symbol in the **GUI Editor** column of the campaign.

![image](images/GUIEditor.png)

In the page that are going to be open you can customize the campaign. In particular by clicking on **Edit** you can add or remove stages and the relative dependencies.

![image](images/EditCampaignStagesAndDependencies.png)
![image](images/ModifyCampaignStagesAndDependencies.png)

## Configure campaign

A double click on the campaign element opens a window for the overall configuration of the campaign.

![image](images/EditCampaignConfiguration.png)

Here you can configure you campaign. **In particular, it is important to check that software version is the one you want (1) and that it is the same for all the stages**. Once configuration is completed, remember to save the changes (2).

![image](images/ChangeCampaignConfiguration.png)

By double clicking on the stage element, you can customize it.

![image](images/EditStageConfiguration.png)

In the new window, you can check the **software_version** and the **cs_split_type** field (1). Concerning **cs_split_type** for Monte Carlo campaign you can leave **None** while in the case of detector data, a good choice is **drainingn** with **nfiles = 10**. As the explanatory comment says, "*This type, when filled out as draining(n) for some integer n, will pull at most n files at a time from the dataset and deliver them on each iteration, keeping track of the delivered files with a snapshot. This means it works well for datasets that are growing or changing from under it*". 

To override the default parameters (2) for production or test campaign, click on the **Edit** button on the right of the corresponding field.

![image](images/ChangeStageConfiguration.png)

In the new window, you can override the default stage parameters. In particular, you have to check the RAM memory limit (**submit.memory**), the fcl file (**global.fclfile**), the folder name where the campaign output will be stored (**global.sample**) and the number of jobs (**submit.N**) (1).

It is recomended that you check:
- the RAM memory limit is sutable for the stage jobs
- the chosen fcl file exists in */pnfs/icarus/resilient/icaruspro/poms_fcl*
- the **global.sample** is the same for all the stages of the campaign

Once done, save the changes (2),

![image](images/ChangeStageParameters.png)

save the stage configuration.

![image](images/SaveStageConfiguration.png)

and once all the stages are correctly configured, save the campaign configuration.

![image](images/SaveCampaignConfiguration.png)

## Launch test campaign

Before to launch the production campaign is better to verify everything is set up correctly. In order to do so, click on the campaign name.

![image](images/SelectCampaign.png)

Click on the first stage of the campaign.

![image](images/SelectStage.png)

Click on the **Launch Campaign Stage Test Jobs Now** button.

![image](images/LaunchTestCampaign.png)

Now you have launched your campaing in *test* mode. Once you have verified everything is set up correctly you can launch the campaign. Otherwise you have to fix the campaign and relaunch it in *test* mode. A short list of the most common cause of failure/helding is reported [here](#most-common-issues)

## Launch campaign

To Launch the campaign, click the button corresponding to your campaing in the **Launch** column.

![image](images/LaunchProductionCampaign.png)

## Monitor campaign

You can monitor your campaign by clicking on the campaign name.

![image](images/SelectCampaign.png)

Then click on the stage you are interested.

![image](images/SelectStage.png)

Finally, click on **Campaign Stage Stats (Landscape)** link.

![image](images/Landscape.png)

The new window report the statistics of the stage. In the bottom part of the page, you can find the stage overall status and its submission id.

![image](images/JobID.png)

By clicking on the submission id, you can check the status of each job, its stdout and stderr.

![image](images/JobStatus.png)

In case of failure or helding of the job, you can get an hint of the reason from its corresponding stdout and stderr.

## Kick campaign stage to complete

When a reasonable fraction (90% - 95%) of the job is successuful finished, you can kick the campaign stage to complete. To do so, from the Campaigns page, click on the **Submission History** button of you campaign.

![image](images/SubmissionHistory.JPG)

Then click on the **Submission ID** of the submission you are interrested.

![image](images/SubmissionID.JPG)

Finally, click on the **Kick to Complete** button.

![image](images/KickToComplete.JPG)

## Define Samweb dataset

Once the job is completed, the output files are in the folder defined in the script that you find in the **launch_script** of the **job_type** configuration in the bottom area of the Campaign Editor (for example: /icarus/app/poms_test/cfg/icarus_test_allupdates_withcaf_allstages.cfg). In the file, the stages are configured and among other there is the field **job_output.dest** or similar.

In order to define a samweb dataset for the stage you are interested, first setup **sam_web_client**:
```
source /cvmfs/icarus.opensciencegrid.org/products/icarus/setup_icarus.sh
ups list                        # just to list products and versions
setup sam_web_client v3_0       # for samweb client
setup fife_utils                # for fife (Fabric for Frontier Experiments) tools
export X509_USER_PROXY=/opt/icaruspro/icaruspro.Production.proxy
```

Then:

- Take one file and use samweb ```samweb -e sbn get-metadata``` command:

```
[07:29:49 ~]$ samweb -e sbn get-metadata gen-prodcorsika_genie_nooverburden_icarus_Jun2021_20210623T130814_325-0048_gen_20210623T180243_g4_20210625T093723_detsim.root
              File Name: gen-prodcorsika_genie_nooverburden_icarus_Jun2021_20210623T130814_325-0048_gen_20210623T180243_g4_20210625T093723_detsim.root
                File Id: 17000891
            Create Date: 2021-06-25T09:38:48+00:00
                   User: icaruspro
              File Size: 10685425324
               Checksum: enstore:273821235
                         adler32:0ab22e34
                         md5:435d50e1b9943c9baea74c2d3f014cd5
         Content Status: good
              File Type: mc
            File Format: artroot
                  Group: icarus
              Data Tier: simulated
            Application: art detsim v09_17_00
             Process Id: 3748723
            Event Count: 25
            First Event: 1
             Last Event: 25
             Start Time: 2021-06-25T09:10:05+00:00
               End Time: 2021-06-25T09:37:22+00:00
            Data Stream: out1
    art.file_format_era: ART_2011a
art.file_format_version: 13
        art.first_event: 1
         art.last_event: 25
       art.process_name: DetSim
           art.run_type: physics
               fcl.name: multitpc_detsim_icarus.fcl
    icarus_project.name: icarus_ICARUS_event_selection_BNB_NuPlus_intime_cosmics_v09_17_00
icarus_project.software: icaruscode
   icarus_project.stage: detsim
 icarus_project.version: v09_17_00
            merge.merge: 0
           merge.merged: 0
        production.name: 2021A_00
        production.type: SBN
                   Runs: 325.0048 (physics)
                Parents: gen-prodcorsika_genie_nooverburden_icarus_Jun2021_20210623T130814_325-0048_gen_20210623T180243_g4.root
```

- From the output, check **icarus_project.name** and **icarus_project.stage** fields

- Run the ```samweb -e sbn create-definition``` command:

```
[07:41:25 ~]$ samweb -e sbn create-definition ICARUS_event_selection_BNB_NuPlus_intime_cosmics_v09_17_00_detsim "icarus_project.name icarus_ICARUS_event_selection_BNB_NuPlus_intime_cosmics_v09_17_00 and icarus_project.stage detsim"
```

- Check the number of files is what you expected:

```
[07:51:14 ~]$ samweb -e sbn list-files --summary "defname:ICARUS_event_selection_intime_cosmics_v09_17_00_detsim"
File count:     2803
Total size:     9866703431744
Event count:    38448
```

- If the number of files is more than you expected, probably there are duplicates. To remove duplicates, run the command:

```
sam_dataset_duplicate_kids --retire_file --delete --dims "defname:ICARUS_event_selection_intime_cosmics_v09_17_00_g4"
```

and/or

```
sam_dataset_duplicate_kids --retire_file --delete --dims "ischildof:(defname:ICARUS_event_selection_intime_cosmics_v09_17_00_g4)"
```


## Most common issues

### fcl not exists

If the job fails and in the stderr you find something like:

```
error: globus_ftp_client: the server responded with an error
550 File not found

program: globus-url-copy -rst-retries 1 -gridftp2 -nodcau -restart -stall-timeout 14400  gsiftp://fndca4b.fnal.gov/pnfs/fnal.gov/usr/icarus/resilient/icaruspro/poms_fcl/g4_enable_spacecharge.fcl file:////srv/no_xfer/0/TRANSFERRED_INPUT_FILES//g4_enable_spacecharge.fclexited status 1
```

probably the fcl file (*g4_enable_spacecharge.fcl* in this example) is not present in */pnfs/icarus/resilient/icaruspro/poms_fcl*

### Held job

If you job is in held state, you can check the reason:

- From the campaign stage page, click on **Campaign Stage Stats (Landscape)** link

![image](images/Landscape.png)

- In the opened page, you can find an histogram with the **Hold Reason**

![image](images/hold_reason.png)

### Exit code

To check the non-zero exit code of the jobs:

- From the campaign stage page, click on **Campaign Stage Stats (Landscape)** link

![image](images/Landscape.png)

- In the opened page, you can find an histogram with the **Nonzero Exit Code**

![image](images/exit_code.png)

- The meaning of the exit code can be found in [here](https://cdcvs.fnal.gov/redmine/projects/icarus-production/wiki/ICARUS_Exit_Codes)

