# Wayback Machine
`The Guide`

The [Internet Archive](https://en.wikipedia.org/wiki/Internet_Archive) is an American digital library that provides free public access to digitalized materials, including websites, collected automatically via its web crawlers.

We can access several versions of these websites using the [Wayback Machine](http://web.archive.org/) to find old versions that may have interesting comments in the source code or files that should not be there. This tool can be used to find older versions of a website at a point in time. Let's take a website running WordPress, for example. We may not find anything interesting while assessing it using manual methods and automated tools, so we search for it using Wayback Machine and find a version that utilizes a specific (now vulnerable) plugin. Heading back to the current version of the site, we find that the plugin was not removed properly and can still be accessed via the `wp-content` directory. We can then utilize it to gain remote code execution on the host and a nice bounty.

![wayback-machine-image](/media/wayback-machine-imageone.png)

We can check one of the first versions of facebook.com captured on December 1, 2005, which is interesting, perhaps gives us a sense of nostalgia but is also extremely useful for us as security researchers.

![wayback-machines-facebook](/media/wayback-machine-imagetwo.png)

We can also use the tool [waybackurls](https://github.com/tomnomnom/waybackurls) to inspect URLs saved by Wayback Machine and look for specific keywords. Provided we have Go set up correctly on our host, we can install the tool as follows.

```bash
go get github.com/tomnomnom/waybackurls
```

To get a list of crawled URLs from a domain with the date it was obtained, we can add the `-dates` switch to our command as follows.

```bash
waybackurls -dates https://facebook.com > waybackurls.txt
```
```bash
cat waybackurls.txt
```