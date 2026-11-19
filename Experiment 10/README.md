# Experiment 10: Phishing Simulation and Email Security Awareness Using Gophish

## 1. Objective

To configure Gophish for a controlled phishing-awareness exercise, use Mailpit as a local SMTP testing server, create the required campaign components, and examine campaign activity without collecting credentials or sensitive information.

## 2. Lab Overview

The practical was carried out in Kali Linux. Gophish was used to create and manage the simulation, while Mailpit acted as the local mail server used to receive test messages.

The workflow consisted of configuring the SMTP sending profile, preparing an awareness email and training page, adding a test recipient, and launching a campaign. The recipient address was a laboratory-only address, and the landing page was designed for awareness training rather than credential collection.

### Components used

| Component | Purpose |
|---|---|
| Kali Linux | Operating environment for the practical |
| Gophish | Phishing simulation and campaign management |
| Mailpit | Local SMTP capture and email inspection |
| `student@lab.local` | Test recipient used in the exercise |

## 3. Procedure and Evidence

### 3.1 Install and start Mailpit

Mailpit was installed on Kali Linux using its installation script. The terminal output confirmed that Mailpit was installed. Its web interface was then opened locally at `http://localhost:8025`.

![Mailpit installation output](screenshots/01-mailpit-installation.png)

*Figure 1: Mailpit installation completed successfully on Kali Linux.*

![Mailpit inbox](screenshots/02-mailpit-inbox.png)

*Figure 2: Mailpit web interface opened locally before receiving the campaign message.*

### 3.2 Configure and verify the sending profile

A new sending profile named **Mailpit** was created in Gophish. The interface type was set to SMTP and the local Mailpit SMTP endpoint was configured as `127.0.0.1:1025`. The sender identity used for the test was `training@lab.local`.

![Sending profile configuration](screenshots/03-sending-profile-configuration.png)

*Figure 3: SMTP sending profile configured to use the local Mailpit service.*

The first test attempt returned an invalid FROM parameter error. The sender details were corrected and the test was sent again.

![Initial SMTP test error](screenshots/04-initial-smtp-test-error.png)

*Figure 4: Initial test-email attempt showing the invalid FROM parameter response.*

The corrected test email appeared in Mailpit, confirming that Gophish could submit a message to the local SMTP service.

![Successful sending profile test](screenshots/05-successful-sending-profile-test.png)

*Figure 5: Test message received in Mailpit after correcting the sender details.*

The sending profile was then saved successfully.

![Saved sending profile](screenshots/06-sending-profile-saved.png)

*Figure 6: Mailpit listed as a saved Gophish sending profile.*

### 3.3 Prepare the awareness email template

An email template named **IT Security Awareness** was created. Its subject was **Important: Security Awareness Test**. The HTML body contained a short awareness message and a link placeholder for the campaign landing page.

The message identified the exercise as a simulation and advised the recipient not to enter real passwords or sensitive information on unverified pages. The tracking image option was left disabled.

![Email template editor](screenshots/07-email-template-editor.png)

*Figure 7: HTML email template prepared in Gophish.*

![Saved email template](screenshots/08-email-template-saved.png)

*Figure 8: Email template saved and visible in the Gophish template list.*

### 3.4 Create the training landing page

A landing page named **Security Awareness Training** was created. It displayed a short notice explaining the purpose of the exercise and the importance of checking links before opening them.

The option to capture submitted data was left disabled. The page did not request credentials.

![Landing page editor](screenshots/09-landing-page-editor.png)

*Figure 9: Training landing page configured in Gophish with data capture disabled.*

![Saved landing page](screenshots/10-landing-page-saved.png)

*Figure 10: Security Awareness Training page saved successfully.*

### 3.5 Add a laboratory recipient

A group named **Lab Test Group** was created with one test recipient:

| First name | Last name | Email | Position |
|---|---|---|---|
| Student | Test | `student@lab.local` | Cybersecurity Student |

![Recipient group editor](screenshots/11-recipient-group-editor.png)

*Figure 11: Test recipient details entered in the Lab Test Group form.*

![Saved recipient group](screenshots/12-recipient-group-saved.png)

*Figure 12: Lab Test Group saved with one member.*

### 3.6 Configure and launch the campaign

A campaign named **Lab Phishing Simulation** was configured using the saved email template, training landing page, Mailpit sending profile, and Lab Test Group.

The campaign URL was set to `http://127.0.0.1`, matching the local Gophish phishing-server listener on port 80. The Gophish administration interface used a separate port, so the campaign URL was not set to the admin dashboard address.

![Campaign configuration](screenshots/13-campaign-configuration.png)

*Figure 13: Campaign settings reviewed before launch.*

The campaign was launched. Gophish displayed the campaign results page and listed the test recipient with the status **Email Sent**.

![Campaign results after launch](screenshots/14-campaign-results-after-launch.png)

*Figure 14: Gophish campaign results showing the email-sent event for the test recipient.*

### 3.7 Inspect the received message and verify the destination

The campaign email was inspected in Mailpit. The sender, recipient, subject, and HTML message were checked. The **Open Training Page** link was visible in the received email.

![Campaign email in Mailpit](screenshots/15-campaign-email-in-mailpit.png)

*Figure 15: Campaign email received and rendered in Mailpit.*

After following the link, add the screenshot of the opened local training page here.

![Training page opened from campaign link](screenshots/16-training-page-opened.png)

*Figure 16: Local Security Awareness Training page opened from the campaign email. Screenshot to be added.*

Finally, refresh the Gophish campaign results and add the final results screenshot below. Use the counts shown in that screenshot when describing recorded events.

![Refreshed campaign activity](screenshots/17-campaign-results-refreshed.png)

*Figure 17: Refreshed campaign activity and event counts. Screenshot to be added.*

## 4. Observations

- Mailpit provided a local inbox for checking messages sent by Gophish.
- The initial test-email attempt reported an invalid FROM parameter; correcting the sender configuration allowed the test email to be delivered.
- Gophish successfully saved the sending profile, email template, landing page, and recipient group.
- The campaign was launched for one laboratory recipient.
- The campaign email was received and its HTML content was displayed correctly in Mailpit.
- The landing page was designed for awareness training and did not request passwords or submitted data.
- Campaign event counts must be read from the refreshed Gophish results page. A message appearing in Mailpit confirms receipt by the local test server, not necessarily an open or click event.

## 5. Result

Gophish was configured with Mailpit as a local SMTP testing service. An awareness email, a non-credential-collecting training page, and a one-member test group were created. The campaign was launched and the email was received in Mailpit. The practical demonstrated the basic workflow of a controlled phishing-awareness simulation and the use of campaign results for reviewing activity.

