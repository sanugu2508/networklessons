Configuration for PEAP-MSCHAPv2 on NPS and Switches
1. Configure the RADIUS Server (NPS) for PEAP-MSCHAPv2

    Open the NPS Console:
        Open the Network Policy Server console (nps.msc).

    Add RADIUS Clients:
        Go to RADIUS Clients and Servers > RADIUS Clients.
        Add each Ruckus switch, Arista switch, and Palo Alto device as a RADIUS Client.
        Enter the IP address of each device and configure a shared secret for authentication.

    Create a Network Policy for PEAP-MSCHAPv2:
        Navigate to Policies > Network Policies.
        Create a new network policy or edit an existing one for PEAP-MSCHAPv2 authentication.
        Set the Conditions to specify user groups (e.g., Network Admins) or other attributes.
        Under Constraints > Authentication Methods, select "Microsoft: Protected EAP (PEAP)".
        Click Edit and choose "Secured password (EAP-MSCHAP v2)" as the inner authentication method.
        Ensure certificate validation settings are correctly configured for PEAP (this is where the server certificate will be used).

2. Configure Ruckus Switches for PEAP-MSCHAPv2

    Access the Ruckus Switch Management Interface:
        Log in to the Ruckus switch via CLI.

    Configure RADIUS Settings:
        Set up 802.1X authentication for the management interface:

        shell

        enable
        configure terminal
        aaa authentication login default radius
        radius-server host [NPS_IP] key [shared_secret]
        line vty 0 4
        login authentication default

        Ensure the switch is configured to use PEAP-MSCHAPv2 for authenticating network admins.

3. Configure Arista Switches for PEAP-MSCHAPv2

    Access the Arista Switch Management Interface:
        Log in to the Arista switch via CLI.

    Configure RADIUS Settings:
        Set up RADIUS authentication for login access:

        shell

        configure terminal
        aaa group server radius RADIUS-SERVER
        server [NPS_IP] key [shared_secret]

        aaa authentication login default group RADIUS-SERVER local
        aaa authorization exec default group RADIUS-SERVER local

        Configure 802.1X settings if needed for wired or management access.

4. Configure Palo Alto Firewalls for PEAP-MSCHAPv2

    Access the Palo Alto Management Interface:
        Log in to the Palo Alto device via the Web UI.

    Configure RADIUS Server Settings:
        Go to Device > Server Profiles > RADIUS.
        Add a new RADIUS server profile:
            Enter the Name for the profile.
            Add the RADIUS server IP address (the IP of your NPS server).
            Enter the shared secret configured on the RADIUS server.
            Set the Timeout and Retry Interval settings as needed.

    Configure Authentication Profile for PEAP-MSCHAPv2:
        Go to Device > Authentication Profile.
        Add a new Authentication Profile.
        Set the Type to RADIUS.
        Select the RADIUS server profile created earlier.
        Under Advanced > Allow List, select all or specific user groups allowed to authenticate.

    Assign Authentication Profiles to the Management Interface:
        Go to Device > Setup > Management.
        Select the Authentication Profile for PEAP-MSCHAPv2 to apply to the management interface.

    Commit the Configuration:
        Click Commit in the upper right corner of the Web UI to apply the changes to the Palo Alto firewall.
