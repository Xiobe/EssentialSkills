# What are RFCs?
RFCs or "Request For Comments" are texts that describe how protocols work. From a analyst point of view it is always handy to have an offline copy so that you can consult them at any time.

# How to obtain a mirror?
You have an [article] (https://www.rfc-editor.org/retrieve/rsync/) on the rfc-editor.org website that describes how to set up a mirror. It basically comes down to
`rsync -avz --delete ftp.rfc-editor.org::Module-Name Target-Directory`

I work with the pdf version so I can easily share the RFCs with customers so the crontab looks like:
`rsync -avz --delete ftp.rfc-editor.org::rfcs-pdf-only ~/RFC`

# Keeping the RFC up-to-date
To have a set-it-and-forget-it I use a cronjob (`crontab -e`) every 24 hours. For example every night at 23:30 you sync.

`30 23 * * * rsync -avz --delete ftp.rfc-editor.org::rfcs-pdf-only ~/RFC`

# Data Volume
The first time you syncronize it the whole RFC mirror needs to be populated with each existing RFC. This will take a while and thus sit back and relax. If you have an IT team and each team member wants to have a copy it might be better to sync to a central system copy over the mirror to the local laptops and then start the sync. This will lower the bandwidth consumption compared to each individual copying the complete mirror.
