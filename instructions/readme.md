# Guidelines for Workflows Template Submissions

The following are guidelines for the various files needed to submit a Workflows Template to Okta. 

## Core Elements of a Workflows Template

These following are the component files and Trello card fields that you will need to provide in order to submit a Template to Okta. Please refer to this document when preparing your flowpack, setup guide, and video walkthrough as well as for tips on how to enter the Template title and description on the associated Trello card.

---

### The Flowpack

This is the actual .flowpack or .folder file that you export from Okta using the "Export" command on the folder in Workflows. Please rename this file to "workflow.flowpack" or "workflow.folder--depending on the original file extension--before attaching to the Trello card.

#### Goal: Build the use case in a way that’s not only functional, but understandable to someone seeing it for the first time. 

##### Guidance:

*   Major steps should be annotated using the “Notes” feature.
*   Make sure it aligns to the list of [supported use cases](https://help.okta.com/en/prod/Content/Topics/Workflows/workflows-main.htm?cshid=ext-Okta-workflows).

---

### Title

This is the Template title that will be displayed in the product.

#### Goal: This is what a customer will see when they’re browsing the catalog.

##### Guidance:

---

*   Make it clear, avoid big words
*   Active voice, start with a verb
*   Limit: 72 characters

---

### Description

This is the Template description that will be displayed in the product.

#### Goal: What it does, why it’s valuable, and how the template works.

##### Guidance:

*   3 to 5 sentences
*   Can be the same as your Overview section of your doc (below)

---

### Links

#### Goal: Attach links to the following files to the Trello card for your Template.

##### Guidance:

*   Please provide links to your files using the "Attachment" button on the Trello card. Links from Google Drive, Box, GitHub, etc. are acceptable as long as we have access to them.
    *   Link to your **Setup Guide** (either a Google Doc or a markdown document named read.md).
    *   Link to your **Flowpack**
    *   Link to your **Video Walk-Through**
    *   Any other links that may be helpful, such as code samples, diagrams, screen shots, etc.

---

### Setup Documentation

#### Goal: Step-by-step guide for how to get the template up and running.

##### Guidance:

*   Assume the user starts from the installed flowpack/folder - you don’t need to explain navigating to the workflows console, etc.
*   Includes Okta config (eg create a user with a factor), setting up connections, etc
*   Does NOT need to include every step on how to build the flow from scratch.

##### Format:

*   Please use this setup guide template as a starting point: [TODO Update this with the link to the doc template](www.google.com). The template is a markdown document, which is the final format that will be used by the product. Please use this template and attach a link your setup guide in markdown if possible.
*   If you are unfamiliar with markdown, you can simply copy the body of the template into a Google Doc and attach the link to that document in the Trello card. Or, if you are ambitious you could try converting your Google Doc to markdown by using this Google Doc extension: [https://gsuite.google.com/marketplace/app/docs_to_markdown/700168918607.](https://gsuite.google.com/marketplace/app/docs_to_markdown/700168918607.)

---

### Video Walk-Through

#### Goal: Basically a live version of the setup guide. Don’t go and build it - just set it up, test it, and talk through why you built the flow that way and what it can achieve for customers.

##### Guidance:

*   Length: 5 to 10 minutes
*   We recommend using [Loom](www.loom.com) as it shows your face for a more personal presentation.
*   Monitors are getting better - make sure you zoom in
*   Will eventually be hosted on Youtube

## Submission Process

If you would like to make a suggestion for a new Workflows Template but aren't interested in building and submitting it yourself, you can make a suggestion directly in the product from the Templates page. If you would like to contribute a template,  you can reach out to the [Workflows Team](mailto:workflows@okta.com). We'll evaluate your idea for use case fit and value to customers. We use a Trello board [Workflows Template Roadmap](https://trello.com/b/gRp3nNHo/workflows-template-roadmap) to track Template suggestions and in-flight Templates. If your Template idea is accepted, we'll add a card and assign you to it. That's where you'll end up attaching links to the artifacts mentioned above. There will be additional instructions and information about the process on the board.
