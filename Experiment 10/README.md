# Experiment 10: Phishing Simulation and Email Security Awareness Using Gophish

## 1. Objective

To configure Gophish for a controlled phishing-awareness exercise, use Mailpit as a local SMTP testing server, create the required campaign components, and examine the campaign activity without collecting credentials or sensitive information.

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

Mailpit was installed on Kali Linux using its installation script. The Mailpit web interface was opened locally at `http://localhost:8025` to inspect messages received by the test SMTP server.

![Mailpit inbox before the test](screenshots/01-mailpit-inbox.png)

*Figure 1: Mailpit inbox opened locally before receiving the campaign message.*

### 3.2 Configure the Gophish sending profile

A new sending profile named **Mailpit** was created in Gophish. The interface type was set to SMTP and the local Mailpit SMTP endpoint was configured as `127.0.0.1:1025`. The sender identity used for the test was `training@lab.local`.

![Mailpit sending profile configuration](screenshots/02-sending-profile-configuration.png)

*Figure 2: SMTP sending profile configured to use the local Mailpit service.*

A test email was sent from the profile. The first attempt returned an invalid FROM parameter error. After correcting the sender details, the test message was accepted and appeared in Mailpit.

![Successful Gophish test email in Mailpit](screenshots/03-sending-profile-test-email.png)

*Figure 3: Test email received in Mailpit, confirming the SMTP configuration.*

The sending profile was then saved successfully.

![Saved sending profile](screenshots/04-sending-profile-saved.png)

*Figure 4: Mailpit listed as a saved Gophish sending profile.*

### 3.3 Prepare the awareness email template

An email template named **IT Security Awareness** was created. Its subject was **Important: Security Awareness Test**. The HTML body contained a short awareness message and a link placeholder for the campaign landing page.

The message explained that the exercise was a simulation and advised the recipient not to enter real passwords or sensitive information on unverified pages. No tracking image was enabled.

![Email template editor](screenshots/05-email-template-editor.png)

*Figure 5: HTML email template prepared in Gophish.*

![Saved email template](screenshots/06-email-template-saved.png)

*Figure 6: Email template saved and visible in the Gophish template list.*

### 3.4 Create the training landing page

A landing page named **Security Awareness Training** was created. It displayed a short notice explaining the purpose of the exercise and the importance of checking links before opening them.

The options for capturing submitted data and passwords were left disabled. The page did not request credentials.

![Landing page editor](screenshots/07-landing-page-editor.png)

*Figure 7: Training landing page configured in Gophish with data capture disabled.*

![Saved landing page](screenshots/08-landing-page-saved.png)

*Figure 8: Security Awareness Training page saved successfully.*

### 3.5 Add a laboratory recipient

A group named **Lab Test Group** was created with one test recipient:

| First name | Last name | Email | Position |
|---|---|---|---|
| Student | Test | `student@lab.local` | Cybersecurity Student |

The group was used only for the local test campaign.

![Recipient group editor](screenshots/09-recipient-group-editor.png)

*Figure 9: Test recipient entered into the Lab Test Group.*

![Saved recipient group](screenshots/10-recipient-group-saved.png)

*Figure 10: Lab Test Group saved with one member.*

### 3.6 Configure and launch the campaign

A campaign named **Lab Phishing Simulation** was configured using the saved email template, training landing page, Mailpit sending profile, and Lab Test Group.

The campaign URL was set to `http://127.0.0.1`, matching the local Gophish phishing-server listener configured on port 80. The Gophish administration interface used a separate port, so the campaign URL was not set to the admin dashboard address.

![Campaign configuration](screenshots/11-campaign-configuration.png)

*Figure 11: Campaign settings checked before launch.*

The campaign was launched. Gophish displayed the campaign results page and listed the test recipient with the status **Email Sent**.

![Campaign results after launch](screenshots/12-campaign-results.png)

*Figure 12: Gophish campaign results showing the email-sent event for the test recipient.*

### 3.7 Verify the delivered message and training page

The campaign email was inspected in Mailpit. The sender, recipient, subject, and HTML message were checked. The **Open Training Page** link was used to access the local awareness page.

![Campaign email in Mailpit](screenshots/13-campaign-email-mailpit.png)

*Figure 13: Campaign email received and rendered in Mailpit.*

![Training page opened from campaign link](screenshots/14-training-page-opened.png)

*Figure 14: Local Security Awareness Training page opened from the campaign email.*

The campaign results were refreshed after the test. The final event counts should be read from the captured results screenshot; opening a message in Mailpit alone does not prove that Gophish recorded an email-open event.

![Refreshed campaign activity](screenshots/15-campaign-results-refreshed.png)

*Figure 15: Campaign activity after checking the training link. The displayed counts are the recorded results.*

## 4. Observations

- Mailpit provided a local inbox for checking messages sent by Gophish.
- The initial test-email attempt reported an invalid FROM parameter; correcting the sender configuration allowed the test email to be delivered.
- Gophish successfully saved the sending profile, email template, landing page, and recipient group.
- The campaign was launched for one laboratory recipient.
- The campaign email was received and its HTML content was displayed correctly in Mailpit.
- The landing page provided an awareness message and did not request or collect passwords.
- Campaign event counts were available in the Gophish results dashboard. An email being visible in Mailpit should not be treated as proof of an open or click event; those events must be confirmed in Gophish.

## 5. Result

Gophish was configured with Mailpit as a local SMTP testing service. An awareness email, a non-credential-collecting training page, and a one-member test group were created. The campaign was launched and the email was received in Mailpit. The exercise demonstrated the basic workflow of a controlled phishing-awareness simulation and the use of campaign results for reviewing activity.

