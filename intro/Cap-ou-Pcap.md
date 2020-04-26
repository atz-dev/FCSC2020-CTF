# WRITE-UP - CAP OU PCAP

### SUBJECT

> Voici la capture d’une communication réseau entre deux postes. Un fichier a été échangé et vous devez le retrouver.
> 
> Le flag est présent dans le contenu du fichier.

In this challenge, we start with a `cap.pcap` file, which we open in `Wireshark`.  
We quickly understand that we will have to find the packet containing the file and restore it.  

### ANALYZING THE .PCAP

![pcap-analysis](/images/pcap-first-analyze.png)

Here, we can therefore see that several exchanges between the private IPs `172.20.20.132` and ` 172.20.20.133` took place via the TCP protocol.  
To reconstruct this exchange, simple thanks to this protocol, I use the Wireshark analysis tools:  
`[Analyze] -> [Follow] -> [TCP Stream]`  

![tcp-stream](/images/tcp-stream.png)

The attacker's commands, `172.20.20.133`, are in blue while the victim's responses, `172.20.20.132`, are in red.  
The attacker therefore used the `xxd` command to generate a hexdump of the `flag.zip` archive, before truncating it and sending it via `netcat` to his machine.  
We must therefore find which TCP packet contained this request in order to extract the hexdump and reconstruct the archive.  

To make our task easier, we can filter the ip with `ip.dst == 172.20.20.133` in order to have only outgoing packets towards this one, and quickly see those whose `length` is greater than the others.  

![ip-dest](/images/ip-dest.png)

Our research seems to be showing results: a packet greatly exceeds the size of its buddies with a length of 530 bytes, including 464 bytes of data.

![data-incoming](/images/data-incoming.png)
 
Here we have our .zip in `hexdump` format, we can now convert it back to its original format.  
Here's the command I used, representing the reverse of that used by the attacker at the start with the same utility `xxd`.  

```
xxd -r -p hexdump.txt > flag.zip
```

Subsequently, we unzip the archive and have a `flag.txt` file, all that remains is to display its content.  

`FCSC{6ec28b4e2b0f1bd9eb88257d650f558afec4e23f3449197b4bfc9d61810811e3}`
