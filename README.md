Bug Hunting

Steps for hunting.
> Search Target : https://chaos.projectdiscovery.io/#/

Recon for subdomain & IP & JS Files
    
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
    
Recon For Ip Address 

    • amass enum -src -ip -brute -d target.com -o target2.txt
    • censys search 'services.http.response.html_title: "Target"' --index-type hosts | jq -c '.[] | {ip: .ip}' | cut -d '"' -f4 > censysIP.txt
    
Directory Search

    > ./dirsearch.py -l target.txt -t -o save.txt --format=plain
    > ./dirsearch.py -l target.txt -w wordlist.txt -t -o save.txt --format=plain
    > ./dirsearch.py -l Ips.txt -t -o save.txt --format=plain
    

Scan Of Ips

    > nmap -Pn -p0-65535 -iL IPs.txt -T4 -sC -sV -vv -A --min-hostgroup 22 > nmap.txt
    > nmap -Pn -p0-65535 -iL domain.txt -T4 -sC -sV -vv -A --min-hostgroup 22 > nmap.txt
