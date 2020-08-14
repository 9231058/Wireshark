# Wireshark
## Introduction
Capturing packets with wireshark gives you a good insights over network layers.
`tshark.sh` is a simple hepler script for packet capturing based on `tshark`.

## Step by Step to Capture

1. Install tshark
```sh
sudo apt install tshark
```

2. Run `tshark.sh` and capture your interface and move it file into your specific directory.
3. Read the pcap file with `tshark`.

## TCP Connection to Closed Port
Here we are trying to create connetion with a closed port and as you can see we have gotten the RST.

```tshark
   57  23.910132 192.168.73.6 → 192.168.73.5 TCP 74 39404 → 23 [SYN] Seq=0 Win=29200 Len=0 MSS=1460 SACK_PERM=1 TSval=2561801316 TSecr=0 WS=128
   58  23.910186 192.168.73.5 → 192.168.73.6 TCP 54 23 → 39404 [RST, ACK] Seq=1 Ack=1 Win=0 Len=0
```

## Funny TCP Connection
One of the main points about TCP connections is their Seq and Ack numbers for each of their packet.
As may know TCP connections start with random sequence number and `tshark` changes it to zero for us.
Sequence number increases with packet lengths as you can see and the PSH flag is used to send data to the application layer immediately.

```tshark
   78  28.719289 192.168.73.6 → 192.168.73.5 TCP 74 47244 → 3000 [SYN] Seq=0 Win=29200 Len=0 MSS=1460 SACK_PERM=1 TSval=2561806125 TSecr=0 WS=128
   79  28.719357 192.168.73.5 → 192.168.73.6 TCP 74 3000 → 47244 [SYN, ACK] Seq=0 Ack=1 Win=28960 Len=0 MSS=1460 SACK_PERM=1 TSval=2184098846 TSecr=2561806125 WS=128
   80  28.719716 192.168.73.6 → 192.168.73.5 TCP 66 47244 → 3000 [ACK] Seq=1 Ack=1 Win=29312 Len=0 TSval=2561806125 TSecr=2184098846
   92  34.493881 192.168.73.6 → 192.168.73.5 TCP 92 47244 → 3000 [PSH, ACK] Seq=1 Ack=1 Win=29312 Len=26 TSval=2561811900 TSecr=2184098846
   93  34.493920 192.168.73.5 → 192.168.73.6 TCP 66 3000 → 47244 [ACK] Seq=1 Ack=27 Win=29056 Len=0 TSval=2184104621 TSecr=2561811900
  133  49.087432 192.168.73.6 → 192.168.73.5 TCP 121 47244 → 3000 [PSH, ACK] Seq=27 Ack=1 Win=29312 Len=55 TSval=2561826493 TSecr=2184104621
  134  49.087464 192.168.73.5 → 192.168.73.6 TCP 66 3000 → 47244 [ACK] Seq=1 Ack=82 Win=29056 Len=0 TSval=2184119215 TSecr=2561826493
  147  53.855036 192.168.73.6 → 192.168.73.5 TCP 66 47244 → 3000 [FIN, ACK] Seq=82 Ack=1 Win=29312 Len=0 TSval=2561831261 TSecr=2184119215
  148  53.855144 192.168.73.5 → 192.168.73.6 TCP 66 3000 → 47244 [FIN, ACK] Seq=1 Ack=83 Win=29056 Len=0 TSval=2184123983 TSecr=2561831261
  149  53.855353 192.168.73.6 → 192.168.73.5 TCP 66 47244 → 3000 [ACK] Seq=83 Ack=2 Win=29312 Len=0 TSval=2561831261 TSecr=2184123983
```
