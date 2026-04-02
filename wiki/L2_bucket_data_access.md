## intro

## create a Pawsey account

<https://support.pawsey.org.au/portal/servicedesk/customer/portal/5/user/login?destination=portal%2F5>

Then send Hadrien your username.

## Client to use: rclone

Download and install rclone: <https://rclone.org/downloads/>

## set up permissions

Once you have been made a member of the PawseyXXXX project, go to:
<https://portal.pawsey.org.au/origin/portal/account/acaciakeys> and
create a new key for the PawseyXXXX project, then copy the configuration
for the rclone client.

Then you can let your client know about this key. In the terminal, run:

` rclone config`

== optional: create a mount point on your filesystem:

` rclone rcd --rc-web-gui`