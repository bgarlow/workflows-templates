# Overview

The purpose of this Workflow pack is to perform various tasks in Google
Workspace when a given target user is being off-boarded from Google
Workspace or from the organization as a whole. This flow pack is
triggered when a user is added to a specified Okta Group (by default
that name of this group is G-Suite De-provisioned Users). The effects of
this Workflow on the Google Workspace User are as follows:

-   **Mailbox Delegate:** Delegate the target user’s GMail mailbox to
    the target user’s manager (The manager is as defined in Okta
    Universal Directory in the Manager attribute).

-   **Mailbox Forward Emails:** Setup a forwarder on the target user’s
    Gmail mailbox so that all emails sent to that mailbox are
    automatically forwarded to the target user’s manager (The manager is
    as defined in Okta Universal Directory in the Manager attribute).

-   **Mailbox Auto-Reply:** Set an GMail auto reply on the target user’s
    GMail mailbox (by default this will inform people that send emails
    to this mail box that the target user is no longer with the company
    as well as who to contact, in this case the user’s manager)

-   **Clear Okta Session (Optional):** When this feature is turned on,
    all of the target user’s Okta sessions will automatically be
    cleared.

-   **Remove Google Workspace Licenses (Optional):** When this features
    is turned on, all of the Google Workspace licenses assigned to the
    target user will be removed.

-   **Remove All Users Google Workspace ASPs (Optional):** When this
    feature is turned on, all of the Application Specific Passwords
    defined by the target user in Google Workspace will be removed.

-   **Remove All Users Devices in Google Workspace (Optional):** When
    this feature is turned on, all of the devices registered to the
    target user in Google Workspace will be removed.

-   **Remove All Users Access Tokens in Google Workspace (Optional):**
    When this feature is turned on, all of the application Access Tokens
    associated with this user will be removed.

-   **Deactivate Google Workspace Directory User (Optional):** When this
    feature is turned on the target user will automatically be
    deactivated in the Google Workspace Directory.

-   **Create GDrive Transfer Request (Optional):** When this feature is
    turned on a transfer request in Google Workspace to have all of the
    target user’s GDrive files and folders transferred to that user’s
    manager (The manager is as defined in Okta Universal Directory in
    the Manager attribute) will be created.

-   **Create Calendar Transfer Request (Optional):** When this feature
    is set to true, a transfer request for the target users Google
    Workspace calendar to the Google Workspace Users manager will be
    created.

# Before you get Started / Prerequisites 

For the workflows in the Okta Workflow Pack to work you will need:

-   An Okta Workflow connector in to your Okta tenant that has Workflows
    enabled

-   A Google Workspace Admin connector in Okta Workflows

-   A GMail connector in Okta Workflows

-   A Google Calendar connector in Okta Workflows

-   A Google Drive connector in Okta Workflows

-   An existing user (that will represent the manager user) in Okta and
    Google Workspaces where the username in Okta and in Google account
    are the same (the user’s email address) The manager user’s email
    address needs to point to a valid email address and have the
    appropriate Google Workspace licenses so that the manager can get
    the email notifications as part of the flow.

-   An exiting target user (that will represent the target user) in Okta
    where the Okta Primary Email Address for that user is the same as
    the user’s Google Workspace username. The manager attribute in Okta
    Universal Directory will need to be the username of the Okta user
    that represents the manager and you created in the step above.

-   An existing Okta Group that will be used to trigger this Workflow
    when the Okta target user is added to this group.

-   \[Optionally\]: Federate your Google Workspace tenant with your Okta
    tenant that has Okta Workflows enabled.

-   If you do not choose to federate your Google Workspace tenant with
    Okta, you will need to configure a Google Workspace application in
    your Okta tenant that has Okta Workflows enabled configured as
    Secure Web Authentication.

-   Add the Google Workspace application to an Okta Group (NOTE: Not the
    Okta Group will be used to trigger this Workflow).

