---
layout: page
title: Hosting a Customizable Course Website
permalink: assignments/hosting_course_website
parent: Assignments
nav_order: 3
---

# Hosting a Customizable Course Website
{:.no_toc}

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

# Intro

This is a step-by-step guide to produce a free, custom course website for all you courses.

The end product will be a custom template containing all your course information for any given class you teach. Since this is a template, you can create a new course website for every new quarter, semester, or year of a given course.

Because this is maintained in GitHub, every change you make is saved and any previous iteration is recoverable.

The template we are using builds from the [just-the-docs](https://just-the-docs.com/) which is designed to create a documentation website for software. 

## Initial set up

### 1. Copy the template [just-the-class](https://github.com/kevinlin1/just-the-class) into your GitHub account
- Click [just-the-class](https://github.com/kevinlin1/just-the-class)

- Click `Use this template`

    ![rect310](https://hackmd.io/_uploads/ryZFMz_vel.png)

- Click `Create a new repository`

    ![image](https://hackmd.io/_uploads/r1anfMuwgg.png)

- Add a `Repository name` and click `Create repository`

    ![rect310](https://hackmd.io/_uploads/S1ncBzdvxg.png)

- This repository is not a template. You will not be able to copy this repository within your account... Unless you convert it to a template first.
- Click `Settings` in the upper right of the page and navigate to the `General` > `Template repository` checkbox

    ![rect310](https://hackmd.io/_uploads/rJaqM4_vge.png)

### 2. Create a website using GitHub Pages

- Click on the `_config.yml` file

    ![image](https://hackmd.io/_uploads/SkmjYM_vex.png)

- Edit this file by clicking the pencil (:pencil2:) or the `Edit this file` icon in the upper right of the `_config.yml` file

    ![image](https://hackmd.io/_uploads/S13ajzOPxg.png)

- Change or add site variables within the `_config.yml` file. Use these variables in the site files with `{% raw %}{{}}{% endraw %}` like `{% raw %}{{ site.tagline }}{% endraw%}` which is `{{ site.tagline }}` for this site.

    {% raw %}
    ```liquid
    title: STAT101 
    tagline: 'STAT 101: A Statistical Adventure'
    description: Welcome to an adventure in statistics :bar_chart:
    author: Colton Baumler
    baseurl: '/course-template'
    url: 'https://ccbaumler.github.io'
    exclude: ["Gemfile", "Gemfile.lock", "LICENSE"]
    ```
    {% endraw %}

    {: .note }
    > The `title` variable is used in various places in the site. Best to keep it below 20 characters.
    >
    > We will use the `tagline` variable as a more descriptive title.
    >
    > The `description` variable describes the course with minimal detail
    >
    > The `author` variable is your name!
    >
    > `baseurl` is the name of the repository you created beginning with `/` 
    >
    > `url` is the root site link and will be structured with `https://<username>.github.io`
    >
    > `exclude` are the repository files that should be excluded from the published site. You can optionally exclude your `README.md` file, BUT you need to create an `index.md` file to build the website.
    >



- Now that the `url` and `baseurl` have been set to the correct format for your course site, Navigate to the `Settings` of the repo. Under `Code and automation` > `Pages` > `Branch`, select the `main` branch and `Save`.

    ![rect310](https://hackmd.io/_uploads/r13I74uDll.png)

## Customizing and expanding the existing material

### Changing the existing webpages of the site

1. For example, I do not want an `About` section in my course site, but I do want a `Syllabus`. To update or change existing site materials...

2. Find the `about.md` file that correspondes to the `About` page of the site. 
    > Generally, the base directory markdown files without an `_` will be site pages of the corresponding name as the file. This can be altered in the header section of a markdown file.

3. Notice that if I change the `title:` in the header that the link text presented on the site reflects the `title:`, but the link url remains `course-template/about/`. 

![image](https://hackmd.io/_uploads/r1tRQcXFee.png)
> `nav_order` guarantees the link will be at the top of the navigation bar on the left of the site.


4. To alter the url path, change either (or both) the filename or add the `permalink:` variable to the header.

![image](https://hackmd.io/_uploads/ByPJC5Qtex.png)

### Changing and adding standalone links (Upper right tabs on site)

The links on the upper right hand side of the site are available on every site page of the entire website. This is where you should place links that may be universally important to the students. Think your learning management site (LMS) for the course that the students amy need to submit homework (Canvas), the chat platform you would like the students to use (Slack), or your portfolio site.

1. Navigate to the `_config.yml` file.
2. Within the `_config.yml` file, jump to `theme settings` section and look at the current `aux_links:`:

    ![image](https://hackmd.io/_uploads/BkiMaoXtel.png)

3. Change this content to reflect the links you would like available across the site

    ![image](https://hackmd.io/_uploads/BkgsCsXFgx.png)
    > Depending on the length of the link text, limit the links to about 4-5 max.

### Adding a unique and dynamic landing page for the site

1. Keep `README.md` for the github repository, but use `index.md` file for the landing page of your website.

    - While this landing page can be a static site like [this course site](https://cmu-crafting-software.github.io/) through markdown formatting. We can add dynamic variables to the `index.md` by calling the site variables and css classes. Below, I set up the site to display the `latest` announcement on the landing page. Like so!

{% if site.announcements %}
{{ site.announcements.last }}
[Previous Announcements](announcements.md){: .btn .btn-outline .fs-3 }
{% endif %}


> Please skim the file for the landing page, below contains a file with detailed comments. 

{% raw %}
```liquid
---
layout: home
title: Stat Adv.
nav_exclude: true
seo:
  type: Course
  name: Stat 101 all the course materials
---

# {{ site.tagline }}
{: .mb-2 }
{{ site.description }}
{: .fs-6 .fw-300 }

## Welcome to Statistics 101! :bar_chart:

The link to this webpage is [{{site.url}}{{ site.baseurl }}]({{site.url}}{{ site.baseurl }}).

Use links at the top right of this page to access the school appropriate learning management system resources (most were mentioned in the [Syllabus]({{site.url}}{{ site.baseurl }}/about/#course-tools)).

This static site is provided as a convenience for quick access and searching of course materials.

{% if site.announcements %}
{{ site.announcements.last }}
[Previous Announcements](announcements.md){: .btn .btn-outline .fs-3 }
{% endif %}
```
{% endraw %}

> This is the same file, only with comments.

{% raw %}
```liquid
---  # Begin the YAML front matter block (required by Jekyll to process this file with custom settings)
layout: home  # Specifies that this page should use the "home" layout defined in the theme
title: Stat Adv.  # Sets the title of the page
nav_exclude: true  # Prevents this page from appearing in the site's navigation bar
seo:  # SEO (Search Engine Optimization) metadata section
  type: Course  # Specifies the type of content, useful for search engines
  name: Stat 101 all the course materials  # A name/description for the course for SEO

---  # End of YAML front matter block

# {{ site.tagline }}  # Renders the site's tagline as an H1 heading (Markdown syntax)
{: .mb-2 }  # Applies a custom CSS class for bottom margin (likely Bootstrap or similar)

{{ site.description }}  # Renders the site's description text (typically defined in `_config.yml`)
{: .fs-6 .fw-300 }  # Applies CSS classes for font size and weight (for styling)

## Welcome to Statistics 101! :bar_chart:  # Section heading with an emoji icon


The link to this webpage is [{{site.url}}{{ site.baseurl }}]({{site.url}}{{ site.baseurl }}).

Use links at the top right of this page to access the school appropriate learning management system resources (most were mentioned in the [Syllabus]({{site.url}}{{ site.baseurl }}/about/#course-tools)).  
# Paragraph directing users to LMS tools, linking to the syllabus using site variables

This static site is provided as a convenience for quick access and searching of course materials.  
# Paragraph explaining the purpose of the site

{% if site.announcements %}  # Liquid conditional: checks if there are any announcements defined in `_config.yml`
{{ site.announcements.last }}  # Displays the most recent announcement

[Previous Announcements](announcements.md){: .btn .btn-outline .fs-3 }  # A styled button linking to a page with older announcements
{% endif %}  # Ends the conditional block
```
{% endraw %}

### Adding new announcements

a. This section of the new index file will update the landing page with the lastest markdown file in the `_announcements` directory:

```
{% if site.announcements %}
{{ site.announcements.last }}
[Previous Announcements](announcements.md){: .btn .btn-outline .fs-3 }
{% endif %}
```
> This chunk of code looks for the `_announcements` directory and an announcement markdown file. It then takes the last announcement file and shows it on the webpage. Finally, it links the text `Previous Announcements` to the announcements webpage for the site.
    
b. Add a new file with a filename like `week-#.md` in the `_announcements` directory.
    
![image](https://hackmd.io/_uploads/HJHl6W-Yle.png)

c. The file must contain a header section like:

```
---
title: Week 2 Announcement
week: 2
date: 2025-08-18
---
```
    
c. Now, the announcement on the landing page for the site has been updated to look like:
    
![image](https://hackmd.io/_uploads/B19uZYXtex.png)

d. Keep adding markdown files with yaml headers to update the announcements of the course site

### Add new content for the site (link on the left hand navigation)

1. To add new content to this site, we first need to create a markdown document in the base directory of the repository to act as the landing page for the website path. I would like to add an assignments page, but you can add any number of site pages containing content for you course. E.g. Course Reader, Syllabus, Resources, FAQ, and so on.

    a. Create a new file...
    
    ![image](https://hackmd.io/_uploads/SyU_DF7tgx.png)
    
    b. Name it according to the content you wish to add...
    
    ![image](https://hackmd.io/_uploads/HJ7hvF7tgx.png)

    c. Add a yaml header defining the site page behavior.
    
    ```
    ---
    layout: page           #This will be a site page
    title: Assignments     #Its site page title will be `Assignments`
    permalink: assignments #Its site path will be `/assignments`
    has_children: true     #It will contain a dropdown menu
    nav_order: 3           #Its position in the navigation bar will be set to 3rd instead of Alphabectical 
    ---

    {:toc}                 # The only contents I want shown are the table of contents that will correspond to the directory containing the course materials
    ```

    > Do not copy the comments into the file. Use this instead!
    ```
    ---
    layout: page
    title: Assignments
    permalink: assignments
    has_children: true
    nav_order: 3
    ---

    {:toc}
    ```


    d. Add a directory and file by `Adding a new file` and typing out the directory name + `/` + the filename like, `assignments/hw01.md`.
    
    e. Add a header similar to below with any markdown content to be displayed on the site page.

    ```
    ---
    layout: page
    title: HW01 Version Control
    permalink: assignments/hw1
    parent: Assignments
    ---
    ```

    f. There should now be a new link on in the navigation pane, it should allow for dropdown navigation and the site should only show the downstream links.
    
    ![image](https://hackmd.io/_uploads/SyrhgoQYlx.png)

### Editing the current schedule

There is a standard, static schedule that is in place upon using the original template that looks like:

![image](https://hackmd.io/_uploads/HJC9bpQtlg.png)

1. Updating the static schedule file to fit you needs is as simple as navigating to the `_schedules/weekly.md`

```
---
timeline:
  - '9:00 AM'
  - '9:30 AM'
  - '10:00 AM'
  - '10:30 AM'
  - '11:00 AM'
  - '11:30 AM'
  - '12:00 PM'
  - '12:30 PM'
  - '1:00 PM'
  - '1:30 PM'
  - '2:00 PM'
  - '2:30 PM'
  - '3:00 PM'
  - '3:30 PM'
  - '4:00 PM'
  - '4:30 PM'
  - '5:00 PM'
  - '5:30 PM'
schedule:
  - name: Monday
    events:
      - name: Lecture
        start: 9:30 AM
        end: 10:30 AM
        location: 150 Wheeler
      - name: Section
        start: 11:30 AM
        end: 12:30 PM
        location: 310 Soda
      - name: Office Hours
        start: 12:30 PM
        end: 2:00 PM
        location: 271 Soda
  - name: Tuesday
  - name: Wednesday
    events:
      - name: Lecture
        start: 9:30 AM
        end: 10:30 AM
        location: 150 Wheeler
      - name: Section
        start: 11:30 AM
        end: 12:30 PM
        location: 310 Soda
      - name: Office Hours
        start: 12:30 PM
        end: 2:00 PM
        location: 271 Soda
  - name: Thursday
  - name: Friday
    events:
      - name: Lecture
        start: 9:30 AM
        end: 10:30 AM
        location: 150 Wheeler
      - name: Section
        start: 11:30 AM
        end: 12:30 PM
        location: 310 Soda
      - name: Office Hours
        start: 12:30 PM
        end: 2:00 PM
        location: 271 Soda
---
```

2. Configure this schedule to fit you needs by setting the `timeline:` to your school day timeframe.

```
---
timeline:
  - '8:30 AM'
  - '9:00 AM'
  - '9:30 AM'
  - '10:00 AM'
  - '10:30 AM'
  - '11:00 AM'
  - '11:30 AM'
  - '12:00 PM'
  - '12:30 PM'
  - '1:00 PM'
  - '1:30 PM'
  - '2:00 PM'
  - '2:30 PM'
```

4. In the `schedule:` section, the first indented `name:` is the day of the week. Each day of the week contains an `events:` subsection may contain any number of named events with starting and ending time as well as the event location. Simply change these to reflect the course schedule.

```
schedule:
  - name: Monday
    events:
      - name: Lecture
        start: 9:30 AM
        end: 10:30 AM
        location: 150 Wheeler
      - name: Section
        start: 11:30 AM
        end: 12:30 PM
        location: 310 Soda
      - name: Office Hours
        start: 12:30 PM
        end: 2:00 PM
        location: 271 Soda
  - name: Tuesday
  - name: Wednesday
    events:
      - name: Lecture
        start: 9:30 AM
        end: 10:30 AM
        location: 150 Wheeler
      - name: Break out discussion
        start: 11:30 AM
        end: 12:30 PM
        location: 310 Soda
      - name: Office Hours
        start: 12:30 PM
        end: 2:00 PM
        location: 271 Soda
  - name: Thursday
  - name: Friday
    events:
      - name: Lecture
        start: 9:30 AM
        end: 10:30 AM
        location: 150 Wheeler
      - name: Dancing
        start: 11:30 AM
        end: 12:30 PM
        location: 310 Soda
      - name: Office Hours
        start: 12:30 PM
        end: 2:00 PM
        location: 271 Soda
---
```

5. By adding the new event names like `Dancing` and `Break out discussion` into the `_scss/custom/schedule.scss` with a color selected at the end of the file (line 106):

```

    &.dancing {
      background-color: $red-000;
    }

    &.break-out-discussion {
      background-color: #fbc02d; // darker yellow
    }

```

6. The final static schedule can look something like this.

![image](https://hackmd.io/_uploads/HkYzWA7Yxe.png)

### Creating a simpler, dynamic schedule!

You may also prefer a google calendar that is updated every week with the schedule you build into a specific calendar at https://calendar.google.com.

1. In your google calendar, create a calendar for your course.
> Note that if you want specific colors displayed on your course site for your separate course event you will need to create unique calendars for the events of the course.
2. In the `Settings and sharing` for a calendar, scroll down to the `Integrate calendar` section and click `Customize`. You now have the ability to customize the `iframe src` link to your preferences.

![image](https://hackmd.io/_uploads/S1EeMyVFxx.png)

3. Below is an example to the schedule page that can get you started!

```=!
# Google schedule

- [Lecture, Discussion, and Special Events Calendar](#ldlc)

{: .note }

> If you are having trouble viewing the calendars below and are using Safari, we suggest switching to an alternate browser (like Chrome). Alternatively, you can go to Safari settings and switch "Prevent cross-site tracking" off, or you can see the _Lecture, Discussion, and Special Events Calendar_ [here](https://calendar.google.com/calendar/embed?height=600&wkst=1&bgcolor=%23ffffff&ctz=America%2FLos_Angeles&showTitle=0&mode=WEEK&src=c_ac325b8ebc6089c1e69cd6b950b9fce98bef018c534491733cf2ee2df764bad5@group.calendar.google.com).

<a name='ldlc'></a>

## Lecture, Discussion, and Special Events Calendar

This calendar contains times for

- Lectures (in **brown**)
- In-person discussion sections (in **yellow**)
- Remote discussion sections (in **blue**)

<iframe data-a11y-errors="true" title="Google Calendar of Course Events" src="https://calendar.google.com/calendar/embed?height=600&wkst=1&ctz=America%2FLos_Angeles&showPrint=0&mode=WEEK&src=Y19hYzMyNWI4ZWJjNjA4OWMxZTY5Y2Q2Yjk1MGI5ZmNlOThiZWYwMThjNTM0NDkxNzMzY2YyZWUyZGY3NjRiYWQ1QGdyb3VwLmNhbGVuZGFyLmdvb2dsZS5jb20&color=%233f51b5" style="border:solid 1px #777" width="800" height="600" frameborder="0" scrolling="no"></iframe>
```

The above `schedule.md` renders like this.

![image](https://hackmd.io/_uploads/r12Q-kNtgg.png)

