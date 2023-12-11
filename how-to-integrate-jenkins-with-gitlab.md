# How to integrate Jenkins with GitLab


The Jenkins integration requires configuration in both GitLab and Jenkins.


1. [Grant Jenkins access to the GitLab project](#grant-jenkins-access-to-the-gitlab-project)

2. [Configure the Jenkins server](#configure-the-jenkins-server)

3. [Configure the Jenkins project](#configure-the-jenkins-project)

4. [Configure the GitLab project](#configure-the-gitlab-project)

5. [References](#references)



## Grant Jenkins access to the GitLab project

To grant Jenkins access to the GitLab project:

**1-1**. Create a personal, project, or group access token.


* [Create a personal access token](https://microfluidics.utoronto.ca/gitlab/help/user/profile/personal_access_tokens.md#create-a-personal-access-token) to use the token for all Jenkins integrations of that user.

* [Create a project access token](https://microfluidics.utoronto.ca/gitlab/help/user/project/settings/project_access_tokens.md#create-a-project-access-token) to use the token at the project level only. For instance, you can revoke
the token in a project without affecting Jenkins integrations in other projects.

* [Create a group access token](https://microfluidics.utoronto.ca/gitlab/help/user/group/settings/group_access_tokens.md#create-a-group-access-token-using-ui) to use the token for all Jenkins integrations in all projects of that group.


### Personal access tokens

You can create as many personal access tokens as you like.

1. On the left sidebar, select your avatar.


![jenkins-gitlab-intg-01](./jenkins.gitlab.integration.images/jenkins-gitlab-intg-01-user-jenkins.jpg)


![jenkins-gitlab-intg-02](./jenkins.gitlab.integration.images/jenkins-gitlab-intg-02-edit-profile.jpg)


![jenkins-gitlab-intg-03](./jenkins.gitlab.integration.images/jenkins-gitlab-intg-03-personal-access-token.jpg)


![jenkins-gitlab-intg-04](./jenkins.gitlab.integration.images/jenkins-gitlab-intg-04-jenkins-access-token.jpg)


![jenkins-gitlab-intg-05](./jenkins.gitlab.integration.images/jenkins-gitlab-intg-05-create-personal-access-tokens.jpg)


![jenkins-gitlab-intg-06](./jenkins.gitlab.integration.images/jenkins-gitlab-intg-06-new-personal-access-token.jpg)


![jenkins-gitlab-intg-07](./jenkins.gitlab.integration.images/jenkins-gitlab-intg-07-active-personal-access-token.jpg)



**WARNING**: The ability to create personal access tokens without expiry was deprecated in GitLab 15.4 and removed in GitLab 16.0.
In GitLab 16.0 and later, existing personal access tokens without an expiry date are automatically given an expiry date
of 365 days later than the current date. The automatic adding of an expiry date occurs on GitLab.com during the 16.0 milestone.
The automatic adding of an expiry date occurs on self-managed instances when they are upgraded to GitLab 16.0. This change is 
a breaking change.


**NOTE**: You can get the version of GitLab remotely without SSH access to server. You should be logged in to access
the following page:

`https://your.domain.name/help`


![jenkins-gitlab-intg-08](./jenkins.gitlab.integration.images/jenkins-gitlab-intg-08-gitlab-version.jpg)


## Configure the Jenkins server

Install and configure the Jenkins plugin. The plugin must be installed and configured to authorize the connection to GitLab.


**2-1**. On the Jenkins server, select **Manage Jenkins > System Configuration > Plugins** to check the installation status
of plugins.


![jenkins-gitlab-intg-09](./jenkins.gitlab.integration.images/jenkins-gitlab-intg-09-system-plugins.jpg)


**2-2**. Select "Installed plugins" tab and search for gitlab plugins.


![jenkins-gitlab-intg-10](./jenkins.gitlab.integration.images/jenkins-gitlab-intg-10-installed-plugins-gitlab.jpg)


**2-3**. If the gitlab plugins are not installed, you can install them in "Available plugins" tab.


![jenkins-gitlab-intg-11](./jenkins.gitlab.integration.images/jenkins-gitlab-intg-11-available-plugins.jpg)


![jenkins-gitlab-intg-12](./jenkins.gitlab.integration.images/jenkins-gitlab-intg-12-available-plugins-gitlab.jpg)


![jenkins-gitlab-intg-13](./jenkins.gitlab.integration.images/jenkins-gitlab-intg-13-download-progress-pending.jpg)


![jenkins-gitlab-intg-14](./jenkins.gitlab.integration.images/jenkins-gitlab-intg-14-download-progress-success.jpg)


![jenkins-gitlab-intg-15](./jenkins.gitlab.integration.images/jenkins-gitlab-intg-15-download-progress-success-full.jpg)


![jenkins-gitlab-intg-16](./jenkins.gitlab.integration.images/jenkins-gitlab-intg-16-installed-plugins-gitlab.jpg)


![jenkins-gitlab-intg-17](./jenkins.gitlab.integration.images/jenkins-gitlab-intg-17-installed-plugins-gitlab-api-plugin.jpg)


The **Credentials** plugin is installed.


![jenkins-gitlab-intg-18](./jenkins.gitlab.integration.images/jenkins-gitlab-intg-18-installed-plugins-credentials.jpg)



**2-4**. Select the **Manage Jenkins > Security > Credentials** to adding the new credential using the GitLab API Token. 


![jenkins-gitlab-intg-19](./jenkins.gitlab.integration.images/jenkins-gitlab-intg-19-system-credentials.jpg)



**2-5**. Click the dropdown button of **global** and select "Add credentials".


![jenkins-gitlab-intg-20](./jenkins.gitlab.integration.images/jenkins-gitlab-intg-20-add-credentials.jpg)


**2-6**. Select "GitLab API token" for the **Kind**. 


![jenkins-gitlab-intg-21](./jenkins.gitlab.integration.images/jenkins-gitlab-intg-21-new-credentials.jpg)


In **API Token** field, [paste the value you copied from GitLab](#grant-jenkins-access-to-the-gitlab-project). Leave ID empty ID is automatically generated later. Input the description. Then press "Create".


![jenkins-gitlab-intg-22](./jenkins.gitlab.integration.images/jenkins-gitlab-intg-22-new-credentials-api-token.jpg)


**2-7**. Go back to the **Manage jenkins > Credentials**. The credential created is here:


![jenkins-gitlab-intg-23](./jenkins.gitlab.integration.images/jenkins-gitlab-intg-23-global-credentials-list.jpg)



**2-8**. Select the **Manage Jenkins > System Configuration > System** to define the GitLab connection using this credential.


![jenkins-gitlab-intg-24](./jenkins.gitlab.integration.images/jenkins-gitlab-intg-24-system.jpg)



![jenkins-gitlab-intg-25](./jenkins.gitlab.integration.images/jenkins-gitlab-intg-25-system-home-dir-executor.jpg)



![jenkins-gitlab-intg-26](./jenkins.gitlab.integration.images/jenkins-gitlab-intg-26-system-jenkins-url.jpg)



Input the connection name and the URL of your GitLab. Select the credential created earlier and then click
"Test Connection". As you can see, the test is Success.


![jenkins-gitlab-intg-27](./jenkins.gitlab.integration.images/jenkins-gitlab-intg-27-system-gitlab-connection.jpg)


![jenkins-gitlab-intg-28](./jenkins.gitlab.integration.images/jenkins-gitlab-intg-28-system-test-connections.png)


**2-9**. Test the connection between jenkins server and gitlab.


![jenkins-gitlab-intg-29](./jenkins.gitlab.integration.images/jenkins-gitlab-intg-29-system-test-connection-result.jpg)



## Configure the Jenkins project


**3**. Set up the Jenkins project you intend to run your build on.


**3-1**. On your Jenkins dashboard, select **New Item**.

![jenkins-gitlab-intg-30](./jenkins.gitlab.integration.images/jenkins-gitlab-intg-30-new-item-create-job.jpg)


**3-2**. Enter the project’s name.

**3-3**. Select **Freestyle** or **Pipeline** and select **OK**.
You should select a freestyle project, because the Jenkins plugin updates the build status on GitLab.
In a pipeline project, you must configure a script to update the status on GitLab.


![jenkins-gitlab-intg-31](./jenkins.gitlab.integration.images/jenkins-gitlab-intg-31-new-job.jpg)


**3-4**. Under the **General** tab, add a project description in the **Description** field. Choose your GitLab connection
from the dropdown list.

![jenkins-gitlab-intg-32](./jenkins.gitlab.integration.images/jenkins-gitlab-intg-32-general.jpg)


**3-5**. In the Source Management section, select the None.

**3-6** select **Build when a change is pushed to GitLab** in the Build Triggers. 

![jenkins-gitlab-intg-33](./jenkins.gitlab.integration.images/jenkins-gitlab-intg-33-freestyle-source-code.jpg)


Click the "?" button. Webhook URL listed here is used later, during the setting of Webhook in GitLab.


![jenkins-gitlab-intg-34](./jenkins.gitlab.integration.images/jenkins-gitlab-intg-34-freestyle-build-trigger.jpg)


The following checkboxes is selected by default:

* Push Events
* Opened Merge Request Events


Select **Advanced** section, create the Secret Token. This token will use in the webhook setting on the GitLab.


![jenkins-gitlab-intg-35](./jenkins.gitlab.integration.images/jenkins-gitlab-intg-35-freestyle-secret-token.jpg)


![jenkins-gitlab-intg-36](./jenkins.gitlab.integration.images/jenkins-gitlab-intg-36-freestyle-secret-token.jpg)


**3-7**. Add a build step. Open the **Add build step** drop-down menu and select **Execute shell**.


![jenkins-gitlab-intg-37](./jenkins.gitlab.integration.images/jenkins-gitlab-intg-37-freestyle-build-steps.jpg)



**3-8**. Enter the commands you want to execute in the Command field. Input the command `echo "The repository is pushed to GitLab"`.
Click the **Save** button to save changes to the project.

![jenkins-gitlab-intg-38](./jenkins.gitlab.integration.images/jenkins-gitlab-intg-38-freestyle-execute-shell.jpg)


**3-9**. Check the status of project.


![jenkins-gitlab-intg-39](./jenkins.gitlab.integration.images/jenkins-gitlab-intg-39-freestyle-project-status.jpg)



## Configure the GitLab project

4. Configure the GitLab integration with Jenkins in one of the following ways.



### With a Jenkins server URL


**4-1-1**. On the left sidebar, select Search or go to and find your project.

**4-1-2**. Select Settings > Integrations.

**4-1-3**. Select Jenkins.

**4-1-4**. Select the Active checkbox.

Select the events you want GitLab to trigger a Jenkins build for:

* Push
* Merge request
* Tag push

Enter the Jenkins server URL.

Optional. Clear the Enable SSL verification checkbox to disable SSL verification.

Enter the Project name. The project name should be URL-friendly, where spaces are replaced with underscores.
To ensure the project name is valid, copy it from your browser’s address bar while viewing the Jenkins project.
If your Jenkins server requires authentication, enter the Username and Password.

Optional. Select Test settings.

Select Save changes.


### With a webhook


**4-2-1**. If you cannot provide GitLab with your Jenkins server URL and authentication information, you can configure 
a webhook to integrate GitLab and Jenkins.


![jenkins-gitlab-intg-40](./jenkins.gitlab.integration.images/jenkins-gitlab-intg-40-gitlab-repository.jpg)


![jenkins-gitlab-intg-41](./jenkins.gitlab.integration.images/jenkins-gitlab-intg-41-setting-webhook.jpg)


**4-2-2**. Create a webhook for your project. Enter the trigger URL (such as https://JENKINS_URL/project/YOUR_JOB).


![jenkins-gitlab-intg-42](./jenkins.gitlab.integration.images/jenkins-gitlab-intg-42-webhook-setting.jpg)


**4-2-3**. Optional. Clear the **Enable SSL verification** checkbox to disable SSL verification.

![jenkins-gitlab-intg-43](./jenkins.gitlab.integration.images/jenkins-gitlab-intg-43-webhook-setting.jpg)


You can see the lis of hooks in the `project Hooks` box.

![jenkins-gitlab-intg-44](./jenkins.gitlab.integration.images/jenkins-gitlab-intg-44-project-hooks.jpg)


**4-2-4**. To test the webhook, select **Push event** under the `Test` button.

![jenkins-gitlab-intg-45](./jenkins.gitlab.integration.images/jenkins-gitlab-intg-45-hook-tests.jpg)


![jenkins-gitlab-intg-46](./jenkins.gitlab.integration.images/jenkins-gitlab-intg-46-hook-success.jpg)



**4-2-5**. You can see the hook log via **View details**.


![jenkins-gitlab-intg-47](./jenkins.gitlab.integration.images/jenkins-gitlab-intg-47-hook-edit.jpg)


![jenkins-gitlab-intg-48](./jenkins.gitlab.integration.images/jenkins-gitlab-intg-48-hook-details.jpg)


![jenkins-gitlab-intg-49](./jenkins.gitlab.integration.images/jenkins-gitlab-intg-49-hook-request-details.jpg)


![jenkins-gitlab-intg-50](./jenkins.gitlab.integration.images/jenkins-gitlab-intg-50-hook-request-body.jpg)


![jenkins-gitlab-intg-51](./jenkins.gitlab.integration.images/jenkins-gitlab-intg-51-hook-response-body.jpg)


**4-2-6** Go back to jenkins **Dashboard**. As you can see the status of the gitlab_push_trigger is success.

![jenkins-gitlab-intg-52](./jenkins.gitlab.integration.images/jenkins-gitlab-intg-52-jenkins-project-status.jpg)


**4-2-7** Click the job link name `gitlab_push_trigger` and see the **Build History** section.


![jenkins-gitlab-intg-53](./jenkins.gitlab.integration.images/jenkins-gitlab-intg-53-jenkins-build-history.jpg)


Click the dropdown button of build number #1 and select **Console Output**.


![jenkins-gitlab-intg-54](./jenkins.gitlab.integration.images/jenkins-gitlab-intg-54-execute-1.jpg)


The **Console Output** contains the complete text log of output from the execution.


![jenkins-gitlab-intg-55](./jenkins.gitlab.integration.images/jenkins-gitlab-intg-55-console-output.jpg)



## References 

1. [Jenkins (docs.gitlab)](https://docs.gitlab.com/ee/integration/jenkins.html)

2. [Jenkins (microfluidics)](https://microfluidics.utoronto.ca/gitlab/help/integration/jenkins.md)

3. [GitLab Plugins](https://plugins.jenkins.io/gitlab-plugin/)

4. [Jenkins Build: Set Up Freestyle Project in Jenkins](https://phoenixnap.com/kb/jenkins-build-freestyle-project)

5. [How to trigger a Jenkins build from Gitlab using a Webhook trigger (Youtube)](https://www.youtube.com/watch?v=r5zhTu694Kc)

