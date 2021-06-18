---
layout: page                                # mandatory, use page always
title: Example of Campaign Creation         # mandatory
description: Example of Campaign Creation   # optional
toc: true                                   # optional
---

# Creation, Submission and Monitor of a Campaign (an example)

In this Markdown page an example of the creation, test and launch of a new production campaign is reported. The main steps are: 
- [Clone an existing campaign](#clone-an-existing-campaign)
- [Add or remove stages and relative dependencies](#add-or-remove-stages-and-relative-dependencies)
- [Configure Campaign](#configure-campaign)
    - fcl file
    - number of jobs
    - number of event per file
    - location of the new production
    - draining(10)
- [Launch test campaign](#launch-test-campaign)
- [Launch campaign](#launch-campaign)
- [Monitor campaign](#monitor-campaign)

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

In the new window, you can check the software version and the **cs_split_type** field (1). A goos choise is **drainingn** with **nfiles = 10**. As the explanatory comment says, "*This type, when filled out as draining(n) for some integer n, will pull at most n files at a time from the dataset and deliver them on each iteration, keeping track of the delivered files with a snapshot. This means it works well for datasets that are growing or changing from under it*". 

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

Now you have launched your campaing in *test* mode. Once you have verified everything is set up correctly you can launch the campaign. Otherwise you have to fix the campaign and relaunch it in *test* mode.

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