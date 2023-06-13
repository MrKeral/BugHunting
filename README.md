# Bug Hunting Methodology

## Search target who has large number of Subdomains
> Search Target : https://chaos.projectdiscovery.io/#/

## Search target using Google Dorks
> [VDP Google Dorks](https://github.com/MrKeral/BugHunting/blob/main/VDP-Dorks/VDP%20Dork.md)

## Recon for subdomain & IP & JS Files
    
    • subfinder -d target.com -t 100 -v -o target1.txt 
    • Wayback: https://web.archive.org/cdx/search/cdx?url=TARGET_URL&matchType=domain&fl=original&collapse=urlkey&limit=100000 
    • amass enum -passive -df target1.txt | tee -a sb.txt
    • amass enum -brute -d target.com -o target2.txt	
    • ./crt.sh target.com > target3.txt
    • cat target1.txt ta2.txt | sort | uniq | tee -a finaldomain.txt
    • httpx -l domain.txt -td -title -ip -sc -mc 200,301 > status.txt
    • httpx -l domain.txt -mc 200 > 200.txt
    • httpx -l domain.txt -mc 301 > 301.txt
    • cat waybackurls.txt | grep js | tee -a jsfile.txt
    
## Recon For Ip Address 

    • amass enum -src -ip -brute -d target.com -o target2.txt
    • censys search 'services.http.response.html_title: "Target"' --index-type hosts | jq -c '.[] | {ip: .ip}' | cut -d '"' -f4 > censysIP.txt
    
## Directory Search

    > ./dirsearch.py -l target.txt -t -o save.txt --format=plain
    > ./dirsearch.py -l target.txt -w wordlist.txt -t -o save.txt --format=plain
    > ./dirsearch.py -l Ips.txt -t -o save.txt --format=plain
    

## Scan Of Ips

    > nmap -Pn -p0-65535 -iL IPs.txt -T4 -sC -sV -vv -A --min-hostgroup 22 > nmap.txt
    > nmap -Pn -p0-65535 -iL domain.txt -T4 -sC -sV -vv -A --min-hostgroup 22 > nmap.txt
    
## Nuclei

    **Easy Mode**
    > nuclei -u https://target.com
    > nuclei -l target.txt
    > nuclei -u target.com:5001                 # For non HTTP/HTTPs
    > nuclei -u https://target.com -as          # Automatic Selection of Templates
    > nuclei -u https://target.com -nt          # New Templates
    
    **Template Mode**
    > nuclei -u https://target.com -t /.local/nuclei-templates/             # By Folder
    > nuclei -u https://target.com -t /.local/nuclei-templates/xss.yaml     # By Filename
    
    **Severity Mode**
    >  nuclei -u https://target.com -s critical,high,medium,low,info
    
    **Rate Limit**
    > nuclei -u https://my.target.site/ -rl 3 -c 2      # restrict outgoing requests to 3 per second and only 2 concurrent
    
    **Resume Scan**
    > nuclei -u https://my.target.site/ -resume /.config/nuclei/resume-ci0lldf58l5ooceq4nj0.cfg
    
## Vulnerbilities
    
