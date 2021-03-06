Etherpad
========

We can add Etherpad to Security Onion to allow us to take notes during
investigations and share those with our team.

Simply run the following commands from a fresh Security Onion install
(master server/or standalone):

``wget https://raw.githubusercontent.com/weslambert/securityonion-etherpad/master/install_etherpad``

``sudo chmod +x install_etherpad``

``sudo ./install_etherpad``

Follow the prompts, until completion.

You should then be able to access Etherpad at the destination defined in
the setup script.

Be sure to configure DNS or client hosts file(s) with the appropriate
information, and run

``sudo so-allow``

to allow port 443 for analysts.
