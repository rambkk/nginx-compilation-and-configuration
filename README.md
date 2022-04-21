# nginx-compilation-and-configuration
Brief nginx compilation and configuration guide with fcgiwrap, mail imap/pop/smtp proxy module, etc.

Need to compile nginx because the default installation is not good enough? This guild could be helpful.

OS:
CentOS
Ubuntu

Configuration:
fcgiwrapper configuration (optional)
mail (for imap, pop, and smtp proxy)

Auth methods (use any):
- Built in nginx authentication
- Perl CGI simple authentication (requires fcgiwrapper)
- Bash script with netcat simple example


Bonus:
- stunnel - making SSL connection to actual mail servers
- Dummy SSL installation
- Let's encrypt free SSL installation


Not (yet) included:
php installation
