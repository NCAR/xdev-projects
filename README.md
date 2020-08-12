**THE CONTENTS OF THIS REPO HAVE BEEN MOVED INTO THE NCAR/XDEV REPO!!!  Please use
that repository in the future.**

# xdev-projects
All past, current, and future projects and planning documents for the Xdev Team.

This repository is meant to contain team our project plans and organization efforts.
First, let's define some terminology:

- ***Projects*** are concerted efforts with a few *measurable* goals that span only
  a few months of work (i.e., a quarter of a year).  *Projects* are meant to be small
  enough in scope and commitment to see the "light at the end of the tunnel."  That
  is, it should be clear when defining a project what the outcome should be before
  starting any work.

- ***Campaigns*** are long-term efforts that can span multiple years, potentially.
  *Campaigns* are large enough in scope and commitment that it is hard to see what
  the outcome will be *specifically*, and yet some (possibly vague) goals can be
  articulated with the expectation that it might take a long time (and a lot of work)
  to achieve those goals.  *Campaigns* should be seen as spanning multiple *projects*.
  *Campaigns* are harder to define specifically than *projects*, and as such should
  be seen more as a way of defining a direction for work to take.

Our work can be categorized into one of two categories:

- ***Operational*** work pertains to day-to-day activities that *do not have a
  defined deadline*.  That is, *operations* can include responding to questions
  from scientists or users of our tools and utilities.  *Operations* can include
  work to stay up-to-date on technology, such as reading blogs, following issue
  trackers, etc..  In simple terms, *operations* are the stuff we do every day
  just because its required to do a good job on our projects, but may not directly
  impact or even be associated with an existing project or campaign.

- ***Project*** work is work on a specific project, tied to a measurable goal (or
  goals) of that project.  Most of our work should be project work.  Projects
  should be scoped with the intention that they can be *fully completed* in a
  matter of 2-3 months (i.e., quarterly), but shorter projects are acceptable.

This repository is meant to help organize and plan our *project* work.  The intent
is for each project to be described by a single document, a *Project Specification*
document.  A template for this document exists in the `projects` directory.
Similarly, *Campaign Specification* documents should be created, too, though the
requirements for defining a campaign as much less stringent.

## Contents

- **`projects/`**: This directory contains all project-related specifications and
  templates.

  - **`projects/current/`**: All project specification documents for on-going
    projects should be placed here.
  - **`projects/complete/`**: All project specification documents for completed
    projects should be placed here.
  - **`projects/construction/`**: All project specification documents for projects
    that are still being constructed/specified should be placed here.  This includes
    projects that are waiting to start (and, therefore, don't have milestone dates
    defined, yet).
  - **`projects/construction/projspec_template.md`**: A template for *Project Specification*
    documents.  When a new project is defined/specified, it should be named
    `projspec_{codename}` where `{codename}` is replaced by the project codename.

- **`campaigns/`**: This directory contains all campaign-related specifications and
  templates.

  - **`campaigns/current/`**: All campaign specification documents for on-going
    campaigns should be placed here.
  - **`campaigns/former/`**: All campaign specification documents for *obsolete*
    campaigns should be placed here.
  - **`campaigns/construction/`**: All campaign specification documents for campaigns
    that are still being constructed/specified should be placed here.
  - **`campaigns/construction/campspec_template.md`**: A template for *Campaign Specification*
    documents.  When a new campaign is defined/specified, it should be named
    `campspec_{codename}` where `{codename}` is replaced by the campaign codename.
