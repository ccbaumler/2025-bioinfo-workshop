---
layout: page
title: Creating a JupyterLite Server for the Classroom
permalink: assignments/create_jupyter_server
parent: Assignments
nav_order: 2
---

# Creating a JupyterLite Server for the classroom
{:.no_toc}

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}


## Set up

1. Navigate to [Titus' template](https://github.com/ctb/2025-jupyterlite-template)
    - https://github.com/ctb/2025-jupyterlite-template

2. In the upper right, click `Use this template` > `Create a new repository`

    ![image](https://hackmd.io/_uploads/BksbP4ergl.png)

    {: .note }
    > The repository name will be the path of the URL. I.e. if you name a repo `stats102` and your username is `spartacus`, the URL will be `spartacus.github.io/stats102`.
    >
    > Repositories may also be public or private. Private repositories may require an upgraded subscription. Look into signing up for [GitHub for Teachers](https://github.com/education) to bypass a subscription fee.

3. Add a `Repository name` and click `Create repository` to generate a ***public*** repository hosting a JupyterLite session for your courses/curriculum.

    ![image](https://hackmd.io/_uploads/S1WBKVeBlg.png)

4. This new repository will be fully set up and attempts to run a `GitHub Action`, or a set of commands that will help build the JupyterLite session.

    ![image](https://hackmd.io/_uploads/HycVqVxrll.png)
    
    ![image](https://hackmd.io/_uploads/HkSL54grxg.png)

    {: .note }
    This `GitHub Action` may fail, showing a red 'X' instead of the green check for a successful run.

5. Enable `GitHub Pages` to create the website from your repositories files for your JupyterLite session.

    a. Go to the `Settings` tab on the upper right
    
    ![image](https://hackmd.io/_uploads/Skemh4eHll.png)

    b. Under `General` > `Code and automation`, select `Pages`
    
    ![image](https://hackmd.io/_uploads/Sku92Nxrge.png)

    c. While looking at `GitHub Pages` settings, under `Branch` select the `main` branch for building the JupyterLite webpage and click `Save`.
    
    ![image](https://hackmd.io/_uploads/BkiChElHxl.png)

    d. After a few minutes the actions will complete and a new section will be available in the `GitHub Pages` settings showing the website link.
    
    ![image](https://hackmd.io/_uploads/BkUPXHlHgg.png)

    e. Clicking the `Pages` link should re-direct you to the `lab/index.html` path for the JupyterLite site. I.e. this path `ccbaumler.github.io/micro` becomes `ccbaumler.github.io/micro/lab/index.html` instead.

## Changing content

{: .note }
> Currently, materials may only be added to the `content/data` and `content/notebooks` directories of the JupyterLite repository.
>
> If you would like an additional directory in the JL server, add a directory and file to said directory. I.e. In `contents/` > `Add file` > `documents/test.md` will add the `test.md` markdown file to `contents/documents/` directory. This will be visable in the JL server.

1. Edit the `README.md` file to reflect the goal of the JupyterLite repository. This should be a broad overview introducing the material of the session, detailing the expected learning outcomes, and explaining the contents of the `data` and `notebooks` directories for the students.

    ![image](https://hackmd.io/_uploads/SJLT8LxHee.png)


2. Add to the `content/data` and `content/notebooks` directories

    a. Files may be written and uploaded in GitHub:
    
      - with the `Add file` button that looks like a '+' sign (:heavy_plus_sign:)

          ![image](https://hackmd.io/_uploads/HJeezveBxe.png)
          
      - from the local computer with the `Add file` > `Upload files` button on the home page of the repo
    
          ![image](https://hackmd.io/_uploads/B1y1OPlSxe.png)
          
      - Drag and drop files or click the `choose your files` link to open your file navigator tool
          
          ![image](https://hackmd.io/_uploads/ryCJFweHle.png)

    {: .note }
    > If files are added within subdirectory, they will automatically be placed in that directory. However, the file path may be edited as needed. Simply add or delete characters until the correct filepath is written in for the file.
    >
    > For files added through the upload method, those files will need to be edited (look for the pencil icon :pencil2: after clicking on the file). Directories are automatically added to the filepath when `/` is included.

    b. Add the file path, file name, and file contents information. Click `Commit changes...` when ready

    ![image](https://hackmd.io/_uploads/rk8xXvxHll.png)

    c. For simple changes, `Commit directly to the main branch` is fine. When it comes time to rework a lot of the contents though, create a new branch and start a pull request. 

    ![image](https://hackmd.io/_uploads/SJQcmvxBxg.png)

    d. A pull request depends on an existing branch that has been copied from another branch, usually the `main` branch. When opening a pull request add any information relevant to the content you are working on changing. 

    ![image](https://hackmd.io/_uploads/rJ43mwgBgl.png)

    e. When you are happy with all the changes, `Merge` the pull request into `main`, `Confirm` the merge, and `Delete` the branch the pull request generated.

3. Removing to the `content/data` and `content/notebooks` directories

   a. To delete entire directories, navigate to the directory and select the three dots`...` > `Delete directory`

   ![image](https://hackmd.io/_uploads/HJv4cverxg.png)

   b. To delete a file, navigate to the file and select the three dotes `...` > `Delete file`

    ![image](https://hackmd.io/_uploads/rJA2qDeSgl.png)

    {: .note }
    After selecting the three dots `...`, you may need to scroll the drop-down menu to get to the `Delete file` selection.

# Common errors

## After creating the repository, I only see a static site like...

If you see something like below, either:
    1. JupyterLite did not build and distribute properly, or
    2. the website path is not navigating to the correct location

   ![image](https://hackmd.io/_uploads/HydGLrxBle.png)

Does adding the `/lab/index.html` path load the JupyterLite session? If it does load JupyterLite or not, the following steps should fix it.

1. Reset the `GitHub Pages` site

    a. Select `Unpublish site`

    ![image](https://hackmd.io/_uploads/SkbxdSeBeg.png)

    b. Save the `Branch` as `None` (make sure to click `Save`)

    ![image](https://hackmd.io/_uploads/B16yqHxSel.png)

    c. Save the `Branch` as `main` once again (make sure to click `Save`)

    ![image](https://hackmd.io/_uploads/BkiChElHxl.png)

    d. Upload some data or notebooks to the respective directories. Allow the actions to complete and navigate to the site once again.
    
{: .warning }
If this workflow does not fix the issue, reach out to me at ccbaumler@ucdavis.edu

## Missing features

![image](https://hackmd.io/_uploads/BkqzPSgrlx.png)

![image](https://hackmd.io/_uploads/HkMVwreBeg.png)

![image](https://hackmd.io/_uploads/BJySPSeSxx.png)

