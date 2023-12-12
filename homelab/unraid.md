# Unraid notes

### Fixing the fan ramp up problem on Supermicro motherboards via IPMI
- determine the IP address of the IPMI module (make sure it's connected to the LAN of your server) by installing an IPMI plugin in Unraid community plugin store
- set up a basic Ubuntu VM on the server
- follow these 2 links:
- https://pgfitzgerald.wordpress.com/2017/02/18/new-home-lab/
- https://www.informaticar.net/supermicro-motherboard-loud-fans/
- install ipmitool
- determine the names of the fans via the IPMI plugin in Unraid (could have spaces, in which case it would be "Fan 1" for example)
- run the commands from those 2 articles once for each fan you're trying to fix the ramp-up/down for
- reboot the server