-   Google Workspace will want to automatically assign licenses to new
    users in the Google Workspace tenant. You need to turn this off for
    Workflows to be able to manage Google Workspace licenses. To do
    this:

    1.  Login to the [<u>Google Workspace Admin
        Console</u>](https://admin.google.com/u/1/ac/home)

    2.  Click on Billing

    3.  Once in Billing you will need to do two things:

        1.  Add an additional license. I chose the Android Management
            license as it is zero cost / free.

        2.  ONLY after you have added the additional license will you
            see the setting to auto assign licenses so that you can turn
            it off for everyone.

# Google Cloud Project and Service Account Setup

## Set up a Google Cloud Project and Service Account

To use Gmail transfer of ownership features, you need to set up a Google
Cloud project and service account. The API endpoints that are needed to
add a delegate or forward emails are only available to service accounts
that have been given domain-wide authority.

In order to perform the steps in this topic, you must have admin access
to a Google Workspace domain and Google Cloud Platform.

## Create a Google Cloud Project

**Note**: It is not necessary to create a new project to complete this
procedure. If you already have a Google Cloud project, then go to step
5.

To create a Google Cloud Project:

1.  Go to Google Cloud
    Platform: [https://console.cloud.google.com](https://console.cloud.google.com/).

2.  Click the Quickstart drop-down in the top navigation bar.

3.  The project dialog appears.

4.  From the drop-down, select an organization, then click NEW PROJECT.

5.  Add a project name to the Project name field, and click Create.

## Enable the Gmail API for Google Cloud Project

To enable the Gmail API:

1.  In the left navigation pane, select **APIs & Services &gt;
    Library**.

2.  In the search field, type Gmail. The search results will show Gmail
    API.

3.  Click Gmail API then **Enable** on the **Gmail API** page.

You have created a Google Cloud project and enabled the Gmail API for
it.

## Create a Service Account for the Google Cloud Project

To create a service account:

1.  In the left navigation pane, select **IAM & Admin &gt; Service
    Accounts**.

2.  At the top of the **Service accounts** page, click **CREATE SERVICE
    ACCOUNT**.

3.  Add a service account name and description (optional) in
    the **Service account details** section, then click **CREATE**.

4.  On the **Service account permissions (optional)** page,
    click **Continue**. This step will be completed later.

5.  On the **Grant users access to this service** page, click **DONE**.
    This step will also be completed later.

You have created a service account.

## Set Up Google Workspace Domain-Wide Delegation of Authority

To use the endpoints to add a delegate or forward a user’s emails, you
need a Google Cloud service account with domain-wide authority. By
enabling domain-wide authority for a service account, you allow the
service account to programmatically access a user’s data without any
manual authorization on their part.

To set up domain-wide delegation of authority:

1.  Find the service account that you created in the previous task.

2.  Click on the name of the service account or **Actions** menu for
    that account name and select **Edit**.

3.  Click the **SHOW DOMAIN-WIDE DELEGATION** drop-down.

4.  In the header bar, click **Edit**.

5.  Select the **Enable Google Workspace Domain-wide Delegation** box.

6.  Add a name in the **Product name for the consent screen** field, and
    click **Save**.

You have set up Google Workspace domain-wide delegation for your
project. The **Client ID** field is now populated with a value for the
service account that will be associated with the Google Workspace
account. Take note of that Client ID value.

## Register and Add Scopes to the Service Account in Google Workspace Domain

To register your service account and add scopes:

1.  Go to [<u>https://admin.google.com</u>](https://admin.google.com/)

2.  If necessary, authenticate with your login credentials.

3.  Select **Security**.

4.  Select **Advanced Settings**.

5.  Click **Manage API client access**.

6.  In the Authorized API clients list, add the Client ID that was
    generated in the previous task in the **Client Name** field.

7.  In the **One or More API Scopes** field, add the required scopes.
    For adding a delegate or forwarding emails, the required scopes are:

8.  [<u>https://www.googleapis.com/auth/gmail.settings.basic</u>](https://www.googleapis.com/auth/gmail.settings.basic)

9.  [<u>https://www.googleapis.com/auth/gmail.settings.sharing</u>](https://www.googleapis.com/auth/gmail.settings.sharing)

10. Click **Authorize**, then click **Save**.

You have registered your service account with the Google Workspace
domain and added the appropriate permissions (scopes) to the account.
The service account can now access users’ data in the Google Workspace
domain.

## Add users and assign roles in the Google Cloud project

Each user in your Google Cloud project needs permissions to use the Add
Delegate, Forward Emails, or Set Auto Reply action card in the Workflows
Gmail connector. Each user will need the role of Service Account User on
the project level. This role will allow a user to load dynamic dropdown
parameters in the action cards.

Each user will also need the role of Service Account Token Creator on
the service account level. This role assigns the user permissions to
generate short-lived credentials for the service account so that it can
access user data in the Google Workspace domain.

To add user and assign the Service Account User role:

1.  Go
    to [<u>https://console.cloud.google.com</u>](https://console.cloud.google.com/) 

2.  Find your Google Cloud project and service account.

3.  From the left navigation pane, click **IAM & Admin**.

4.  Select **IAM**. 1. Click **ADD**. 1. In the **New members** field,
    add the user’s email address.

5.  In the **Select a role** dropdown, select **Service Accounts &gt;
    Service Account User**.

6.  Click **Save**.

To add user and assign the Service Account Token Creator role:

1.  From the left navigation pane, click **IAM & Admin**.

2.  Click **Service Accounts**.

3.  Click the checkbox for your service account, then click **SHOW INFO
    PANEL**.

4.  In the Permissions section, click **ADD MEMBER**.

5.  In the **New members** field, add the user’s email address.

6.  In the **Select a role** dropdown, select **Service Accounts &gt;
    Service Account Token Creator**.

7.  Click **Save**.

You have added a user to your Google Project who can authenticate to the
Workflows Gmail connector and use the Add Delegate, Forward Emails, and
Set Auto Reply action cards.

# Okta Workflows - Flow Pack Setup Steps

1.  Go to the Okta Workflows Console

2.  Create a new folder called “Google Workspace Off-Boarding Flow”

3.  Import the [<u>googleGoogle
    WorkspaceOffBoardingFlow.flopack</u>](https://drive.google.com/file/d/1mWqxTJNYXa5gOyifvBFNyO7wa2EZtRlY/view?usp=sharing)
    into this newly created folder

4.  If you have not already done so, authorize the connections to Google
    Workspace Admin, GMail, Google Drive and Okta.

5.  Select the “Google Workspace Off-Boarding Flow” flow from the
    folder.

6.  Change the name of the “Okta Google Workspace De-provisioned Group”
    variable to match the desired existing Okta Group that will be used
    to trigger this flow when an Okta user (target user) is added to
    this group.

7.  You can toggle on / off one or more of the Off Boarding tasks to be
    performed in the “Call Flow - \[CHILD\] Google Workspace
    Off-Boarding Tasks” card. To toggle on a given feature, set the
    value of the feature variable to true. Likewise to turn off a given
    feature, set the value of the feature variable to false. The
    optional tasks you can toggle on / off are listed below and
    described in the Overview section above:

    1.  Create GDrive Transfer Request

    2.  Remove G-Suite Licenses

    3.  Clear Okta Target User Sessions

    4.  Remove All Users Google Workspace ASPs

    5.  Remove All Users Devices in Google Workspace

    6.  Remove All Users Access Tokens in Google Workspace

    7.  Deactivate Google Workspace Directory User

8.  Once you are done with the changes, click Save to save the update to
    this flow.

9.  Toggle this flow from Off to On

10. Open the “\[CHILD\] Google Workspace Off-Boarding Tasks” workflow

11. Make sure that the following Okta Workflow cards have valid
    connections assigned:

    1.  Read User \[Target\]: A valid Okta connection to the your Okta
        tenant

    2.  Read User \[Manager\]: A valid Okta connection to your Okta
        tenant

    3.  Add Target User’s Manager as Delegate (GMail):

        1.  A valid Gmail connector to your Google Workspace - GMail
            environment

        2.  A valid Google Cloud Platform Project (that you created in
            the Google Cloud Project and Service Account Setup section)

        3.  A valid Google Cloud Platform Service Account that you
            created in the Google Cloud Project and Service Account
            Setup section)

    4.  Forward Target User Emails to Target User's Manager (GMail):

        1.  A valid Gmail connector to your Google Workspace - GMail
            environment

        2.  A valid Google Cloud Platform Project (that you created in
            the Google Cloud Project and Service Account Setup section)

        3.  A valid Google Cloud Platform Service Account that you
            created in the Google Cloud Project and Service Account
            Setup section)

    5.  Set Target User Auto Reply (GMail):

        1.  A valid Gmail connector to your Google Workspace - GMail
            environment

        2.  A valid Google Cloud Platform Project (that you created in
            the Google Cloud Project and Service Account Setup section)

        3.  A valid Google Cloud Platform Service Account that you
            created in the Google Cloud Project and Service Account
            Setup section)

    6.  Create Transfer Request to Transfer Target Users files / folders
        to Manager (Google Drive)

        1.  A valid Google Drive connector to your Google Workspace
            environment

    7.  Read User Licenses (Google Workspace Admin)

        1.  A valid Google Workspace Admin connector to your Google
            Workspace environment.

    8.  Unassign Licenses from User (Google Workspace Admin)

        1.  A valid Google Workspace Admin connector to your Google
            Workspace environment.

    9.  Read All User ASPs (Google Workspace Admin)

        1.  A valid Google Workspace Admin connector to your Google
            Workspace environment.

    10. Read All User Devices (Google Workspace Admin)

        1.  A valid Google Workspace Admin connector to your Google
            Workspace environment.

    11. Read All User Access Tokens (Google Workspace Admin)

        1.  A valid Google Workspace Admin connector to your Google
            Workspace environment.

    12. Read User (Google Workspace Admin)

        1.  A valid Google Workspace Admin connector to your Google
            Workspace environment.

    13. Deactivate User (Google Workspace Admin)

        1.  A valid Google Workspace Admin connector to your Google
            Workspace environment.

    14. Send Manager Email (GMail)

        1.  A valid Gmail connector to your Google Workspace - GMail
            environment

12. Once you have confirmed all of the connection options above, if
    anything has changed click on the Save button to save the changes to
    this flow pack.

13. Toggle this flow from Off to On

14. Open the \[CHILD\] Delete target users Google Workspace ASP Flow

15. Ensure that the Delete User ASP (Google Workspace Admin) card has a
    valid Google Workspace Admin connector to your Google Workspace
    environment.

16. If changes were made click the Save button to save the changes to
    this flow pack.

17. Toggle this flow from Off to On

18. Open the \[CHILD\] Remove Google Workspace Users Access Token Flow

19. Ensure that the Delete User Access Token (Google Workspace Admin)
    card has a valid Google Workspace Admin connector to your Google
    Workspace environment.

20. If changes were made click the Save button to save the changes to
    this flow pack.

21. Toggle this flow from Off to On

22. Open the \[CHILD\] Remove Users Device in Google Workspace Flow

23. Ensure that the Delete User Device (Google Workspace Admin) card has
    a valid Google Workspace Admin connector to your Google Workspace
    environment.

24. If changes were made click the Save button to save the changes to
    this flow pack. Navigate back to the flows in the flow pack

25. Toggle this flow from Off to On

<u>Testing this Okta Workflow Flow</u>

To test the Google Workspace Off-Boarding Flow:

## Stage Google Workspace Target User’s GMail Settings

1.  Login to [<u>https://mail.google.com</u>](https://mail.google.com/)
    as the target user in Google Workspace.

2.  Click on the gear icon in the right top corner

3.  Click on the “See all settings” button.

4.  Click on the “Forwarding and POP/IMAP” tab

5.  Under the “Forwarding” section check to make sure that no existing
    entry exists for the “Forward a copy of incoming mail to” section.
    If there is an entry, remove by using the drop down.

6.  Now go to the “Accounts” tab.

7.  Under the “Grant access to your account:” section make sure there is
    no entry in that section. If there is use the “delete” link to
    remove it.

8.  Click on the “General” tab.

9.  Scroll down to the “Vacation responder” section.

10. Ensure that the option for “Vacation responder off” option is
    selected.

11. Scroll to the bottom of this page and select “Save Changes” if
    available.

## Record Google Workspace Target User’s Calendar Events Settings 

1.  While you are still logged into Google Workspace as the target user,
    goto
    [<u>https://calendar.google.com/</u>](https://calendar.google.com/)

2.  Make note of any calendar events that the target user currently has
    on their Google Calendar.

## Record Google Workspace Target User’s Drive Settings 

1.  While you are still logged into Google Workspace as the target user,
    goto
    [<u>https://drive.google.com/</u>](https://calendar.google.com/)

2.  Make note of any files or folders that the target user currently has
    on their Google Drive.

## Record Google Workspace Target User’s Google Workspace Settings 

1.  Login into your Google Admin Console
    ([<u>https://admin.google.com/</u>](https://admin.google.com/)) in
    your Google tenant as a Super Admin.

2.  Click on the Users icon

3.  Click on the link for your target user in Google Workspace from the
    users displayed.

4.  Make note of the status of “Active” in the top left section under
    the selected user’s display name and email address.

5.  Scroll down and make note of any devices listed under the “Managed
    Devices” section.

6.  Scroll down to the “Security” section and make not of the number of
    application-specific passwords created for this user under the
    “Application-specific password” section.

7.  Now scroll down and make note of the Google Workspace licenses
    assigned to the user under the “Licenses” section.

## Initiate the Google Workspace Off-Boarding Flow Workflow

1.  Initiate the Workflow by adding an existing Google Workspace user in
    the Okta Admin Console to the Okta group that you created as part of
    the prerequisites section as well as defined in the “Okta Google
    Workspace De-Provisioning Group” variable in the “Google Workspace
    Off-Boarding Flow”. 

2.  Goto the Okta Workflows Console and click on the “Google Workspace
    Off-Boarding Flow” workflow.

3.  Click on Flow History and validate that this workflow was executed
    and that there were no errors during the execution of this workflow.

## Validate the Google Workspace Off-Boarding Flow Workflow Execution

1.  Login to [<u>https://mail.google.com</u>](https://mail.google.com/)
    as the target user in Google Workspace.

    1.  NOTE: If you chose the option to deactivate (suspend) the Google
        target user in the Google Directory, you may have to activate
        (un-suspend) the user in Google first before you can login as
        that user to perform the validation below.

2.  Click on the gear icon in the right top corner

3.  Click on the “See all settings” button.

4.  Click on the “Forwarding and POP/IMAP” tab

5.  Under the “Forwarding” section validate that in the “Forward a copy
    of incoming mail to” section that the email address that emails for
    the target Google Workspace user is set to the email address of the
    target user’s manager as defined in the Okta Universal Directory in
    the Manager attribute.

6.  Now go to the “Accounts” tab.

7.  Under the “Grant access to your account:” section and validate that
    access to this account has been granted to the Google Workspace
    user’s manager user in Google Workspace as defined for the target
    user in the Okta Universal Directory in the Manager attribute.

8.  Click on the “General” tab.

9.  Scroll down to the “Vacation responder” section.

10. Ensure that the option for “Vacation responder on” option is
    selected.

11. In this section validate that the subject line reads “&lt;target
    user’s first name&gt; &lt;target user’s last name&gt; is no longer
    with the company.”

12. In this section validate that the message box reads “&lt;target
    user’s first name&gt; &lt;target user’s last name&gt; is no longer
    with the company. If you need assistance please contact &lt;manager
    user’s first name&gt; &lt;manager user’s last name&gt; at
    &lt;manager user’s email address&gt;.”

13. If you selected the option in the “Google Workspace Off-Boarding
    Flow“ workflow to transfer the target user’s calendar events to the
    target user’s manager:

    1.  Login as the target user’s manager in Google Workspace

    2.  Goto
        [<u>https://calendar.google.com/</u>](https://calendar.google.com/)

    3.  Make note of the target user’s calendar events in the manager
        user’s Google Calendar.

14. If you selected the option in the “Google Workspace Off-Boarding
    Flow“ workflow to transfer the target user’s files and folders in
    Google Drive to the target user’s manager:

    1.  Login as the target user’s manager in Google Workspace

    2.  Goto
        [<u>https://drive.google.com/</u>](https://calendar.google.com/)

    3.  Make note of the target user’s files and folders in the manager
        user’s Google Drive.

15. Now login into your Google Admin Console
    ([<u>https://admin.google.com/</u>](https://admin.google.com/)) in
    your Google tenant as a Super Admin.

16. Click on the Users icon

17. Click on the link for your target user in Google Workspace from the
    users displayed.

18. If you selected the option in the “Google Workspace Off-Boarding
    Flow” to deactivate the Google Workspace user notice that the status
    of this user is set to “Inactive” in the top left section under the
    selected user’s display name and email address.

19. If you selected the option in the “Google Workspace Off-Boarding
    Flow” to remove the target user’s devices, scroll down and validate
    that there are no devices listed under the “Managed Devices”
    section.

20. If you selected the option in the “Google Workspace Off-Boarding
    Flow” to remove the target user’s application specific passwords
    validate that there are value for application-specific passwords
    created in the “Application-specific password” section is set to
    “0”.

21. If you selected the option in the “Google Workspace Off-Boarding
    Flow” workflow to remove the target user’s Google Workspace
    licenses, scroll down and validate that Google Workspace licenses
    assigned to the user under the “Licenses” section have been removed.

<u>Limitations & Known Issues</u>

This Okta Workflow flow pack makes some assumptions about the target
user:

-   The target user does not have a forwarder email address for their
    GMail mailbox defined already in Google Workspace

-   The target user does not have an existing delegate for their GMail
    mailbox already defined in Google Workspace

-   The target user’s Google Workspace user account has a license that
    gives them access to Google Drive and GMail.

-   The target user’s manager Google Workspace user account has a
    license that gives them access to GMail.
