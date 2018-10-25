# Functions with OCI Compute

Use Functions to send emails with [Oracle Cloud Infrastructure Email Delivery](https://docs.cloud.oracle.com/iaas/Content/Email/Concepts/overview.htm)

## Pre-requisites

### Configure OCI Email Delivery

- [Generate SMTP Credentials for a User](https://docs.cloud.oracle.com/iaas/Content/Email/Tasks/generatesmtpcredentials.htm) - you'll have to configure these user credentials in the app (`OCI_EMAIL_DELIVERY_USER_OCID` and `OCI_EMAIL_DELIVERY_USER_PASSWORD` variables)
- [Add approved sender](https://docs.cloud.oracle.com/iaas/Content/Email/Tasks/managingapprovedsenders.htm) - use `OCI_EMAIL_DELIVERY_APPROVED_SENDER` parameter to configure this in the app
- [Note down value for the SMTP server](https://docs.cloud.oracle.com/iaas/Content/Email/Tasks/configuresmtpconnection.htm) - it'll be used in the `OCI_EMAIL_DELIVERY_SMTP_SERVER` configuration attribute

Clone this repo

### Switch to correct context

- `fn use context <your context name>`
- Check using `fn ls apps`

### Create app

`fn create app --annotation oracle.com/oci/subnetIds=<SUBNETS> --config OCI_EMAIL_DELIVERY_USER_OCID=<OCI_EMAIL_DELIVERY_USER_OCID> --config OCI_EMAIL_DELIVERY_USER_PASSWORD=<OCI_EMAIL_DELIVERY_USER_PASSWORD> --config REGION=<REGION> --config OCI_EMAIL_DELIVERY_SMTP_SERVER=<OCI_EMAIL_DELIVERY_SMTP_SERVER> --config OCI_EMAIL_DELIVERY_APPROVED_SENDER=<OCI_EMAIL_DELIVERY_APPROVED_SENDER> fn-email-app`

e.g.

`fn create app --annotation oracle.com/oci/subnetIds='["ocid1.subnet.oc1.phx.aaaaaaaaghmsma7mpqhqdhbgnby25u2zo4wqlrrcskvu7jg56dryxt3hgvka"]' --config OCI_EMAIL_DELIVERY_USER_OCID=ocid1.user.oc1..aaaaaaaa4seqx6jeyma46ldy4cbuv35q4l26scz5p4rkz3rauuoioo26qwmq@ocid1.tenancy.oc1..aaaaaaaaydrjm77otncda2xn7qtv7l3hqnd3zxn2u6siwdhniibwfv4wwhta.3n.com --config OCI_EMAIL_DELIVERY_USER_PASSWORD='jsA$>>Fn-wH6T{kiN6)y' --config OCI_EMAIL_DELIVERY_SMTP_SERVER=smtp.us-phoenix-1.oraclecloud.com --config OCI_EMAIL_DELIVERY_APPROVED_SENDER=test@test.com fn-email-app`

**Check**

`fn inspect app fn-email-app`

## Moving on...

Deploy the app...

`cd fn-email-app` and `fn -v deploy --app fn-email-app`

Test it out

> change the email contents as per your requirements (*don't forget to provide a valid email address*)

`echo -n '{"To": "test@gmail.com", "Subject": "Hello from Fn", "Body": "Have fun with Fn at fnproject.io"}' | fn invoke fn-email-app sendemail`

If all goes well, you should see the email (*don't forget to check your spam folder*) and a response from the function - `Sent email successfully!`